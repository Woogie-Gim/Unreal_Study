# Unreal C++ Coding Standard

- https://docs.unrealengine.com/5.3/ko/epic-cplusplus-coding-standard-for-unreal-engine/

## 코딩 표준 (Coding Standard)
- 프로그래밍을 작성하는데 지켜야 하는 프로그래밍 이름 규칙, 작성 방법 등을 지정한 가이드라인
- 코딩 스타일 (Coding Style), 코딩 컨벤션 (Coding Convention) 이라고도 함

### 좋은 코딩 표준
- 절대적으로 좋은 코딩 표준이란 없음.
  - 이전에 내가 사용한 코딩 표준이 항상 옳은 것이 아님.
- 중요한 것은 코딩 표준을 정하고 잘 따르는 것
  - 이미 프로젝트에 코딩 표준이 있다면 그대로 따라야 함.
- 프로젝트의 모든 코드는 한 사람이 만든 것처럼 보여져야 함.
- 좋은 소프트웨어 회사들은 자신만의 코딩 표준이 있음.
-  언리얼 엔진은 자체적으로 코딩 표준을 정했기 때문에 기존 C++ 코딩 방법을 버리고 언리얼 엔진 코딩 표준을 따라야 함.

## 언리얼 C++ 코딩 표준

### 클래스 체계

- 클래스 는 작성자보다는 읽는 사람을 염두에 두고 조직되어야 합니다. 읽는 사람은 대부분 클래스의 퍼블릭 인터페이스를 사용할 것이므로, 퍼블릭 구현을 먼저 선언한 후 클래스의 프라이빗 구현이 뒤따라야 합니다.

```C++
 UCLASS()

    class EXAMPLEPROJECT_API AExampleActor : public AActor
    {
        GENERATED_BODY()

    public: 
        // 이 액터 프로퍼티의 디폴트값 설정
        AExampleActor();

    protected:

              // 게임 시작 시 또는 스폰 시 호출
        virtual void BeginPlay() override;

    };
```

## 프로그래밍 언어의 다양한 명명 규칙
- 파스칼 케이싱 (Pascal Casing): 합성어의 첫 글자를 대문자를 사용해 명명 (UnrealEngine) -> 언리얼엔진이 따르는 규칙
- 카멜(낙타) 케이싱 (Camel Casing): 첫 합성어는 소문자로 나머지는 대문자를 사용해 명명 (unrealEngine)
- 스네이크 케이싱 (Snake Casing): 합성어 사이에 언더바 (_)를 사용해 명명 (unreal_engine)

- 템플릿 클래스에는 접두사 T를 포함합니다.
```C++
class TAttribute
```
- UObject에서 상속하는 클래스에는 접두사 U를 포함합니다.
```C++
class UActorComponent
```
- AActor에서 상속하는 클래스에는 접두사 A를 포함합니다.
```C++
class AActor
```
- SWidget에서 상속하는 클래스에는 접두사 S를 포함합니다
```C++
class SCompoundWidget
```
- 추상적 인터페이스인 클래스에는 접두사 I를 포함합니다.
```C++
class IAnalyticsProvider
```
- (예외) 부울 변수에는 접두사 b를 포함합니다.
```C++
bPendingDestruction

bHasFadedIn.
```

- 등등

### 포용적 단어 선택

- 문제의 소지가 있는 단어들은 가급적 피하는 게 좋다

![Alt text](<Images/단어 목록.PNG>)

### 포터블 C++ 코드

- int 및 부호 없는 int 타입은 플랫폼에 따라 크기가 다를 수 있습니다. 최소 너비는 32비트로 보장되며, 정수 너비가 중요치 않은 경우라면 코드에서 사용해도 괜찮습니다. 명시적으로 크기가 정해진 타입은 여전히 시리얼라이즈 또는 리플리케이트된 포맷으로 사용합니다.

![Alt text](Images/%ED%8F%AC%ED%84%B0%EB%B8%94.PNG)

### 표준 라이브러리

![Alt text](<Images/표준 라이브러리.PNG>)

### const 정확도
- Const는 문서이자 컴파일러 지시어(directive)입니다. 모든 코드는 const 정확도를 맞추어야 합니다. 여기에는 다음과 같은 가이드라인이 포함됩니다.
- 변경하면 안되는 것을 인지시켜야 한다.

```C++
void SomeMutatingOperation(FThing& OutResult, const TArray<Int32>& InArray)
    {
        // InArray는 여기서 수정되지 않지만, OutResult는 수정될 수도 있습니다.
    }

    void FThing::SomeNonMutatingOperation() const
    {
        // 이 코드는 자신을 호출한 FThing을 수정하지 않습니다.
    }

    TArray<FString> StringArray;
    for (const FString& : StringArray)
    {
        // 이 루프의 바디는 StringArray를 수정하지 않습니다.
    }
```

- 포인터가 가리키는 것이 아니라 포인터 자체를 const로 만들 때는 끝에 const 키워드를 넣습니다. 레퍼런스는 어떤 방식으로도 '재할당' 불가하며, 같은 방법으로 const로 만들 수 없습니다.

```C++
// const 포인터에서 const 이외 오브젝트 - 포인터로의 재할당은 불가하나, T는 여전히 수정 가능합니다.
    T* const Ptr = ...;

    // 틀림
    T& const Ref = ...;
```

### C++ 언어 문법

- 언리얼 엔진은 기본적으로 C++20 언어 버전으로 컴파일하며, 빌드 시 요구하는 최소 언어 버전은 C++17입니다. 언리얼 엔진은 최신 컴파일러 전반에서 잘 지원되는 다수의 최신 언어 기능을 사용합니다.

### Override 및 Final

- override 및 final 키워드는 사용할 수 있을 뿐만 아니라, 사용을 강력히 권합니다. 빠진 부분이 다수 있을 수 있으나, 서서히 수정될 예정입니다.

### Nullptr

- nullptr 은 모든 경우 C 스타일 NULL 매크로 대신 사용해야 합니다.

### Auto

- 몇 가지 예외를 제외하면 C++ 코드에서 auto 를 사용해서는 안 됩니다.

### 타입 Enum
- 열거형(Enumerated, Enum) 클래스는 기존 네임스페이스 열거형인 일반 열거형 및 UENUM 을 대체합니다.

```C++
  // 기존 열거형
    UENUM()
    namespace EThing
    {
        enum Type
        {
            Thing1,
            Thing2
        };
    }

    // 새 열거형
    UENUM()
    enum class EThing : uint8
    {
        Thing1,
        Thing2
    }
```

### 물리적 종속성
- header에는 최대한 include를 줄이고 세밀하게 작성해야 한다.

![Alt text](<Images/물리적 종속성.PNG>)

### 일반적인 스타일 문제

- 포인터와 레퍼런스의 스페이스는 그 오른쪽에 딱 한 칸만 두어야 합니다.

    - 그래야 특정 타입에 대한 모든 포인터나 레퍼런스에 빠르게 Find in Files 를 사용할 수 있습니다. 예시는 다음과 같습니다.

```C++
//다음은 사용 가능합니다.

     FShaderType* Ptr

    //다음은 사용해서는 안 됩니다.

        FShaderType *Ptr
        FShaderType * Ptr
```

## 정리

### 언리얼 엔진 코딩 표준
1. public에서 private로 이어지는 클래스 체계 (Organization)를 준수
2. 명명 규칙
   1. 파스칼 케이싱(Pascal Casing)을 사용한다.
   2. 소문자를 가급적 사용하지 않고 공백 및 언더스코어(_)가 없음
   3. 모든 클래스와 구조체에는 고유한 접두사가 있음
3. 코드의 명확성
   1. 파라미터에 가급적 In과 Out 접두사를 사용해 명시
   2. const 지시자(directive)의 적극적인 활용
   3. 레퍼런스를 통한 복사 방지
   4. auto 키워드는 가급적 자제
4. Find In Files의 활용
5. 헤더 파일 및 #include 구문은 의존성을 최소화시켜 주의 깊게 다룰 것