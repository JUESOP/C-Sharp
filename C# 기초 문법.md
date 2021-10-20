### C# 언어

- 자바와 C++와 비슷, .NET Framework을 이용하여 프로그래밍하는 대표적 언어

- 윈도우 프로그래밍, 웹 프로그래밍, 게임 및 모바일 프로그래밍 등 모든 영역에서 사용되는 범용 프로그래밍 언어

- .cs 확장자 사용, 별도의 헤더 파일 사용하지 않음




### C# 개발도구

- C# 개발 회사인 MS에서 개발한 Visual Studio를 보편적으로 사용

- 하지만 본인은 Unity에 내장되어있는 MonoDevelop을 사용(개인 컴퓨터는 VS설취가 되어있으므로 VS사용)





### C# 기본 예제 "Hello World"
```
namespace C_Study
{
    class MainClass
    {
        public void Main(string[] args)
        {
            System.Console.WriteLine("Hello World");
        }
    }
}
```


- JAVA와 유사한 모습

- System.Console.WriteLine() : C언어의 printf와 동일, 개행O

- System.Console.Write()      : C언어의 printf와 동일, 개행X

- 주석처리는 C와 동일하게 "//" or "/**/" 사용





### C# 데이터 타입

- C언어에 있는 데이터 타입을 모두 가지고 있음
- 다만 Unity에서는 속도를 위해서 long, double 타입은 잘 사용하지 않으며, int, float 타입이 이를 대신함
- C# 에서는 float 타입 뒤에는 f, double 타입 뒤에는 d, char 타입은 ''(작은 따옴표), string 타입은 ""(큰따옴표)으로 표시

- 보통 정수나 날짜 타입같은 Value Type은 NULL 값을 가질 수 없는데 C#에서는 Nullable Type이라고 Value Type이 NULL을 가질 수 있게 함

- 물음표를 타입 뒤에 붙이면 Nullable Type이 되며 Nullable Type을 다시 Value Type으로 변환 하기 위해서는 .Value속성을 사용함



(EX)
```
// Nullable 타입
int? i = null;
i = 101;
            
bool? b = null;

//int? -> int
Nullable<int> j = null;
j = 10;
int k = j.Value;
```


### C# 변수 및 상수

- 변수는 전역 변수, 로컬(지역)변수 C언어와 다른 점이 없음

- 또한 변수명 구별도 C와 똑같음

- 상수임을 명시하기 위해서 const 키워드 사용





### C# 배열

- 배열 선언과 초기화는 다음과 같이 이루어짐

- 1차 : type[] name = new type[size];    초기화 : type[] name = { };

- 2차 : type[ , ] name = new type[size, size]    초기화 :  type[ , ] name = { { }, { } };

- 3차 : type[ , , ] name;     ··· 32차 배열까지 가질 수 있음

- 선언과 초기화를 제외한 사용은 타 언어와 동일

- 다차원 배열에서 각 차원별 배열 요소 크기가 동일한 Rectangular 배열은 [ , ]와 같이 괄호안에 콤마로 분리해서 표현(C언어 스타일)

- 각 차원별 배열 요소 크기가 가변적인 가변 배열(Jagged Array)의 경우 [ ][ ]와 같이 각 차원마다 괄호를 별도로 사용(Java 언어 스타일)



(가변배열 EX : 아직 이해 덜됨)
```
//1차 배열 크기(3)는 명시해야
int[][] A = new int[3][];

//각 1차 배열 요소당 서로 다른 크기의 배열 할당 가능
A[0] = new int[2];
A[1] = new int[3] { 1, 2, 3 };
A[2] = new int[4] { 1, 2, 3, 4 };

A[0][0] = 1;
A[0][1] = 2;
```


- 배열을 다른 객체나 메서드에 전달할 때, 배열 전체를 가리키는 참조 값만을 전달함

(배열 전달 EX)
```
public static void Main(string[] args)
{            
    int[] scores = { 1, 2, 3, 4, 5 };
    int sum = calculate(scores); // 배열 전달: 배열명 사용
    System.Console.WriteLine(sum);        
}

static int calculate(int[] array) // 배열 받는 쪽
{
    int sum = 0;
    for (int i = 0; i < array.Length; i++)
    {
        sum += array[i];
    }
    return sum;
}
```

### C# 문자열

- C# 문자열은 Immutable Type으로 한번 문자열이 설정되면, 다시 변경 불가능

- 따라서 만약 변수 s에 s = "AB"를 한 후 다시 s = "CD"라고 하게 되면 .NET 시스템은 새로운 string 객체를 생성하여 "CD"라는 데이터로 초기화 한후 이를 변수 s에 할당하게 된다 즉 내부적으로 전혀 다른 메모리를 갖는 객체를 가리키게 됨

- 문자열을 문자배열로 변환 할때는 .ToCharAraay() 메소드 사용

- 문자배열을 문자열로 변환 할때는 다음과 같이 변환
```
char[] charArray = { 'A', 'B', 'C', 'D' };
string s = new string(charArray);
```
- Unity 사용 시 문자열 최적화를 위해 사용되는 클래스 System.Text.StringBuilder 클래스가 있다

- Mutable 타입인 StringBuilder 클래스는 문자열 갱신이 많은 곳에서 사용됨, 클래스가 별도 메모리를 생성, 소멸하지 않고 일정한 버퍼를 갖고 문자열 갱신을 효율적으로 처리하기 때문

- 계속해서 문자열을 추가 변경하는 코드에서는 string 대신 StringBuilder를 사용해야 함

(StringBuilder EX)
```
using System;
using System.Text;

namespace MySystem
{
   class Program
   {
      static void Main(string[] args)
      {                  
         StringBuilder sb = new StringBuilder();
         for (int i = 1; i <= 26; i++)
         {
            sb.Append(i.ToString());
            sb.Append(System.Environment.NewLine);
         }
         string s = sb.ToString();

         Console.WriteLine(s);
      }
   }
```

### C# 연산자

- 타 언어와 거의 다른 것이 없음

- ?? 연산자의 경우 ?? 왼쪽 피연산자의 값이 NULL인 경우 ?? 뒤의 피연자 값을 리턴하고, 아니면 그냥 ?? 앞의 피연산자 값을 리턴함

- ?? 연산자는 왼쪽 피연산자가 NULL이 허용되는 데이터 타입의 경우에만 사용 가능하다 즉 위에서 설명한 Nullable Type은 ?? 연산이 가능함

(?? 연산자 EX)
```
int? i = null;

i = i ?? 0;



string s = null;

s = s ?? string.Empty;
```

### C# 조건문&반복문

- 조건문은 다른 언어와 다른 것이 없음

- 반복문의 경우 C#에는 foreach 문이 존재

- foreach문은 배열이나 컬렉션에 주로 사용하는데, 각 요소를 하나씩 꺼내와서 foreach 루프 내의 블럭을 실행할 때 사용

(foreach문 EX)
```
string[] array = new string[] {"AB", "CD", "EF"};

foreach(string s in array)

{

System.Console.WriteLine(s);

}
```
- 다른 루프 문과 foreach 문을 비교하게 되면 foreach문의 경우 다른 루프 문보다 내부적으로 최적화 되어 있으므로 가능하면 foreach문을 사용하는 것이 좋음

(for문과 foreach문 비교 EX 확연한 차이가 보인다)
```
public static void C_Study()
{
    // 3차배열 선언
    string[,,] arr = new string[,,] { 
            { {"1", "2"}, {"11","22"} }, 
            { {"3", "4"}, {"33", "44"} }
    };

    //for 루프 : 3번 루프를 만들어 돌림
    for (int i = 0; i < arr.GetLength(0); i++)
    {
        for (int j = 0; j < arr.GetLength(1); j++)
        {
            for (int k = 0; k < arr.GetLength(2); k++)
            {
                Debug.WriteLine(arr[i, j, k]);
            }
        }
    }

    //foreach 루프 : 한번에 3차배열 모두 처리
    foreach (var s in arr)
    {
        Debug.WriteLine(s);
    }
}
```


### C# yield(이해정도만.. 아직 어렵)

- 컬렉션 데이터를 하나씩 리턴할 때 사용함

- Enumerator(Iterator)라 불리는 이 기능은 집합적인 데이터셋으로부터 데이터를 하나씩 호출자에게 보냄



### C# 예외처리

- 다른 언어와 동일하게 try-catch, throw 등의 예외처리 문이 있음

- 추가적으로 try-catch-finally 예외처리 방법이 있는데 finally의 경우는 예외가 발생하던지 안하던지 상관없이 마지막에 반드시 실행되는 블럭



### C# 클래스(구조체 건너뜀)

- 다른 언어와 마찬가지로 클래스에 메서드, 속성, 필드, 이벤트 등을 멤버로 포함하여 클래스 정의로부터 객체를 생성하여 사용하게 됨

- 또한 접근제한자에 따라 외부 객체로부터 접근이 허용될 수도, 제한될 수도 있음

- Partial 클래스라는 개념이 있는데 이는 2개의 파일에 동일한 클래스의 필드, 메서드, 속성등을 나눠 저장하기 위해 이용함



### C# 메서드

- 다른 것보다도 Pass by Value, Pass by Reference에 대해 정리해볼까 함

- Pass by Value의 경우 디폴트로 값을 복사해서 전달 한다 전달된 인수를 메서드에서 변경한다해도 메서드가 끝나고 함수가 리턴된 후, 전달되었던 인수의 값은 원래 값 그대로 유지됨

(Pass by Value EX)
```
class Program
{
    private void Calculate(int a)
    {
        a *= 2;
    }

    static void Main(string[] args)
    {
        Program p = new Program();

        int val = 100;
        p.Calculate(val);  
        // val는 그대로 100        
    }
    ```
- 반면 Pass by Reference의 경우 ref 키워드를 사용하는데 ref 키워드를 사용할 경우 메서드 내에서 변경된 값은 리턴 후에도 유효함

- C#에는 ref와 비슷한 기능을 하는 것으로 C# out 키워드가 있는데 ref는 해당 변수가 사전에 초기화 되어야 하지만, out은 사전에 변수를 초기화할 필요가 없음
```
// ref 정의
static double GetData(ref int a, ref double b)
{ return ++a * ++b; }

// out 정의
static bool GetData(int a, int b, out int c, out int d)
{
    c = a + b;
    d = a - b;
    return true;
}

static void Main(string[] args)
{
    // ref 사용. 초기화 필요.
    int x = 1;
    double y = 1.0;
    double ret = GetData(ref x, ref y);

    // out 사용. 초기화 불필요.
    int c, d;
    bool bret = GetData(10, 20, out c, out d);
}
```
- Optaional 파라미터라고 어떤 메서드의 파라미터가 디폴트 값을 갖고 있다면, 메서드 호출시 이러한 파라미터를 생략하는 것을 허용한다 반드시 파라미터들 중 맨 마지막에 놓여져야 한다
```
class Program
{
    // Optional 파라미터: calcType
    int Calc(int a, int b, string calcType = "+")    // a, b만 채울 경우 자동으로 "+"가 들어감
    {
        switch (calcType)
        {
            case "+":
                return a + b;
            case "-":
                return a - b;
            case "*":
                return a * b;
            case "/":
                return a / b;
            default:
                throw new ArithmeticException();
        }
    }

    static void Main(string[] args)
    {
        Program p = new Program();
        int ret = p.Calc(1, 2);
        ret = p.Calc(1, 2, "*");
    }
}
```
출처: https://ggyu-kim.tistory.com/51 [하루도 빠짐없이 써보는 IT 일기!]
