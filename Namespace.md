### 1. 네임스페이스(Namespace)
 
    C# 은 C / C++ 와는 달리 컴포넌트 기반의 언어입니다. 그래서 여러개의 컴포넌트를 이용해서 하나의
    프로그램을 만드는 경우가 많은데 이 컴포넌트들을 모두 한명의 개발자가 만들수도 있지만, 다른 
    개발자가 제공하는 컴포넌트를 사용하거나 시스템에서 제공하는 컴포넌트를 사용해서 개발하는 
    경우도 많습니다.
 
    그렇기 때문에 서로 다른 컴포넌트에 존재하는 클래스명이 우연하게 일치하여 오류가 발생할수
    있는데, 해당 컴포넌트가 자신이 개발한 것이 아니라면 클래스명을 수정할수 없어서 문제가 될수도
    있습니다. 예를들어, TipsExam 과 유사한 작업을 하는 클래스가 필요하여 아래와 같이 TipsExam
    이라는 클래스를 추가하면 기존 클래스와 이름이 중복되어 오류가 발생하게 됩니다.
 
    class TipsExam
    {
        public static void Main()
        {
            System.Console.WriteLine("Hello, World");
        }
    }
 
    class TipsExam
    {
        public void Test()
        {
            System.Console.WriteLine("Hello, World");
        }
    }
   
    그래서 이러한 문제를 줄이고자 C# 에서는 역할이나 의미가 유사한 클래스들을 그룹지을수 있는
    기술을 제공하는데 이것이 네임스페이스 입니다. 물론, 클래스에 다른 클래스를 포함시켜서
    그룹지을수도 있지만, 이것은 실제 수행에 영향을 미치기 때문에 단순히 문법상에 코드 충돌을
    방지하는 네임스페이스와는 차이가 있습니다.
      
    위에서 사용한 코드를 네임스페이스를 사용하여 오류가 나지 않도록 재구성해보면 아래와 같습니다.
    즉, 같은 이름을 가진 클래스가 존재하더라도 네임스페이스에 의해서 구별되어지기 때문에 오류가
    발생하지 않는것입니다.
    ( 네임스페이스를 사용한다고해서 코드 수행 능력이 떨어지는것은 아닙니다. )
 
    namespace Ex1
    {
        class TipsExam
        {
            public static void Main()
            {
                System.Console.WriteLine("Hello, World");
            }
        }
    }
 
    namespace Ex2
    {
        class TipsExam
        {
            public void Test()
            {
                System.Console.WriteLine("Hello, World");
            }
        }
    }
 
    이렇게 TipsExam 클래스를 Ex1 이라는 네임스페이스에 포함시키면 네임스페이스를 명시하지
    않고서는 TipsExam 클래스를 사용할수 없습니다. 위와 같이 두개의 네임스페이스로 나누어진
    상태에서 다른 네임스페이스에 포함된 클래스에 어떻게 접근하는지 알아보도록 하겠습니다.
 
    namespace Ex1
    {
        class TipsExam
        {
            public static void Main()
            {
                // 네임스페이스가 명시되지 않으면 현재 네임스페이스를 우선으로
                //  처리하기 때문에 Ex1 에 존재하는 TipsExam 클래스 객체를 생성한다.
                TipsExam data1 = new TipsExam();
                // Ex1 에 소속된 TipsExam 에는 Test 라는 함수가 없기 때문에 오류가 발생한다.
                data1.Test();
            }
        }
    }
 
    namespace Ex2
    {
        class TipsExam
        {
            public void Test()
            {
                System.Console.WriteLine("Hello, World");
            }
        }
    }
 
    위 예제의 의도는 Ex1 에 포함된 TipsExam 클래스의 Main 함수에서 Ex2 에 포함된 TipsExam
    클래스의 Test 함수를 사용하기 위함이였습니다. 그러나 네임스페이스를 명시하지 않아서
    Ex2 의 TipsExam 클래스가 아닌 Ex1 의 TipsExam 클래스가 사용되어 문제가 발생한것입니다.
 
    따라서 본래의 의도대로 처리하기 위해서는 네임스페이스를 직접적으로 명시해야 합니다.
    정상적으로 네임스페이스 안에 정의된 클래스를 사용하려면 ' . ' 연산자를 이용하여 네임스페이스를
    클래스명 앞에 명시해주어야 합니다. 위의 코드는 아래와 같이 수정할 수 있습니다.
 
    namespace Ex1
    {
        class TipsExam
        {
            public static void Main()
            {
                // Ex2 네임스페이스에 포함된 TipsExam 클래스를 객체화한다.
                Ex2.TipsExam data1 = new Ex2.TipsExam();
                data1.Test();
            }
        }
    }
 
    namespace Ex2
    {
        class TipsExam
        {
            public void Test()
            {
                System.Console.WriteLine("Hello, World");
            }
        }
    }
 
    위의 코드처럼 사용하고자 하는 클래스가 코드를 구성한 네임스페이스와 다른 경우에는 반드시
    네임스페이스를 명시해주어야 하지만 동일한 네임스페이스 내의 클래스를 사용하는 경우에는
    네임스페이스를 생략할 수 있습니다.
 
    namespace Ex1
    {
        class TipsExam
        {
            public static void Main()
            {
                // 같은 네임스페이스에 포함된 SubExam 클래스를 객체화한다.
                SubExam data1 = new SubExam();
                // Ex1.SubExam data1 = new Ex1.SubExam();
                data1.Test();
            }
        }
 
        class SubExam
        {
            public void Test()
            {
                System.Console.WriteLine("Hello, World");
            }
        }
    }
 
    위 코드에서는 자신이 선언한 Ex1 외에도 System 이라는 네임스페이스가 사용되고 있습니다.
    System 이라는 네임스페이스는 기본적으로 C#에서 제공되는 네임스페이스이며 시스템 운영을 위해
    필수적인 클래스들을 포함하고 있습니다.
 
    위 예제코드에서는 System 네임스페이스에 포함된 Console 이라는 클래스를 사용하기 위해서
    System.Console 이라고 적은것이고 Console 클래스는 콘솔창에 문자열을 출력하거나 입력
    받기 위해서 사용되며 Console 클래스의 WriteLine 메서드는 사용자가 지정한 문자열을 콘솔창에
    출력하는 기능을 가지고 있습니다.
 
    Console 클래스는 시스템에서 제공하는 System 이라는 네임스페이스에 포함되어 있기때문에
    해당 클래스가 포함하는 네임스페이스를 아래와 같이 모두 명시하여 사용해야 하는 것입니다.
 
    System.Console.WriteLine("Hello, World");
  
 
    1.1 중첩된 네임스페이스 
 
        네임스페이스는 위의 코드들처럼 하나만 정의하여 사용할 수 있지만 아래의 코드처럼 여러개를
        중첩하여 사용할 수도 있습니다.
   
        namespace Ex1
        {
            class TipsExam
            {
                // 프로그램이 실행될 때 가장 먼저 수행되는 Main 함수
                static void Main(string[] args)
                {
                    // Ex2 네임스페이스 내의 SubEx 네임스페이스 내의 Exam 클래스를 객체화한다.
                    Ex2.SubEx.Exam exam = new Ex2.SubEx.Exam();
                    // 해당 클래스의 Test 함수를 호출한다.
                    exam.Test();
                }
            }
        }
 
        namespace Ex2
        {
            namespace SubEx
            {
                class Exam
                {
                    public void Test()
                    {
                        System.Console.WriteLine("Hello World");
                    }
                }
            }
        }
 
        네임스페이스에 클래스가 포함되어 있을 때 클래스명 앞에 네임스페이스를 명시해주었듯이
        중첩된 네임스페이스 안에 클래스가 포함되어 있는 경우에도 상위 네임스페이스부터 순차적으로
        네임스페이스명을 명시하여 클래스에 접근할 수 있습니다.
 
        또한 여러개의 네임스페이스가 중첩되어 코드가 복잡해질 수 있기때문에 ' . ' 연산자를 이용해서
        아래와 같은 표현으로도 사용할 수 있습니다. 
 
        // Ex2 네임스페이스에 SubEx 네임스페이스가 중첩되어 있다는 표현이다. 이 코드의 축약형이다.
        // namespace Ex2
        // {
        //     namespace SubEx
        //     {
        //     }
        // }
        namespace Ex2.SubEx
        {
            class Exam
            {
                ....
            }
        }
