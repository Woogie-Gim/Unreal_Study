# 언리얼 오브젝트 리플렉션 시스템 1

## 언리얼 오브젝트의 특징

### 언리얼 프로퍼티 시스템 (리플렉션)
- https://www.unrealengine.com/ko/blog/unreal-property-system-reflection

- 리플렉션(Reflection)은 프로그램이 실행시간에 자기 자신을 조사하는 기능
- 플렉션된 프로퍼티와 아닌 것을 같은 클래스에 섞어도 됩니다만, 리플렉션된 프로퍼티가 아닌 것은 해당 리플렉션에 의존하는 시스템 전부에 보이지 않는 다는 점만 주의하시면 됩니다 (즉 리플렉션 되지 않은 UObject 포인터 그대로를 저장하면 보통 가비지 콜렉터가 레퍼런스를 확인할 수 없기 때문에 위험한 일입니다).

### 언리얼 오브젝트의 구성
- 언리얼 오브젝트에는 특별한 프로퍼티와 함수를 지정할 수 있음
  - 관리되는 클래스 멤버 변수 : UPORPERTY
  - 관리되는 클래스 멤버 함수 : UFUNCTION
  - 에디터와 연동되는 메타데이터를 심을 수 있음
- 모든 언리얼 오브젝트는 클래스 정보와 함께 함.
  - 클래스를 이요해 자신이 가진 프로퍼티와 함수 정보를 컴파일 타임과 런타임에서 조회할 수 있음
- 이렇게 다양한 기능을 제공하는 언리얼 오브젝트는 NewObjectAPI를 사용해 생성해야 함.

![Alt text](<images/언리얼 오브젝트 구성.PNG>)

### 언리얼 오브젝트의 클래스 기본 오브젝트
- 언리얼 클래스 정보에는 클래스 기본 오브젝트(Class Default Object)가 포함되어 있음
- 클래스 기본 오브젝트는 줄여서 CDO라고 부름
- CDO는 언리얼 객체가 가진 기본 값을 보관하는 템플릿 객체임
- 한 클래스로부터 다수의 물체를 생성해 게임 콘텐츠에 배치할 때 일관성 있게 기본 값을 조정하는데 유용하게 사용됨
- CDO는 클래스 정보로부터 GetDefaultObject 함수를 통해 얻을 수 있음
- UClass 및 CDO는 엔진 초기화 과정에서 생성되므로 콘텐츠 제작에서 안심하고 사용할 수 있음

![Alt text](images/CDO.PNG)

### 언리얼 오브젝트의 처리

- https://docs.unrealengine.com/5.3/ko/unreal-object-handling-in-unreal-engine/

### 예제를 위한 클래스 다이어 그램
- 어떤 학교에서 학생과 교수가 함께 수업하는 상황의 구현
- 학교 정보는 GameInstance에서 지정
- 인물 클래스 (Person)
  - 학생 클래스 (Stundent)
  - 선생 클래스 (Teacher)

![Alt text](images/%EC%98%88%EC%A0%9C.PNG)

- MyGameInstance 생성 후 기본 세팅

![Alt text](<images/기본세팅 1.PNG>)

![Alt text](<images/기본세팅 2.PNG>)

- 클래스 불러오기 위한 다양한 함수

![Alt text](<images/클래스 1.PNG>)

![Alt text](<images/클래스 2.PNG>)

- 임의로 체크 함수를 통과하지 못했을 때 가정

![Alt text](<images/클래스 3.PNG>)

![Alt text](<images/클래스 4.PNG>)

- ensure 함수

![Alt text](<images/클래스 5.PNG>)

- 에디터가 꺼지지 않고 Output Log에 에러메시지가 뜨게 한다

- ensureMsgf를 통해 에러 메세지를 설정할 수 잇다

![Alt text](<images/클래스 6.PNG>)

![Alt text](<images/클래스 7.PNG>)

### Class Default Object (CDO)

- schoolName의 기본값은 빈 문자열인데 기본값을 만들고 싶다면 생성자 코드에 기본값을 지정하면 된다

![Alt text](<images/클래스 8.png>)

![Alt text](<images/클래스 9.PNG>)

- 생성자 코드에 작성한 기본 값을 설정 후 다시 SchoolName을 할당하면 기본값과 별개로 할당이 된다.

![Alt text](<images/클래스 10.PNG>)

![Alt text](<images/클래스 11.PNG>)

- GetClass() 함수를 통해 기본값에 접근할 수 있다
- 단 생성자 코드를 변경하게 되는 경우는 헤더파일과 똑같이 에디터를 종료 후 빌드를 실행하는 것이 안정적이다

### CDO를 생성하는 순간

- 엔진이 초기화 되는 과정에서 CDO, UClass 정보들이 만들어지고 게임이나 어플리케이션이 가동된다.

## 정리

### 언리얼 오브젝트 시스템
1. 언리얼 오브젝트에는 항상 클래스 정보를 담은 UClass 객체가 매칭되어 있다
2. UClass로부터 언리얼 오브젝트의 정보를 파악할 수 있음
3. UClass에는 클래스 기본 오브젝트 (CDO)가 연결되어 있어 이를 활용해 개발의 생산성을 향상시킬 수 있음
4. 클래스 정보와 CDO는 엔진 초기화 과정에서 생성되므로 게임 개발에서 안전하게 사용 가능
5. 헤더 정보를 변경하거나 생성자 정보를 변경하면 에디터를 끄고 컴파일 하는 것이 안정적임

# 언리얼 오브젝트 리플렉션 시스템 2

## 실습 예제의 구성

### 예제를 위한 클래스 다이어 그램
- 어떤 학교에서 학생과 교수가 함께 수업하는 상황의 구현
- 학교 정보는 GameInstance에서 지정
- 인물 클래스 (Person)
  - 학생 클래스 (Stundent)
  - 선생 클래스 (Teacher)

![Alt text](images/%EC%98%88%EC%A0%9C.PNG)

### 언리얼 오브젝트의 속성과 함수
- 클래스에 설정할 프로퍼티 정보
- Person에는 DoLesson이라는 가상 함수가 있음
  - Student의 DoLesson은 수업을 듣는 행동
  - Teacher의 DoLesson은 수업을 가르치는 행동

![Alt text](<images/예제 2.PNG>)

![Alt text](<images/class 1.PNG>)

- Person, Student, Teacher 오브젝트를 상속받는 클래스 생성

![Alt text](<images/class 2.PNG>)

![Alt text](<images/class 3.PNG>)

![Alt text](<images/class 4.PNG>)

![Alt text](<images/class 5.PNG>)

![Alt text](<images/class 6.PNG>)

![Alt text](<images/class 7.PNG>)

![Alt text](<images/class 8.PNG>)

![Alt text](<images/class 9.PNG>)

![Alt text](<images/class 10.PNG>)

![Alt text](<images/class 11.PNG>)

![Alt text](<images/class 12.PNG>)

## 정리

### 언리얼 리플렉션 시스템의 활용
1. 리플렉션 시스템을 사용해 언리얼 오브젝트의 특정 속성과 함수를 이름으로 검색할 수 있다.
2. 리플렉션 시스템을 사용해 접근 지시자와 무관하게 값을 설정할 수 있다
3. 리플렉션 시스템을 사용해 언리얼 오브젝트의 함수를 호출할 수 있다

```
언리얼 엔진의 기본 프레임웍은 리플렉션을 활용해 구축되어 있으므로 언리얼 엔진을 이해하기 위해서는 리플렉션 시스템을 이해하는 것이 필요함
```
