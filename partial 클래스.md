# partial 클래스

클래스 정의를 분할하는 것이 바람직한 몇 가지 상황이 있습니다.

- 대규모 프로젝트에서 작업하는 경우 클래스를 개별 파일에 분산하면 여러 프로그래머가 동시에 클래스에 대해 작업할 수 있습니다.
- 자동으로 생성된 소스로 작업하는 경우 소스 파일을 다시 만들지 않고도 클래스에 코드를 추가할 수 있습니다. Visual Studio에서는 Windows Forms, 웹 서비스 래퍼 코드 등에 만들 때 이 방식을 사용합니다. Visual Studio에서 만든 파일을 수정하지 않고도 이러한 클래스를 사용하는 코드를 만들 수 있습니다.
- 소스 생성기를 사용하여 클래스에서 추가 기능을 생성하는 경우  


클래스 정의를 분할하려면 다음과 같이 partial 키워드 한정자를 사용합니다.

C#
```
public partial class Employee
{
    public void DoWork()
    {
    }
}

public partial class Employee
{
    public void GoToLunch()
    {
    }
}
```

partial 키워드는 클래스, 구조체 또는 인터페이스의 다른 부분을 네임스페이스에서 정의할 수 있음을 나타냅니다. 모든 부분은 partial 키워드를 사용해야 합니다. 최종 형식을 생성하려면 컴파일 시간에 모든 부분을 사용할 수 있어야 합니다. 모든 부분에 public, private 등의 동일한 액세스 가능성이 있어야 합니다.

부분이 abstract로 선언된 경우 전체 형식이 abstract로 간주됩니다. 부분이 sealed로 선언된 경우 전체 형식이 sealed로 간주됩니다. 부분이 기본 형식을 선언하는 경우 전체 형식이 해당 클래스를 상속합니다.

기본 클래스를 지정하는 부분은 모두 일치해야 하지만 기본 클래스를 생략하는 부분도 여전히 기본 형식을 상속합니다. 부분에서 다른 기본 인터페이스를 지정할 수 있으며, 최종 형식은 모든 partial 선언에 나열된 모든 인터페이스를 구현합니다. 부분 정의에 선언된 클래스, 구조체 또는 인터페이스 멤버는 다른 모든 부분에서 사용할 수 있습니다. 최종 형식은 컴파일 시간의 모든 부분 조합입니다.
```
[SerializableAttribute]
partial class Moon { }

[ObsoleteAttribute]
partial class Moon { }
```
이러한 선언은 다음 선언과 동일합니다.

```
[SerializableAttribute]
[ObsoleteAttribute]
class Moon { }
```

예를 들어 다음 선언을 살펴보세요.
```
partial class Earth : Planet, IRotate { }
partial class Earth : IRevolve { }
```
이러한 선언은 다음 선언과 동일합니다.
```
class Earth : Planet, IRotate, IRevolve { }
```

# 제한  
partial 클래스 정의로 작업할 때 따라야 할 몇 가지 규칙이 있습니다.

- 동일한 형식의 일부로 작성된 모든 부분 형식(Partial Type) 정의를 partial로 수정해야 합니다. 예를 들어 [!code-csharpAllDefinitionsMustBePartials#7] 클래스 선언은 오류를 생성합니다.
- partial 한정자는 class, struct 또는 interface 키워드 바로 앞에만 올 수 있습니다.
- [!code-csharpNestedPartialTypes#8] 예제와 같이 부분 형식 정의에 중첩된 부분 형식을 사용할 수 있습니다.
- 동일한 형식의 일부로 작성된 모든 부분 형식(Partial Type) 정의는 동일한 어셈블리와 동일한 모듈(.exe 또는 .dll 파일)에서 정의해야 합니다. 부분 정의는 여러 모듈에 걸쳐 있을 수 없습니다.
- 모든 부분 형식(Partial Type) 정의에서 클래스 이름 및 제네릭 형식 매개 변수가 일치해야 합니다. 제네릭 형식은 부분일 수 있습니다. 각 부분 선언에서 동일한 매개 변수 이름을 동일한 순서로 사용해야 합니다.
- 부분 형식(Partial Type) 정의에서 다음 키워드는 선택 사항이지만, 부분 형식(Partial Type) 정의에 있는 경우 동일한 형식의 다른 부분 정의에서 지정된 키워드와 충돌할 수 없습니다.  
* public
* private
* protected
* internal
* abstract
* sealed
* 기본 클래스
* new 한정자(중첩된 부분)
* 제네릭 제약 조건
