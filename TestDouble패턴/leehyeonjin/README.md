# Mockito - Test Double 패턴

---

### 테스트 더블 (Test Double) 패턴

테스트 더블(Test Double) 패턴은 소프트웨어 테스팅에서 실제 의존성을 단순화된 또는 제어가능한 객체로 대체하는 개념.

이 패턴은 테스트 대상 코드를 외부 시스템이나 구성 요소로부터 격리시켜 독립적으로 코드의 동작을 확인하는 데 도움이 됨.

1. 목(Mock) : 목 객체는 테스트 중에 받아야 할 호출에 대한 예상값으로 미리 프로그래밍되어 있음.
2. 스텁(Stub) : 스텁은 특정 메소드 호출에 대해 미리 정의된 응답을 제공함. 실제 의존성의 특정 동작을 모방하는 데 도움이 됨.
3. 페이크(Fake) : 페이크는 실제 구현과 유사항 인터페이스를 제공하는 단순화된 구현. 테스트 중에 실제 구현을 사용하는 것이 성능상 문제가 되는 경우 자주 사용됨.
4. 스타이(Spy) : 스파이는 모크와 비슷하지만, 받은 호출에 대한 정보를 기록하여 테스트가 이후에 상호작용을 검사할 수 있도록 함.

테스트 더블 패턴은 특히 외부 시스템, 데이터베이스, API 또는 복잡한 구성 요소와 상호작용 할 때 효과적임. 이러한 요소들은 테스팅을 어렵거나 일관성 없게 만들 수 있음.

테스트 더블을 사용하여 특정 부분을 독립적으로 테스트 함으로써 더 신뢰성 있고, 유지보수가 용이한 테스트를 수행할 수 있음.

테스트 더블 패턴은 소프트웨어 테스팅에서 사용되는 일반적인 개념으로, 이것의 기원은 단일 개인이나 조직에 귀속되지 않음.

대신 이는 소프트웨어 개발 및 테스트 커뮤니티 내에서 집단적인 지식과 실천을 통해 발전해왔음.

---

### Mock 오브젝트

**Mockito**

Mockito는 모킹 프레인워크(Mocking Framework)임.

깔끔하고 간단한 API로 테스트를 작성할 수 있게 해줌.

Mockito는 태스트가 매우 가독성 있으며, 깔끔한 에러를 보여줌.

**목 객체(Mock Object)를 생성하는 여러가지 방법**

1. mock()
- Mockito의 mock() 메소드는 Mockito 라이브러리에서 가장 기본적이고 자주 사용되는 메소드 중 하나. 이 메소드는 인터페이스, 추상 클래스 또는 구체 클래스의 모킹된 객체를 생성.
- @Mock 이라는 어노테이션 지원.

```java
import org.mockito.Mockito;

public class Example {
    public interface MyObject {
        String getValue();
    }

    public static void main(String[] args) {
        // MyObject 인터페이스의 모킹된 객체 생성
				// 주어진 인터페이스나 클래스를 구현하는 객체의 목(Mock)을 생성
				// 이렇게 생성된 목 객체는 기본적으로 null 0, false 등 기본값을 반환
        MyObject mockObject = Mockito.mock(MyObject.class);

        // 모킹된 객체의 동작 설정
        Mockito.when(mockObject.getValue()).thenReturn("Mocked value");

        // 모킹된 객체 사용
        System.out.println(mockObject.getValue()); // 출력: "Mocked value"
    }
}
```
</br>

1. spy()
- Mockito의 spy() 메소드는 실제 객체를 사용하면서 일부 메소드를 모킹하여 테스트하는 데 사용되는 기능.
- mock() 메소드와 달리 spy()는 기존 객체를 랩핑하여 테스트 할 수 있음.
- 모킹된 메소드가 호출되면 실제 구현이 실행되지만, 원하는 메소드에 대해서는 테스트에서 명시적인 동작 설정 가능.
- 기존 객체를 테스트 중에 랩핑하여 사용하면, 테스트 대상 객체와의 상호작용과 . 더가까이 시뮬레이션하여 보다 정확한 테스트가 가능해짐.

```java
import org.mockito.Mockito;

public class Example {
    public static class MyObject {
        public String getValue() {
            return "Real Value";
        }
        
        public String getRealValue() {
            return "Real Value";
        }
    }

    public static void main(String[] args) {
        // MyDependency 클래스의 실제 객체 생성
        MyObject realObject = new MyObject();

        // 실제 객체를 스파이 객체로 래핑
        MyObject spyObject = Mockito.spy(realObject);

        // 일부 메서드의 동작을 모킹
        Mockito.when(spyObject.getValue()).thenReturn("Mocked value");

        // 스파이 객체 사용
        System.out.println(spyObject.getValue()); // 출력: "Mocked value"
        System.out.println(spyObject.getRealValue()); // 출력: "Real Value"
    }
}
```

---

### 행동 검증하기

1. verify()
- 테스트 중에 모의 객체와의 상호작용 검증(메소드가 예상대로 호출되었는지, 호출횟수가 일치하는지, 호출순서가 일치하는지 등).

2. times()
- 특정 모의 객체의 메소드 호출 횟수를 검증하는데 사용됨.
- 테스트 중에 모의 객체와 상호작용이 예상대로 이루어지는지를 확인하는데 유용함.
- 메소드 호출 횟수를 정확하게 검증 가능.

3. ArgumetCaptor
- 목 객체를 사용하여 메소드 호출시 전달된 인자들을 캡처하고 추출하는 기능 제공 클래스.
- 특정 메소드가 호출될 때 전달된 인자들을 테스트 중에 검증하거나, 다른 곳에서 사용할 수 있도록 할 때 유용.

---

### Given 절에서 유용한 패턴

**1. Test Data Builder**

복잡한 테스트 데이터를 쉽고 간결하게 만들어주는 패턴.

특히, 객체를 생성하고 초기화하는 과정을 단순화하고 가독성을 높이는데 도움이 됨.

**Test Data Builder 기본 아이디어**

1. 객체 생성과 초기화를 담당하는 별도의 빌더 클래스 생성.
2. 빌더 클래스는 필요한 속성을 설정하는 메소드 제공.
3. 메소드 체인을 사용하여 속성 설정 → 가독성 향상.
4. 빌더 클래스의 build() 메소드를 호출하여 실제 테스트에 사용할 객체 생성.

> 💡 일반적으로 `**@Entity**` 클래스는 `**@Builder**`를 갖는 경우가 많기 때문에, 빌더 패턴은 따로 구현하지 않아도 됨.

```java
@Builder
public class User {
    private String name;
    private int age;
    // ... (생략)
}

public class UserTestDataBuilder {
    public static User.Builder validUser() {
        return User.builder()
                 .name("jyujyu")
                 .age(20);
    }

    public static User.Builder invalidUser() {
        return User.builder()
						      // 잘못된 데이터로 설정
    }
}
```
</br></br>
**2. Object Mother**

테스트 데이터를 쉽게 생성하고 관리하기 위한 패턴.

Fixture Object 패턴과 유사.

특히 여러 테스트 케이스에서 사용되는 동일한 객체 생성 로직을 중앙에서 관리하여 코드 중복을 줄임.
</br></br>

**Object Mother 기본 아이디어**

1. 여러 테스트에서 사용되는 테스트 데이터를 관리하는 클래스 생성.
2. 해당 클래스에는 다양한 테스트 데이터를 생성하고 초기화하는 메소드들이 포함됨.
3. 각 테스트 메소드에서 필요한 테스트 데이터를 Ibject Mother 클래스의 메소드를 호출하여 가져옴.

```java
public class User {
    private String name;
    private int age;
    // ... (생략)
}

public class UserMother {
    public static User createValidUser() {
        User user = new User();
        user.setName("jyujyu");
        user.setAge(20);
        // ... (다른 필드 설정)
        return user;
    }

    public static User createInvalidUser() {
        User user = new User();
        // 잘못된 데이터로 설정
        return user;
    }
}
```
</br></br>

**3. Fixture Object(= TestFixture)**

테스트에서 사용되는 공통 데이터와 객체들을 별도의 Fixture 클래스로 분리하여 관리.

테스트 데이터를 생성하고 초기화하는 역할 수행.

테스트 클래스가 더 간결해지고, 테스트 데이터의 관리가 편리해지며, 테스트 환경 설정과 정리 작업을 효과적으로 처리 가능.

**Fixture Object(= TestFixture) 기본 아이디어**

1. 테스트 데이터를 생성하고 초기화하는 작업을 담당하는 Fixture 클래스를 생성.
2. Fixture 클래스는 테스트에 필요한 객체와 데이터를 논리적으로 그룹화.
3. 각 테스트 메소드에서 필요한 Fixture 객체를 생성하여 사용함.

```java
public class User {
    private String name;
    private int age;
    // ... (생략)
}

public class UserFixture {
    public static User createValidUser() {
        User user = new User();
        user.setName("jyujyu");
        user.setAge(20);
        // ... (다른 필드 설정)
        return user;
    }

    public static User createInvalidUser() {
        User user = new User();
        // 잘못된 데이터로 설정
        return user;
    }
}

// enum으로도 구현합니다
public enum UserFixture {
  VALID_USER(new User("jyujyu", 20),
  INVALID_USER(new User(잘못된값));
  
  private User user;

  UserFixture(User user);

  public User getUser() {
    return user;
  }
}
```
