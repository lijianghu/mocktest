

- Mockito简介
- 什么是mock？
- 在软件开发的世界之外, "mock"一词是指模仿或者效仿。 因此可以将“mock”理解为一个替身，替代者. 在软件开发中提及"mock"，通常理解为模拟对象或者Fake。
- 
- 为什么需要Mock?
- Mock是为了解决units之间由于耦合而难于被测试的问题。所以mock object是unit test的一部分。
- 
- Mock的好处是什么?
- 
- 提前创建测试，TDD（测试驱动开发）
- 
- 这是个最大的好处吧。如果你创建了一个Mock那么你就可以在service接口创建之前写Service Tests了，这样你就能在开发过程中把测试添加到你的自动化测试环境中了。换句话说，模拟使你能够使用测试驱动开发。
- 团队可以并行工作
- 
- 这类似于上面的那点；为不存在的代码创建测试。但前面讲的是开发人员编写测试程序，这里说的是测试团队来创建。当还没有任何东西要测的时候测试团队如何来创建测试呢？模拟并针对模拟测试！这意味着当service借口需要测试时，实际上QA团队已经有了一套完整的测试组件；没有出现一个团队等待另一个团队完成的情况。这使得模拟的效益型尤为突出了。
- 你可以创建一个验证或者演示程序。
- 
- 由于Mocks非常高效，Mocks可以用来创建一个概念证明，作为一个示意图，或者作为一个你正考虑构建项目的演示程序。这为你决定项目接下来是否要进行提供了有力的基础，但最重要的还是提供了实际的设计决策。
- 为无法访问的资源编写测试
- 
- 这个好处不属于实际效益的一种，而是作为一个必要时的“救生圈”。有没有遇到这样的情况？当你想要测试一个service接口，但service需要经过防火墙访问，防火墙不能为你打开或者你需要认证才能访问。遇到这样情况时，你可以在你能访问的地方使用MockService替代，这就是一个“救生圈”功能。
- Mock 可以交给用户
- 
- 在有些情况下，某种原因你需要允许一些外部来源访问你的测试系统，像合作伙伴或者客户。这些原因导致别人也可以访问你的敏感信息，而你或许只是想允许访问部分测试环境。在这种情况下，如何向合作伙伴或者客户提供一个测试系统来开发或者做测试呢？最简单的就是提供一个mock，无论是来自于你的网络或者客户的网络。soapUI mock非常容易配置，他可以运行在soapUI或者作为一个war包发布到你的java服务器里面。
- 隔离系统
- 
- 有时，你希望在没有系统其他部分的影响下测试系统单独的一部分。由于其他系统部分会给测试数据造成干扰，影响根据数据收集得到的测试结论。使用mock你可以移除掉除了需要测试部分的系统依赖的模拟。当隔离这些mocks后，mocks就变得非常简单可靠，快速可预见。这为你提供了一个移除了随机行为，有重复模式并且可以监控特殊系统的测试环境。
Mockito使用示例

########作者：流水不腐小夏
########链接：http://www.jianshu.com/p/77db26b4fb54
########來源：简书
########著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



```java

public class 测试mock {
  @Mock
  Pies mock;

  LinkedList mockedList = mock(LinkedList.class);

  @Test
  public void t01模拟对象() {
    // TODO 1 模拟对象
    // 模拟LinkedList 的一个对象
    // 此时调用get方法，会返回null，因为还没有对方法调用的返回值做模拟
    System.out.println(mockedList.get(0));
  }

  @Test
  public void t02模拟方法调用的返回值() {
    // TODO 2 模拟方法调用的返回值
    // 模拟获取第一个元素时，返回字符串first。 给特定的方法调用返回固定值在官方说法中称为stub。
    when(mockedList.get(0)).thenReturn("first");
    when(mockedList.get(1)).thenReturn("second");
    // when(mockedList.get(anyInt())).thenReturn("any");
    // 此时打印输出first
    System.out.println(mockedList.get(0));
  }

  @Test
  public void t03模拟方法调用抛出异常() {
    // TODO 3 模拟方法调用抛出异常
    // 模拟获取第二个元素时，抛出RuntimeException
    when(mockedList.get(1)).thenThrow(new RuntimeException());
    // 此时将会抛出RuntimeException
    System.out.println(mockedList.get(1));
  }

  @Test
  public void t04无返回值类型那么可以使用此方法模拟异常抛出() {
    // TODO 4 如果一个函数没有返回值类型，那么可以使用此方法模拟异常抛出
    doThrow(new RuntimeException("clear exception")).when(mockedList).clear();
    mockedList.clear();
  }

  @Test
  public void t05模拟调用方法时的参数匹配() {
    // TODO 5 模拟调用方法时的参数匹配
    // anyInt()匹配任何int参数，这意味着参数为任意值，其返回值均是element
    when(mockedList.get(anyInt())).thenReturn("element");
    // 此时打印是element
    System.out.println(mockedList.get(999));
  }

  @Test
  public void t06模拟调用方法调用次数() {
    // TODO 6 模拟调用方法调用次数
    System.out.println("模拟调用方法调用次数");
    // 调用add一次
    mockedList.add("once");
    // 下面两个写法验证效果一样，均验证add方法是否被调用了一次
    verify(mockedList).add("once");
    verify(mockedList, times(1)).add("once");
  }

  @Test
  public void t02校验行为() {
    // TODO 7 校验行为
    // mock creation
    List mockedList = mock(List.class);
    // using mock object
    mockedList.add("one");
    mockedList.clear();
    // verification
    verify(mockedList).add("one");
    verify(mockedList).clear();
  }
  @Test
  public void t08模拟方法调用() {
    // TODO 模拟方法调用（Stubbing）
    // You can mock concrete classes, not just interfaces
    LinkedList mockedList = mock(LinkedList.class);
    // stubbing
    when(mockedList.get(0)).thenReturn("first");
    when(mockedList.get(1)).thenThrow(new RuntimeException("抛出一个异常！"));
    // following prints "first"
    System.out.println(mockedList.get(0));
    // following throws runtime exception
    System.out.println(mockedList.get(1));
    // following prints "null" because get(999) was not stubbed
    System.out.println(mockedList.get(999));
    verify(mockedList).get(0);
  }

  @Test
  public void t09参数匹配() {
    // TODO 参数匹配 ?
    // stubbing using built-in anyInt() argument matcher
    when(mockedList.get(anyInt())).thenReturn("element");
    // stubbing using custom matcher (let's say isValid() returns your own
    // matcher implementation):
    // when(mockedList.contains(argThat(isValid()))).thenReturn("element");
    // following prints "element"
    System.out.println(mockedList.get(999));
    // you can also verify using an argument matcher
    verify(mockedList).get(anyInt());
    // argument matchers can also be written as Java 8 Lambdas
    verify(mockedList).add("someString -> someString.length() > 5");

  }

  @Test
  public void t10校验方法调用次数() {
    // TODO 校验方法调用次数
    // using mock
    mockedList.add("once");

    mockedList.add("twice");
    mockedList.add("twice");

    mockedList.add("three times");
    mockedList.add("three times");
    mockedList.add("three times");
    // following two verifications work exactly the same - times(1) is used by
    // default
    verify(mockedList).add("once");
    verify(mockedList, times(1)).add("once");
    // exact number of invocations verification
    verify(mockedList, times(2)).add("twice");
    verify(mockedList, times(3)).add("three times");
    // verification using never(). never() is an alias to times(0)
    verify(mockedList, never()).add("never happened");
    // verification using atLeast()/atMost()
    verify(mockedList, atLeastOnce()).add("three times");
    verify(mockedList, atLeast(2)).add("five times");// 这个会异常
    verify(mockedList, atMost(5)).add("three times");
  }

  @SuppressWarnings("unchecked")
  @Test
  public void t11校验方法调用顺序() {
    /*
     * // A. Single mock whose methods must be invoked in a particular order
     * List singleMock = mock(List.class); // using a single mock
     * singleMock.add("was added first"); singleMock.add("was added second"); //
     * create an inOrder verifier for a single mock InOrder inOrder =
     * inOrder(singleMock); // following will make sure that add is first called
     * with // "was added first, then with "was added second"
     * inOrder.verify(singleMock).add("was added first");
     * inOrder.verify(singleMock).add("was added second");
     */

    // B. Multiple mocks that must be used in a particular order
    List<String> firstMock = mock(List.class);
    List<String> secondMock = mock(List.class);
    // using mocks
    firstMock.add("was called first");
    secondMock.add("was called second");
    // create inOrder object passing any mocks that need to be verified in order
    InOrder inOrder = inOrder(firstMock, secondMock);
    // following will make sure that firstMock was called before secondMock
    inOrder.verify(firstMock).add("was called first");
    inOrder.verify(secondMock).add("was called second");
    // Oh, and A + B can be mixed together at will

  }

  @Test
  public void t12校验方法是否从未调用() {
    // using mocks - only mockOne is interacted
    List<String> mockOne = mock(List.class);
    List<String> mockTwo = mock(List.class);
    List<String> mockThree = mock(List.class);
    mockOne.add("one");
    // ordinary verification
    verify(mockOne).add("one");
    // verify that method was never called on a mock
    verify(mockOne, never()).add("two");
    // verify that other mocks were not interacted
    verifyZeroInteractions(mockTwo, mockThree);
  }

  @Test
  public void t13快速创建Mock对象() {
    // public class ArticleManagerTest {
    // @Mock private ArticleCalculator calculator;
    // @Mock private ArticleDatabase database;
    // @Mock private UserProvider userProvider;
    // @Before
    // public void before(){
    // MockitoAnnotations.initMocks(this);
    // }
    // }

  }

  @Test
  public void t14自定义返回不同结果() {
    when(mock.someMethod("some arg")).thenThrow(new RuntimeException()) // 第一次会抛出异常
        .thenReturn("foo"); // 第二次会返回这个结果
    // First call: throws runtime exception:
    mock.someMethod("some arg"); // 第一次
    // Second call: prints "foo"
    System.out.println(mock.someMethod("some arg")); // 第二次
    // Any consecutive call: prints "foo" as well (last stubbing wins).
    System.out.println(mock.someMethod("some arg")); // 第n次(n> 2)，依旧以最后返回最后一个配置

  }

  @Test
  public void t15对返回结果进行拦截() {
    when(mock.someMethod(anyString())).thenAnswer(new Answer() {
      @Override
      public Object answer(InvocationOnMock invocation) {
        Object[] args = invocation.getArguments();
        Object mock = invocation.getMock();
        return "called with arguments: " + args;
      }
    });
    // the following prints "called with arguments: foo"
    System.out.println(mock.someMethod("foo"));
  }

  @Test
  public void t16Mock函数操作() {
    // 可以通过doThrow(), doAnswer(), doNothing(), doReturn() and doCallRealMethod()
    // 来自定义函数操作。
  }

  @Test
  public void t17暗中调用真实对象() {
    List list = new LinkedList();
    List spy = spy(list);
    // optionally, you can stub out some methods:
    when(spy.size()).thenReturn(100);
    // using the spy calls *real* methods
    spy.add("one");
    spy.add("two");
    // prints "one" - the first element of a list
    System.out.println(spy.get(0));
    // size() method was stubbed - 100 is printed
    System.out.println(spy.size());
    // optionally, you can verify
    verify(spy).add("one");
    verify(spy).add("two");
  }

  @Test
  public void t18改变默认返回值() {
    Foo mock = mock(Foo.class, Mockito.RETURNS_SMART_NULLS);
    Foo mockTwo = mock(Foo.class, new YourOwnAnswer());
  }

  @Test
  public void t19捕获函数的参数值() {
    ArgumentCaptor<Person> argument = ArgumentCaptor.forClass(Person.class);
    verify(mock).doSomething(argument.capture());
    assertEquals("John", argument.getValue().getName());
  }

  @Test
  public void t20部分Mock() {
    // you can create partial mock with spy() method:
    List list = spy(new LinkedList());
    // you can enable partial mock capabilities selectively on mocks:
    Foo mock = mock(Foo.class);
    // Be sure the real implementation is 'safe'.
    // If real implementation throws exceptions or depends on specific state of
    // the object then you're in trouble.
    when(mock.someMethod()).thenCallRealMethod();

  }

  @Test
  public void t21重置Mock() {
    List mock = mock(List.class);
    when(mock.size()).thenReturn(10);
    mock.add(1);
    reset(mock);
    // at this point the mock forgot any interactions & stubbing

  }

  @Test
  public void t22序列化() {
    List<Object> list = new ArrayList<Object>();
    List<Object> spy = mock(ArrayList.class, withSettings().spiedInstance(list)
        .defaultAnswer(CALLS_REAL_METHODS).serializable());
  }

  @Test
  public void t23检查超时() {
    // passes when someMethod() is called within given time span
    verify(mock, timeout(100)).someMethod();
    // above is an alias to:
    verify(mock, timeout(100).times(1)).someMethod();
    // passes when som`eMethod() is called *exactly* 2 times within given time
    // span
    verify(mock, timeout(100).times(2)).someMethod();
    // passes when someMethod() is called *at least* 2 times within given time
    // span
    verify(mock, timeout(100).atLeast(2)).someMethod();
    // verifies someMethod() within given time span using given verification
    // mode
    // useful only if you have your own custom verification modes.
    verify(mock, new Timeout(100, yourOwnVerificationMode)).someMethod();
  }

  @Test
  public void t24Mock详情() {
    Mockito.mockingDetails(someObject).isMock();
    Mockito.mockingDetails(someObject).isSpy();
  }
}

```
