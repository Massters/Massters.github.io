---
layout: blog
code: true
category: code
title:  "GoogleTest教程"
date:   2023-01-08
tags:
- Google Test
- C++
- Linux
---

# Google Test 教程

## 1. Google Test_1

```C++
//Tests Factorial()
//Tests factorial of negative numbers.
//This test is named "Negative", and belongs to the "FactoriaTest"
//Tests factorial of 0. 用于测试 函数 变量重用, 函数 string 辅助函数 操作 大小写转 substr去除两端空格
TEST(测试名字FactorialTest, Negative)
{
    EXPECT_EQ(1, Factorial(-5));//期望相等
    ASSERT_EQ(Factorial(-10), 1);//如果不满足,则停止,跳出测试案例
}
//如果参数太多,则可以使用参数表
```

- 参数化:

  1. 添加类 

     ```c++
     //继承自public::test::TestWithParam<T>
     class IsPrimeParamTest: public::testing::TestWithParam<int>
     ```

  2. 获取参数并测试

     ```c++
     TEST_P(IsPrimeParamTest,ExpectTrueReturn)
     {
         int n = GetParam();
         EXCEPT_TRUE(IsPrime(n));
     }
     ```

  3. 告诉gtest你的测试参数

     ```c++
     INSTANTIATE_TEST_CASE_P(TrueReturn, IsPrimeParamTest, testing::Values(3, 5, 11, 23, 17)); 
     //第3个参数还可以是
     //Range(begin, end[, step]) testing::Range(3,10,2) testing::Values(3,5,7,9)
     //ValuesIn(容器和c数组)ValuesIn(being, end) testing::ValuesIn(vector<int>)
     
     ```

- gtest事件机制,3 种事件

  - 不同:成员变量的生命周期, setup teardown的调用时机

  1. testcase

     每个案例执行前后,多次对类初始使用测试

     string 有很多方法

     一个类 多个行为 先后顺序相关

     ```c++
     class CaseTestSmpl : public testing::Test
     {
     protected:
         virtual void SetUp(){
             temp_.Init();
         } 
         virtual void TearDown(){
         	temp_.Finalize(); 
         }
         T temp_;
     }
     TEST_F(CaseTestSmpl, test_1st)
     {
         //temp_.SetUp();
         //这里可以执行使用 T以及它的内部方法
         int i = this->temp_.calc1();
         int i = this->temp_.calc2();
         
         //temp_.TearDown();
     }
     TEST_F(CaseTestSmpl, test_2nd)
     {
         //temp_.SetUp();
         //这里可以执行使用 T以及它的内部方法
         int i = this->temp_.calc2();
         int i = this->temp_.calc1();
         
         //temp_.TearDown();
     }
     ```

      

  2. testsuite

     在某一批案例中,第一个执行前到最后一个执行后

     一般用于类行为测试 或者 其他有联系的多个方法测试 cpp

     多个函数的时候 组合 变量保存整个过程 

     ```c++
     class SuiteTsetSmpl : public testing:Test
     {
     protected:
         //第一次调用前
         static void SetUpTestCase(){
             shared_ = new T;
         }
         //最后一次调用后
         static void TearDownTestCase(){
             delete shared_;
             shared_ = NULL;
         }
         static T* shared_;
     }
     TEST_F(SuiteTestSmpl, test_1st)
     {
         //这里可以引用shared_
     }
     TEST_F(SuiteTestSmpl, test_2nd)
     {
         //这里可以引用shared_
     }
     ```

     

  3. test global

     全局事件:所有案例执行前后

     可用于组合类行为测试

     继承于`testing::Environment`

     需要一个main函数.把类名注册到全局中

     ```c++
     class GlobalTestSmpl : public testing::Environment
     {
     protected:
         virtual void SetUp(){
             //准备工作
         }
         //return 之后执行清理工作
         virtual void TearDown(){
             //清理工作
         }
         int a;//全局有效,生命周期全局
     };
     
     int _tmain(int argc, char* argv[])
     {
         //注册全局事件 GlobalTestSmpl
         testing::AddGlobalTestEnvironment(new GlobalTestSmpl);
         testing::InitGoogleTest(&argc, argv);
         return RUN_ALL_TESTS();
     }
     ```

- 怎么选择事件
  - setup teardown内部调用时机
  - 不同事件类的成员变量的 生命周期

- 实际使用的问题

  ```c++
  //TEST(用例的名字，描述)
  TEST(ArrayContent, Compare){
      int a[] = {1,2,3,4,5,6};
      int b[] = {1,2,3,4,6,5};
      for(int i = 0; i < 6); i++){
          EXPECT_EQ(a[i], b[i]) << "Index: " << i;
      }
  }
  ```

## 2. 简单的测试案例

- 组成:单元测试+测试案例
- 单元测试
  - gdb:基础调试
  - 动态追踪的方式

- 什么是一个好的框架？

  1. 独立, 可重复
  2. 反映测试代码的结构,测试完备性
  3. 测试失败的时候, 尽可能提供多的错误信息, 一次测试能发现或修复多个错误
  4. 只需要关注测试本身以及自动跟踪测试,不需要枚举它们
  5. 测试简洁,高效

- 如何实现自动跟踪？

  ```c++
  std::vector<TestCase*> testcases_; //一个案例集合,一个保存的容器
  TestCase* RegisterTestCase(Test...);//一个注册测试案例
  int Run();//执行单元测试
  ```

  模块中写好的测试代码,调用注册逻辑(注册测试案例),注册逻辑把它放到容器之中

  Run函数去遍历容器,容器中的TestCase* 类中也有一个Run()函数.它里面有执行具体测试的代码

  ```c++
  //单元测试类
  class UnitTest
  {
  public:
      //获取单例
      static UnitTest* GetInstance();
      //注册测试案例
      TestCase* RegisterTestCase(TestCase* testcase);
      //执行单元测试
      int Run();
      TestCase* CurrentTestCase;//记录当前执行的测试案例
      int nTestResult;//总的执行结果
      int nPassed;//通过案例数
      int nFailed;//失败案例数
  protected:
      std::vector<TestCase*> vtestcases_;//案例集合
  }
  
  //具体测试案例
  NTEST(FooTest_FailDemo)
  {
      EXPECT_EQ(4, Foo(1,2));
      EXPECT_EQ(2, Foo(1,2));
  }
  ```

## 3. GoogleTest_2

- 组成
  - googletest
    - gtest/gtest.h:宏定义
    - libgtest.a:逻辑
    - libgtest_main.a:提供了main函数,做初始化工作
  - googlemock:打桩测试
    - gmock/gmock.h
    - libgmock.a
    - libgmock_main.a
  - gmock在gtest之上开发的,包含关系

- 层次关系

  - 单元测试:一个项目代码只有一个单元测试
    - 测试套件:具体功能
      - 具体的测试案例:具体的参数
        - 断言
        - 断言
        - 断言
        - ...
      - 测试案例
      - 测试案例
      - ...
    - 测试套件
    - 测试套件
    - ...

- 实例:

  ```c++
  //测试案例
  //TEST:宏定义
  //TEST(test_suite_name, test_name)
  //测试相同的函数 ,用相同的套件
  //测试函数的不同方向, 设计不同的测试案例
  TEST(FactorialTest, Negative)
  {
      EXPECT_EQ(1, Factorial(-5));
      EXPECT_EQ(1, Factorial(-1));
      EXPECT_GT(Factorial(-10), 0);
  }
  
  TEST(IsPrimeTest, Negative)
  {
      EXPECT_FALSE(IsPrime(-1));
      EXPECT_FALSE(IsPrime(-2));
      EXPECT_FALSE(IsPrime(INT_MIN));
  }
  TEST(IsPrimeTest, Positive)
  {
      EXPECT_FALISE(IsPrime(4));
      EXPECT_FALSE(IsPrime(5));
  }
  ```

- 断言

  - 成对出现

    - expect_true expect_false:期望的值, 出错继续运行(在一个测试案例中)
    - assert_true assert_false:出错直接报错,不继续运行

    - 一般使用Expect

  - 可以在断言后加入具体的错误信息

    ```c++
    ASSERT_TRUE(IsPrime(12)) << "12 不是质数";
    ```

  - 错误类型:

    - 致命错误:用Assert_*
    - 非致命错误:用Expect_*

- googletest 具体特性

  1. 简单测试 函数测试 类的测试

     `TEST(测试套件名, 测试案例名)`

  2. 测试夹具test fixture(一般多个测试案例之间不能共享数据,所以需要夹具)(测试案例夹共享数据的方式)

     1. 需要继承一个类 `testing::Test`,然后在里面声明一些数据

        testing::Test里面有两个虚函数:

        1. SetUp():测试套件在测试之间调用, 做初始化工作
        2. TearDown():测试套件在测试之后调用

     2. `TEST_F(测试夹具类名, 测试案例名)`

        ```c++
        //声明测试夹具
        //TEST_F(测试夹具类名, 测试案例名)
        TEST_F(QueueTestSmpl3, DefaultConstructor)
        {
            EXPECT_EQ(0u, q0_.Size());
        }
        TEST_F(QueueTestSmpl3, Dequeue)
        {
            int* n = q0_.Dequeue();
            EXPECT_TRUE(n == nullptr);//不能为NULL,#define (void*) 0
            
            n = q1.Dequeue();
            ASSERT_TRUE(n != nullptr);
            ASSERT_EQ(2, *n) << "应该要等于1";
            EXPECT_EQ(0u, q1_.Size());
            delete n;
        }
        ```

- 事件机制:可以在单元测试, 测试套件, 测试案例 前后做一些埋点工作 及断言的返回值

  - 单元测试

    ```c++
    //事件类
    class TestEventListener
    {
    public:
        virtual ~TestEventListener(){}
        //单元测试之前的回调函数
        virtual void OnTestProgramStart(const UnitTest& unit_test) = 0;
        //测试套件之前的回调函数
        virtual void OnTestSuiteStart(const TetSuite& ){}
        //测试案例之前的回调函数
        virtual void OnTestStart(const TestInfo& test_info) = 0;
        //断言结束之后的回调函数 
        virtual void OnTestPartResult(const TestPartResult& test_part_result) = 0;
    }
    ```

  - 内存泄漏检测

    - c++ new class

      1. operator new本质上malloc
      2. 在1分配内存上通过调用构造函数来初始化内存

    - 通过重载操作符检测

      ```c++
      class CLeakMem
      {
      public:
          void* operator new(size_t allocation_size)
          {
              allocated_++;//埋点
              return malloc(allocation_size);
          }
          void operator delete(void* block, size_t)
          {
              allocated_--;
              free(block);
          }
      private:
          static int allocated_;
      }
      int CLeakMem::allocated_ = 0;
      
      //检测
      class LeakChecker:public EmptyTestEventListener
      {
      private:
          //Called before a test starts
          void OnTestStart(const TestInfo&) override
          {
              initially_allocated_ = Water::allocated();
          }
          
          //Called after a test ends.
          void OnTestEnd(const TestInfo&) override
          {
              int difference = Water::allocated() - initially_allocated_;
              EXPECT_LE(difference, 0) << "Leaked" << difference << " unit(s) of class!";
          }
          
          int initially_allocated_;
      }
      ```

    - C语言怎么内存检测? hook malloc free

- google mock打桩测试

  - 什么时候使用?

    - 测试很慢, 依赖太多的库或使用昂贵的资源
    - 测试脆弱, 使用一些资源是不可靠的(例如网络)
    - 测试代码如何处理失败(例如,文件检验和错误),但不容易造成.
    - 确保模块以正确的方式与其他模块进行交互,但是很难观察到交互,因此希望看到观察行动结束时的副作用
    - 想模拟出复杂的依赖

  - 怎么使用?

    1. 怎么模拟这个行为？使用一个类模拟其他对象.

       ```c++
       //编写模拟类
       #include "gmock/gmock.h"
       class MockTurtle: public 
       {
       public:
           ...
           MOCK_METHOD0(PenUp, void());
           MOCK_METHOD0(PenDown, void());
           MOCK_METHOD1(Forward, void(int distance));
           MOCK_METHOD2(GoTo, void(int x, int y));
           MOCK_CONST_METHOD0(GetX, void());
       }
       ```

    2. 怎么出发业务？写期望行为

       - 核心思想:

       1. 调用哪些接口
       2. 调用多少次
       3. 调用顺序
       4. 接口的参数是什么
       5. 接口的返回值是什么

       ```c++
       //模拟类 继承模拟的接口
       class MockFoo : public FooInterface
       {
       public:
           MOCK_METHOD0(getArbitraryString, std::string());//0表示函数的参数为0,std;;string()是函数的返回值
           MOCK_METHOD0(getPosition, int());//0表示函数的参数为0
       };
       
       //测试流程
       int main(int argc, char** argv)
       {
           ::testing::InitGoogleMock(&argc, argv);//初始化
           int n = 100;
           string value = "Hello World!";
           MockFoo mockFoo; //初始化类
           EXPECT_CALL(mockFoo, getArbitraryString())//调用哪一些接口
               .Times(1)//调用多少次
               .WillOnce(Return(value));//接口的返回值
           string returnValue = mockFoo.getArbitrartString();
           cout << "Returned Value: " << val << endl;
           //在这里Times(2)意思是调用两次, 但是下边只调用了一次,所以会报出异常
           EXPECT_CALL(mockFoo, getPosition())
               .Time(2)
           	.WillRepeatedly(Return(n++));
           int val = mockFoo.getPosition(); //100 //在这里实际调用了1次
           cout << "Returned Value: " << val << endl;
           //getPosition指定了调用两次,这里只调用了一次,所以运行结果显示出错
           return EXIT_SUCCESS;    
       }
       
       ```

    3. 写具体测试

- 火焰图:动态追踪的方式(推荐)
- gdb:基础的调试(不推荐)