---
layout: blog
code: true
category: capnp
title: "Cap'n Proto Sample Code"
date:   2023-01-29
- cap'n proto
---

# Cap'n Proto C++ RPC sample code

## calculator-server.c++

```c++
// Copyright (c) 2013-2014 Sandstorm Development Group, Inc. and contributors
// Licensed under the MIT License:
//
// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to deal
// in the Software without restriction, including without limitation the rights
// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
// copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:
//
// The above copyright notice and this permission notice shall be included in
// all copies or substantial portions of the Software.
//
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
// THE SOFTWARE.

#include "calculator.capnp.h"
#include <kj/debug.h>
#include <capnp/ez-rpc.h>
#include <capnp/message.h>
#include <iostream>

typedef unsigned int uint;

kj::Promise<double> readValue(Calculator::Value::Client value) {
  // Helper function to asynchronously call read() on a Calculator::Value and
  // return a promise for the result.  (In the future, the generated code might
  // include something like this automatically.)\
  // 在 Calculator::Value 上异步调用 read() 并返回结果承诺的辅助函数。 （将来，生成的代码可能会自动包含类似的内容。）

  return value.readRequest().send()
      .then([](capnp::Response<Calculator::Value::ReadResults> result) {
    return result.getValue();
  });
}

kj::Promise<double> evaluateImpl(
    Calculator::Expression::Reader expression,
    capnp::List<double>::Reader params = capnp::List<double>::Reader()) {
  // Implementation of CalculatorImpl::evaluate(), also shared by
  // FunctionImpl::call().  In the latter case, `params` are the parameter
  // values passed to the function; in the former case, `params` is just an
  // empty list.
  // CalculatorImpl::evaluate() 的实现，也由 FunctionImpl::call() 共享。 在后一种情况下，“params”是传递给函数的参数值； 在前一种情况下，`params` 只是一个空列表。
  switch (expression.which()) {
    case Calculator::Expression::LITERAL:
      return expression.getLiteral();

    case Calculator::Expression::PREVIOUS_RESULT:
      return readValue(expression.getPreviousResult());

    case Calculator::Expression::PARAMETER: {
      KJ_REQUIRE(expression.getParameter() < params.size(),
                 "Parameter index out-of-range.");
      return params[expression.getParameter()];
    }

    case Calculator::Expression::CALL: {
      auto call = expression.getCall();
      auto func = call.getFunction();

      // Evaluate each parameter.
      kj::Array<kj::Promise<double>> paramPromises =
          KJ_MAP(param, call.getParams()) {
            return evaluateImpl(param, params);
          };

      // Join the array of promises into a promise for an array.
      // 将承诺数组加入数组的承诺中。
      kj::Promise<kj::Array<double>> joinedParams =
          kj::joinPromises(kj::mv(paramPromises));

      // When the parameters are complete, call the function.
      // 当参数齐全时，调用函数。
      return joinedParams.then([KJ_CPCAP(func)](kj::Array<double>&& paramValues) mutable {
        auto request = func.callRequest();
        request.setParams(paramValues);
        return request.send().then(
            [](capnp::Response<Calculator::Function::CallResults>&& result) {
          return result.getValue();
        });
      });
    }

    default:
      // Throw an exception.
      // 抛出异常。
      KJ_FAIL_REQUIRE("Unknown expression type.");
  }
}

class ValueImpl final: public Calculator::Value::Server {
  // Simple implementation of the Calculator.Value Cap'n Proto interface.
  // Calculator.Value Cap'n Proto 接口的简单实现。
public:
  ValueImpl(double value): value(value) {}

  kj::Promise<void> read(ReadContext context) {
    context.getResults().setValue(value);
    return kj::READY_NOW;
  }

private:
  double value;
};

class FunctionImpl final: public Calculator::Function::Server {
  // Implementation of the Calculator.Function Cap'n Proto interface, where the
  // function is defined by a Calculator.Expression.
  // Calculator.Function Cap'n Proto 接口的实现，其中函数由 Calculator.Expression 定义。
public:
  FunctionImpl(uint paramCount, Calculator::Expression::Reader body)
      : paramCount(paramCount) {
    this->body.setRoot(body);
  }

  kj::Promise<void> call(CallContext context) {
    auto params = context.getParams().getParams();
    KJ_REQUIRE(params.size() == paramCount, "Wrong number of parameters.");

    return evaluateImpl(body.getRoot<Calculator::Expression>(), params)
        .then([KJ_CPCAP(context)](double value) mutable {
      context.getResults().setValue(value);
    });
  }

private:
  uint paramCount;
  // The function's arity.
  // 函数的元数。

  capnp::MallocMessageBuilder body;
  // Stores a permanent copy of the function body.
  // 存储函数体的永久副本。
};

class OperatorImpl final: public Calculator::Function::Server {
  // Implementation of the Calculator.Function Cap'n Proto interface, wrapping
  // basic binary arithmetic operators.
  // Calculator.Function Cap'n Proto 接口的实现，包装基本的二进制算术运算符。
public:
  OperatorImpl(Calculator::Operator op): op(op) {}

  kj::Promise<void> call(CallContext context) {
    auto params = context.getParams().getParams();
    KJ_REQUIRE(params.size() == 2, "Wrong number of parameters.");

    double result;
    switch (op) {
      case Calculator::Operator::ADD:     result = params[0] + params[1]; break;
      case Calculator::Operator::SUBTRACT:result = params[0] - params[1]; break;
      case Calculator::Operator::MULTIPLY:result = params[0] * params[1]; break;
      case Calculator::Operator::DIVIDE:  result = params[0] / params[1]; break;
      default:
        KJ_FAIL_REQUIRE("Unknown operator.");
    }

    context.getResults().setValue(result);
    return kj::READY_NOW;
  }

private:
  Calculator::Operator op;
};

class CalculatorImpl final: public Calculator::Server {
  // Implementation of the Calculator Cap'n Proto interface.
  // 计算器 Cap'n Proto 接口的实现。
public:
  kj::Promise<void> evaluate(EvaluateContext context) override {
    return evaluateImpl(context.getParams().getExpression())
        .then([KJ_CPCAP(context)](double value) mutable {
      context.getResults().setValue(kj::heap<ValueImpl>(value));
    });
  }

  kj::Promise<void> defFunction(DefFunctionContext context) override {
    auto params = context.getParams();
    context.getResults().setFunc(kj::heap<FunctionImpl>(
        params.getParamCount(), params.getBody()));
    return kj::READY_NOW;
  }

  kj::Promise<void> getOperator(GetOperatorContext context) override {
    context.getResults().setFunc(kj::heap<OperatorImpl>(
        context.getParams().getOp()));
    return kj::READY_NOW;
  }
};

int main(int argc, const char* argv[]) {
  if (argc != 2) {
    std::cerr << "usage: " << argv[0] << " ADDRESS[:PORT]\n"
        "Runs the server bound to the given address/port.\n"
        "ADDRESS may be '*' to bind to all local addresses.\n"
        ":PORT may be omitted to choose a port automatically." << std::endl;
    return 1;
  }

  // Set up a server.
  // 设置服务器。
  capnp::EzRpcServer server(kj::heap<CalculatorImpl>(), argv[1]);

  // Write the port number to stdout, in case it was chosen automatically.
  // 将端口号写入标准输出，以防它被自动选择。
  auto& waitScope = server.getWaitScope();
  uint port = server.getPort().wait(waitScope);
  if (port == 0) {
    // The address format "unix:/path/to/socket" opens a unix domain socket,
    // in which case the port will be zero.
    // 地址格式 "unix:/path/to/socket" 打开一个 unix 域套接字，在这种情况下端口将为零。
    std::cout << "Listening on Unix socket..." << std::endl;
  } else {
    std::cout << "Listening on port " << port << "..." << std::endl;
  }

  // Run forever, accepting connections and handling requests.
  // 永远运行，接受连接和处理请求。
  kj::NEVER_DONE.wait(waitScope);
}

```

## calculator-client.c++

```c++
// Copyright (c) 2013-2014 Sandstorm Development Group, Inc. and contributors
// Licensed under the MIT License:
//
// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to deal
// in the Software without restriction, including without limitation the rights
// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
// copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:
//
// The above copyright notice and this permission notice shall be included in
// all copies or substantial portions of the Software.
//
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
// THE SOFTWARE.

#include "calculator.capnp.h"
#include <capnp/ez-rpc.h>
#include <kj/debug.h>
#include <math.h>
#include <iostream>

class PowerFunction final: public Calculator::Function::Server {
  // An implementation of the Function interface wrapping pow().  Note that
  // we're implementing this on the client side and will pass a reference to
  // the server.  The server will then be able to make calls back to the client.
  //包装 pow() 的 Function 接口的实现。 请注意，我们在客户端实现它，并将引用传递给服务器。 然后服务器将能够回叫客户端。
    
    
public:
  kj::Promise<void> call(CallContext context) {
    auto params = context.getParams().getParams();
    KJ_REQUIRE(params.size() == 2, "Wrong number of parameters.");
    context.getResults().setValue(pow(params[0], params[1]));
    return kj::READY_NOW;
  }
};

int main(int argc, const char* argv[]) {
  if (argc != 2) {
    std::cerr << "usage: " << argv[0] << " HOST:PORT\n"
        "Connects to the Calculator server at the given address and "
        "does some RPCs." << std::endl;
    return 1;
  }

  capnp::EzRpcClient client(argv[1]);
  Calculator::Client calculator = client.getMain<Calculator>();

  // Keep an eye on `waitScope`.  Whenever you see it used is a place where we
  // stop and wait for the server to respond.  If a line of code does not use
  // `waitScope`, then it does not block!
    
  // 密切关注“waitScope”。 每当您看到它被使用时，我们就会停下来等待服务器响应。 如果一行代码没有使用 `waitScope`，那么它就不会阻塞！
  auto& waitScope = client.getWaitScope();

  {
    // Make a request that just evaluates the literal value 123.
    //
    // What's interesting here is that evaluate() returns a "Value", which is
    // another interface and therefore points back to an object living on the
    // server.  We then have to call read() on that object to read it.
    // However, even though we are making two RPC's, this block executes in
    // *one* network round trip because of promise pipelining:  we do not wait
    // for the first call to complete before we send the second call to the
    // server.
	//发出一个只计算字面值 123 的请求。
    
    //里有趣的是 evaluate() 返回一个“值”，这是另一个接口，因此指向服务器上的一个对象。 然后我们必须调用该对象的 read() 来读取它。
    //而，即使我们正在创建两个 RPC，由于承诺流水线，这个块在*一个*网络往返中执行：我们不会等待第一个调用完成，然后再将第二个调用发送到服务器。
    std::cout << "Evaluating a literal... ";
    std::cout.flush();

    // Set up the request.// 设置请求。
    auto request = calculator.evaluateRequest();
    request.getExpression().setLiteral(123);

    // Send it, which returns a promise for the result (without blocking).
    // 发送它，返回对结果的承诺（无阻塞）。
    auto evalPromise = request.send();

    // Using the promise, create a pipelined request to call read() on the
    // returned object, and then send that.
	// 使用 promise，创建一个管道请求以在返回的对象上调用 read()，然后发送它。
    auto readPromise = evalPromise.getValue().readRequest().send();

    // Now that we've sent all the requests, wait for the response.  Until this
    // point, we haven't waited at all!
    // 现在我们已经发送了所有请求，等待响应。 到此为止，我们根本就等不及了！
    auto response = readPromise.wait(waitScope);
    KJ_ASSERT(response.getValue() == 123);

    std::cout << "PASS" << std::endl;
  }

  {
    // Make a request to evaluate 123 + 45 - 67.
    //
    // The Calculator interface requires that we first call getOperator() to
    // get the addition and subtraction functions, then call evaluate() to use
    // them.  But, once again, we can get both functions, call evaluate(), and
    // then read() the result -- four RPCs -- in the time of *one* network
    // round trip, because of promise pipelining.

      
    //提出评估 123 + 45 - 67 的请求。
    
    //lculator 接口要求我们首先调用 getOperator() 获取加法和减法函数，然后调用 evaluate() 来使用它们。 但是，再一次，我们可以获得两个函数，调用 evaluate()，然后 read() 结果——四个 RPC——在 *一个* 网络往返的时间内，因为承诺流水线。
    std::cout << "Using add and subtract... ";
    std::cout.flush();

    Calculator::Function::Client add = nullptr;
    Calculator::Function::Client subtract = nullptr;

    {
      // Get the "add" function from the server.
      // 从服务器获取“add”函数。
      auto request = calculator.getOperatorRequest();
      request.setOp(Calculator::Operator::ADD);
      add = request.send().getFunc();
    }

    {
      // Get the "subtract" function from the server.
      // 从服务器获取“减法”函数。
      auto request = calculator.getOperatorRequest();
      request.setOp(Calculator::Operator::SUBTRACT);
      subtract = request.send().getFunc();
    }

    // Build the request to evaluate 123 + 45 - 67.
    // 构建评估 123 + 45 - 67 的请求。
    auto request = calculator.evaluateRequest();

    auto subtractCall = request.getExpression().initCall();
    subtractCall.setFunction(subtract);
    auto subtractParams = subtractCall.initParams(2);
    subtractParams[1].setLiteral(67);

    auto addCall = subtractParams[0].initCall();
    addCall.setFunction(add);
    auto addParams = addCall.initParams(2);
    addParams[0].setLiteral(123);
    addParams[1].setLiteral(45);

    // Send the evaluate() request, read() the result, and wait for read() to
    // finish.
    //  发送 evaluate() 请求，read() 结果，等待 read() 完成。
    auto evalPromise = request.send();
    auto readPromise = evalPromise.getValue().readRequest().send();

    auto response = readPromise.wait(waitScope);
    KJ_ASSERT(response.getValue() == 101);

    std::cout << "PASS" << std::endl;
  }

  {
    // Make a request to evaluate 4 * 6, then use the result in two more
    // requests that add 3 and 5.
    //
    // Since evaluate() returns its result wrapped in a `Value`, we can pass
    // that `Value` back to the server in subsequent requests before the first
    // `evaluate()` has actually returned.  Thus, this example again does only
    // one network round trip.
	// 发出计算 4 * 6 的请求，然后在另外两个将 3 和 5 相加的请求中使用结果。
    
    // 由于 evaluate() 返回其包含在 Value 中的结果，我们可以在第一个 `evaluate()` 实际返回之前在后续请求中将该 `Value` 传回服务器。 因此，这个例子同样只进行一次网络往返。
    std::cout << "Pipelining eval() calls... ";
    std::cout.flush();

    Calculator::Function::Client add = nullptr;
    Calculator::Function::Client multiply = nullptr;

    {
      // Get the "add" function from the server.
      auto request = calculator.getOperatorRequest();
      request.setOp(Calculator::Operator::ADD);
      add = request.send().getFunc();
    }

    {
      // Get the "multiply" function from the server.
      auto request = calculator.getOperatorRequest();
      request.setOp(Calculator::Operator::MULTIPLY);
      multiply = request.send().getFunc();
    }

    // Build the request to evaluate 4 * 6
    auto request = calculator.evaluateRequest();

    auto multiplyCall = request.getExpression().initCall();
    multiplyCall.setFunction(multiply);
    auto multiplyParams = multiplyCall.initParams(2);
    multiplyParams[0].setLiteral(4);
    multiplyParams[1].setLiteral(6);

    auto multiplyResult = request.send().getValue();

    // Use the result in two calls that add 3 and add 5.

    auto add3Request = calculator.evaluateRequest();
    auto add3Call = add3Request.getExpression().initCall();
    add3Call.setFunction(add);
    auto add3Params = add3Call.initParams(2);
    add3Params[0].setPreviousResult(multiplyResult);
    add3Params[1].setLiteral(3);
    auto add3Promise = add3Request.send().getValue().readRequest().send();

    auto add5Request = calculator.evaluateRequest();
    auto add5Call = add5Request.getExpression().initCall();
    add5Call.setFunction(add);
    auto add5Params = add5Call.initParams(2);
    add5Params[0].setPreviousResult(multiplyResult);
    add5Params[1].setLiteral(5);
    auto add5Promise = add5Request.send().getValue().readRequest().send();

    // Now wait for the results.
    KJ_ASSERT(add3Promise.wait(waitScope).getValue() == 27);
    KJ_ASSERT(add5Promise.wait(waitScope).getValue() == 29);

    std::cout << "PASS" << std::endl;
  }

  {
    // Our calculator interface supports defining functions.  Here we use it
    // to define two functions and then make calls to them as follows:
    //
    //   f(x, y) = x * 100 + y
    //   g(x) = f(x, x + 1) * 2;
    //   f(12, 34)
    //   g(21)
    //
    // Once again, the whole thing takes only one network round trip.
	// 我们的计算器界面支持定义函数。 这里我们使用它
    // 定义两个函数，然后按如下方式调用它们：
    
    //   f(x, y) = x * 100 + y
    //   g(x) = f(x, x + 1) * 2；
    //   f(12, 34)
    //   g(21)
    
    // 再一次，整个过程只需要一次网络往返。
    std::cout << "Defining functions... ";
    std::cout.flush();

    Calculator::Function::Client add = nullptr;
    Calculator::Function::Client multiply = nullptr;
    Calculator::Function::Client f = nullptr;
    Calculator::Function::Client g = nullptr;

    {
      // Get the "add" function from the server.
      auto request = calculator.getOperatorRequest();
      request.setOp(Calculator::Operator::ADD);
      add = request.send().getFunc();
    }

    {
      // Get the "multiply" function from the server.
      auto request = calculator.getOperatorRequest();
      request.setOp(Calculator::Operator::MULTIPLY);
      multiply = request.send().getFunc();
    }

    {
      // Define f.
      auto request = calculator.defFunctionRequest();
      request.setParamCount(2);

      {
        // Build the function body.
        auto addCall = request.getBody().initCall();
        addCall.setFunction(add);
        auto addParams = addCall.initParams(2);
        addParams[1].setParameter(1);  // y

        auto multiplyCall = addParams[0].initCall();
        multiplyCall.setFunction(multiply);
        auto multiplyParams = multiplyCall.initParams(2);
        multiplyParams[0].setParameter(0);  // x
        multiplyParams[1].setLiteral(100);
      }

      f = request.send().getFunc();
    }

    {
      // Define g.
      auto request = calculator.defFunctionRequest();
      request.setParamCount(1);

      {
        // Build the function body.
        auto multiplyCall = request.getBody().initCall();
        multiplyCall.setFunction(multiply);
        auto multiplyParams = multiplyCall.initParams(2);
        multiplyParams[1].setLiteral(2);

        auto fCall = multiplyParams[0].initCall();
        fCall.setFunction(f);
        auto fParams = fCall.initParams(2);
        fParams[0].setParameter(0);

        auto addCall = fParams[1].initCall();
        addCall.setFunction(add);
        auto addParams = addCall.initParams(2);
        addParams[0].setParameter(0);
        addParams[1].setLiteral(1);
      }

      g = request.send().getFunc();
    }

    // OK, we've defined all our functions.  Now create our eval requests.
	// 好的，我们已经定义了所有的函数。 现在创建我们的评估请求。
    // f(12, 34)
    auto fEvalRequest = calculator.evaluateRequest();
    auto fCall = fEvalRequest.initExpression().initCall();
    fCall.setFunction(f);
    auto fParams = fCall.initParams(2);
    fParams[0].setLiteral(12);
    fParams[1].setLiteral(34);
    auto fEvalPromise = fEvalRequest.send().getValue().readRequest().send();

    // g(21)
    auto gEvalRequest = calculator.evaluateRequest();
    auto gCall = gEvalRequest.initExpression().initCall();
    gCall.setFunction(g);
    gCall.initParams(1)[0].setLiteral(21);
    auto gEvalPromise = gEvalRequest.send().getValue().readRequest().send();

    // Wait for the results.
    KJ_ASSERT(fEvalPromise.wait(waitScope).getValue() == 1234);
    KJ_ASSERT(gEvalPromise.wait(waitScope).getValue() == 4244);

    std::cout << "PASS" << std::endl;
  }

  {
    // Make a request that will call back to a function defined locally.
    //
    // Specifically, we will compute 2^(4 + 5).  However, exponent is not
    // defined by the Calculator server.  So, we'll implement the Function
    // interface locally and pass it to the server for it to use when
    // evaluating the expression.
    //
    // This example requires two network round trips to complete, because the
    // server calls back to the client once before finishing.  In this
    // particular case, this could potentially be optimized by using a tail
    // call on the server side -- see CallContext::tailCall().  However, to
    // keep the example simpler, we haven't implemented this optimization in
    // the sample server.
	// 发出将回调本地定义的函数的请求。
    
    // 具体来说，我们将计算 2^(4 + 5)。 但是，指数不是由计算器服务器定义的。 因此，我们将在本地实现 Function 接口并将其传递给服务器以供其在计算表达式时使用。
    
    // 此示例需要两次网络往返才能完成，因为服务器在完成之前回调一次客户端。 在这种特殊情况下，这可能会通过在服务器端使用尾调用来优化——请参阅 CallContext::tailCall()。 但是，为了使示例更简单，我们没有在示例服务器中实现此优化。
    std::cout << "Using a callback... ";
    std::cout.flush();

    Calculator::Function::Client add = nullptr;

    {
      // Get the "add" function from the server.
      auto request = calculator.getOperatorRequest();
      request.setOp(Calculator::Operator::ADD);
      add = request.send().getFunc();
    }

    // Build the eval request for 2^(4+5).
    auto request = calculator.evaluateRequest();

    auto powCall = request.getExpression().initCall();
    powCall.setFunction(kj::heap<PowerFunction>());
    auto powParams = powCall.initParams(2);
    powParams[0].setLiteral(2);

    auto addCall = powParams[1].initCall();
    addCall.setFunction(add);
    auto addParams = addCall.initParams(2);
    addParams[0].setLiteral(4);
    addParams[1].setLiteral(5);

    // Send the request and wait.
    auto response = request.send().getValue().readRequest()
                           .send().wait(waitScope);
    KJ_ASSERT(response.getValue() == 512);

    std::cout << "PASS" << std::endl;
  }

  return 0;
}

```

## calculator.capnp

```c++
# Copyright (c) 2013-2014 Sandstorm Development Group, Inc. and contributors
# Licensed under the MIT License:
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.

@0x85150b117366d14b;

interface Calculator {
  # A "simple" mathematical calculator, callable via RPC.
  #
  # But, to show off Cap'n Proto, we add some twists:
  #
  # - You can use the result from one call as the input to the next
  #   without a network round trip.  To accomplish this, evaluate()
  #   returns a `Value` object wrapping the actual numeric value.
  #   This object may be used in a subsequent expression.  With
  #   promise pipelining, the Value can actually be used before
  #   the evaluate() call that creates it returns!
  #
  # - You can define new functions, and then call them.  This again
  #   shows off pipelining, but it also gives the client the
  #   opportunity to define a function on the client side and have
  #   the server call back to it.
  #
  # - The basic arithmetic operators are exposed as Functions, and
  #   you have to call getOperator() to obtain them from the server.
  #   This again demonstrates pipelining -- using getOperator() to
  #   get each operator and then using them in evaluate() still
  #   only takes one network round trip.
  // 一个“简单”的数学计算器，可通过 RPC 调用。
  
  // 但是，为了炫耀 Cap'n Proto，我们添加了一些曲折：
  // - 您可以将一个调用的结果用作下一个调用的输入，而无需网络往返。 为此，evaluate() 返回一个包含实际数值的“Value”对象。 该对象可以在后续表达式中使用。 使用 promise 流水线，Value 实际上可以在创建它的 evaluate() 调用返回之前使用！
  // - 您可以定义新函数，然后调用它们。 这再次展示了流水线，但它也让客户端有机会在客户端定义一个函数并让服务器回调它。
  // - 基本算术运算符作为函数公开，您必须调用 getOperator() 从服务器获取它们。 这再次演示了流水线——使用 getOperator() 获取每个运算符，然后在 evaluate() 中使用它们仍然只需要一次网络往返。
  evaluate @0 (expression :Expression) -> (value :Value);
  # Evaluate the given expression and return the result.  The
  # result is returned wrapped in a Value interface so that you
  # may pass it back to the server in a pipelined request.  To
  # actually get the numeric value, you must call read() on the
  # Value -- but again, this can be pipelined so that it incurs
  # no additional latency.

  //计算给定的表达式并返回结果。 结果被包装在 Value 接口中返回，以便您可以在管道请求中将其传回服务器。 要实际获取数值，您必须对 Value 调用 read() —— 但同样，这可以通过流水线进行处理，因此不会产生额外的延迟。
    
  struct Expression {
    # A numeric expression.
	// # 一个数值表达式。
    union {
      literal @0 :Float64;
      # A literal numeric value.
      // # 文字数值。

      previousResult @1 :Value;
      # A value that was (or, will be) returned by a previous
      # evaluate().
      //# 之前的 evaluate() 返回的值（或将要返回的值）。

      parameter @2 :UInt32;
      # A parameter to the function (only valid in function bodies;
      # see defFunction).
      //# 函数的参数（仅在函数体中有效；参见 defFunction）。

      call :group {
        # Call a function on a list of parameters.
        // # 在参数列表上调用函数。
        function @3 :Function;
        params @4 :List(Expression);
      }
    }
  }

  interface Value {
    # Wraps a numeric value in an RPC object.  This allows the value
    # to be used in subsequent evaluate() requests without the client
    # waiting for the evaluate() that returns the Value to finish.

    //在 RPC 对象中包装一个数值。 这允许在后续的 evaluate() 请求中使用该值，而无需客户端等待返回值的 evaluate() 完成。
      
    read @0 () -> (value :Float64);
    # Read back the raw numeric value.
    // # 读回原始数值。
  }

  defFunction @1 (paramCount :Int32, body :Expression)
              -> (func :Function);
  # Define a function that takes `paramCount` parameters and returns the
  # evaluation of `body` after substituting these parameters.

  //定义一个函数，该函数采用 paramCount 参数并在替换这些参数后返回对 body 的评估。
    
  interface Function {
    # An algebraic function.  Can be called directly, or can be used inside
    # an Expression.
    #
    # A client can create a Function that runs on the server side using
    # `defFunction()` or `getOperator()`.  Alternatively, a client can
    # implement a Function on the client side and the server will call back
    # to it.  However, a function defined on the client side will require a
    # network round trip whenever the server needs to call it, whereas
    # functions defined on the server and then passed back to it are called
    # locally.

    //一个代数函数。 可以直接调用，也可以在表达式内部使用。 客户端可以使用“defFunction()”或“getOperator()”创建在服务器端运行的函数。 或者，客户端可以在客户端实现一个函数，服务器将回调它。 但是，只要服务器需要调用它，在客户端定义的函数将需要网络往返，而在服务器上定义然后传递回它的函数是在本地调用的。
      
    call @0 (params :List(Float64)) -> (value :Float64);
    # Call the function on the given parameters.
    // # 根据给定的参数调用函数。
  }

  getOperator @2 (op :Operator) -> (func :Function);
  # Get a Function representing an arithmetic operator, which can then be
  # used in Expressions.

  //获取表示算术运算符的函数，然后可以在表达式中使用它。
    
  enum Operator {
    add @0;
    subtract @1;
    multiply @2;
    divide @3;
  }
}

```

> [samples code](https://github.com/capnproto/capnproto/tree/master/c%2B%2B/samples)