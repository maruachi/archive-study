# Item 1. 생성자 대신 정적 팩터리 메서드를 고려하라

## 장점

1. 이름을 가질 수 있다.

`BigInteger.probablePrime()` vs `new BigInteger(int, int, Random)`

정적 팩토리 메소드를 사용하면, "값 소수인 BigInteger를 반환한다." 그 의미가 분명하기 드러난다.

2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

내부 Pool을 사용하여 인스턴스를 캐싱이 가능하다. 그 예로 `Boolean.valueOf(boolean)`가 있다.

3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

Interface에 정적 메소드로 팩토리 메소드를 구현하면, 구현체를 감추고 클라이언트에게 제공 가능하다. 이는 API를 작게 만드는데, 클라이언트가 느끼는 API의 개념적 무게가 구현체에서 인터페이스로 축소되기 때문이다. `java.utils.Collections`에서 `List.of()`가 그 예 중에 하나다. List의 구현이 어떤 것인지 모르지만, List 인터페이스를 알기에 사용법을 익히기 쉽다.
	
EnumSet은 enum 요소가 64개에 대한  set 집합 정보를 long의 비트를 이용하여 표현하는데, enum 요소가 64개 이하일 때는 long 하나로 표현하고 64개 이상일 때는 long 배열로 표현한다. 이는 EnumSet 구현 방식이 다르다는 걸 의미하며 RegularEnumSet (long), JumboEnumSet (long 배열)로 구분 되어 있다. 사실 4번이 말하고자 하는 바가 3번과 크게 달라보이지 않는다.

5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

서비스 제공자 프레임워크 3개의 핵심 컴포넌트

- 서비스 인터페이스(service inteface): 구현체 동작을 정의. Connection
- 제공자 등록 API(provider registration API): 제공자가 구현체를 등록할 때 사용 DriverManager.registerDriver
- 서비스 접근 API(service access API): 클라이언트가 서비스의 인터페이스를 획득 DriverManager.getConnection

```java
// 1. registerDriver
Connection conn = DriverManager.registerDriver(new com.mysql.jdbc.Driver());

// 2. getConnection
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "username", "password");
```

제공자 등록 API는 구현체 인스턴스를 직접 넣어줘야 하고 서비스 접근 API는 구현체 생성 방식을 inteface한다. 서비스 접근 API에서는 실제 클라이언트는 구현체가 어떤 모습인지 모른다.


## 단점

1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.

`new [class name]()` 방식의 생성은 일괄적으로 객체를 인스턴스화 하기에 생성자를 찾기가 편한데, 정적 팩터리 메서드는 API doc을 참조하거나 개발자의 의도를 이해해야 한다.

# Item 2. 생성자에 매개변수가 많다면 빌더를 고려하라

생성자에 매개변수가 많을 때 사용할 수 있는 패턴이 여러 개 있다. 그중에 대표적으로 Builder 패턴을 가장 많이 쓴다. 

Builder 패턴은 점층적 생성자 패턴과 자바빈 패턴의 장점을 취한 패턴이다. 먼저 점층적 생성자 패턴과 자바빈 패턴의 문제점을 알아보고 Builder 패턴의 장점을 알아보자.

점층적 생성자 패턴은 매개변수가 많은 생성자가 적은 생성자를 wrapping해서 구현하는 방식이다. 직관적이라는 장점이 있지만 결국 생성자의 인터페이스 제한을 뛰어 넘지 못하기 때문에 매개변수 입력 값을 모두 확인하여 코드를 이해 해야한다. 즉 클라이언트 측의 가독성이 좋지 않다고 얘기할 수 있다. 반면에 자바빈 패턴은 setter를 이용해서 기본 생성자로 생성한 인스턴스에 값을 변경하여 초기화하는 방식이다. 이는 객체의 불변이 깨지는 지점이 생기고 일관성을 유지할 수 없다는 단점이 있다.

반면 Builder 패턴은 클라이언트에서 아래와 같은 패턴으로 사용된다.

```java
NutritionFacts nutritionFacts = NutritionFacts.builder()
                .servingSize(1)
                .servings(3)
                .calories(0)
                .fat(0)
                .carbohydrate(0)
                .sodium(0)
                .build();
```

각 매개 변수를 메소드로 할당하여 값을 입력하여 가독성을 높였고 최종적으로 build() 호출로 인스턴스를 생성하여 생성 단계 이후에 객체의 불변을 유지할 수 있다. Builder 패턴의 구현은 내부 정적 클래스를 선언하여 구현할 수 있다.

# Item 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라

기본적으로 생성자 private으로 두어서 생성자로 인스턴스가 추가 생성되는 것을 방지한다. 싱글톤 인스턴스를 클라이언트에 제공하는 방식은 여러가지가 있다.

- public static final 필드에 인스턴스를 생성하여 제공
- 정적 메소드로 인스턴스 제공

첫번째 방법은 싱글톤임을 API를 통해서 분명하게 드러낸다. 하지만 두번째 방법은 정적 메소드로 인터페이스하기 때문에 여러 기능을 제공할 수 있다. 예를 들어서 스레드 별로 다른 인스턴스를 제공하거나 제너릭 싱글톤을 생성할 수 있다.

하지만 싱글턴 직렬화 시에 역직렬화 할 때 새로운 인스턴스가 생성되는 문제가 있다. 이를 방지하기 위해 인스턴스 필드를 transient로 선언하고 readResolve 메소드로 인스턴스를 제공해야한다. (역직렬화 시에 readResolve 리턴값으로 readObject 결과를 대체하고 대체 . readObject 결과는 GC 대상이 된다.)

열거 타입으로 싱글톤을 생성할 수 있는데, 이는 직렬화에서 발생할 수 있는 문제나 리플렉션으로 private 메소드를 강제 호출하여 인스턴스를 생성하는 공격 모두 방지할 수 있다. (Enum 타입의 상수 값은 싱글톤임을 보장하고 있고 직렬화가 암시적으로 선언되어 있다)

# Item 4. 인스턴스화를 막으려거든 private 생성자를 사용하라

java.lang.Math와 java.util.Arrays 같이 static method를 모아 놓은 클래스가 있다. static method를 모아 놓는 것은 객체 지향적 설계에서는 주의해야 하지만 때론 유용할 때가 있다. 이런 경우에는 인스턴스화를 막는 것이 좋은데, 인스턴스화를 막는 방법 중에 가장 좋은 방법은 기본 생성자를 private으로 설정하고 주석을 달아주는 것이다. 추상 클래스도 인스턴스화를 막기는 하지만, 상속하여 인스턴스화할 수 있기 때문에 완전히 막는 것은 불가능하다. 그리고 추상 클래스는 상속을 유도하기 때문에 더욱더 좋지 않다.

# Item 5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

Dependency Injection(DI)는 사실상 DI 프레임워크인 Spring framework을 공부할 때 열심히 본 개념이다. 의존성 주입은 구현의 생성에 대한 책임을 외부로 넘기면서 객체의 구현 의존성을 끊어서 재사용성과 유연성 테스트 용이성을 개선해주는 개념이다.

# Item 6. 불필요한 객체 생성을 피하라

객체 생성의 비용의 차이는 큰 성능 차이를 만들어낸다.

1. String 리터널로 인스턴스 재사용
```java
String string = new String("Lucky Vicky") // 매번 새로운 객체를 생성
String string = "Lucky Vicky" // String Pool에 인스턴스를 이후에 재사용 가능
```

2. 기본 타입 형변환으로 객체 생성
```java
Long sum = 0L
for (int i = 0; i < 100; i++){
	sum += i;
}
```
박싱된 기본 타입일 때 형변환으로 매번 Long 객체를 생성하는 비효율이 존재한다. 이때는 long 기본 타입을 사용할 때 성능 개선의 효과를 볼 수 있다.

3. 정규 표현식 패턴 인스턴스 생성

```java
static boolean isRomanNumeral(String s) {
  return s.marches("^(?=.)M*(C[MD] }D?C{0,3})"
  +  "(X[CL]}L?X{0,3})(I[XV]|V?I{0,3})$");
}
```

정규 표현식은 유한 상태 머신으로 변환된다. 간단한 정규 표현식도 유한 상태 머신으로 변환하는 비용이 크기 때문에, 이를 캐싱해두는 것이 성능 상에 이점이 크다.

```java
public class RomanNumerals {
    private static final Pattern ROMAN = Pattern.compile("^(?=.)M*(C[MD]|D?C{0,3})" +
            "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})");

    static boolean isRomanNumeral(String s) {
        return ROMAN.matcher(s).matches();
    }
}
```
`Pattern.compile()`에서 유한 상태 머신을 미리 생성했기 때문에 matches() 호출 시에 새로운 인스턴스를 생성할 필요가 없기에 성능이 개선 됐다.

- [[아이템 06] 불필요한 객체 생성을 피하라](https://github.com/java-squid/effective-java/issues/6)

# Item 7. 다 쓴 객체 참조를 해제하라

메모리를 자동으로 관리해주는 GC 덕분에 java를 편하게 사용할 수 있지만, GC의 대상이 되지 않아 메모리 누수가 발생하여 프로그램이 예기치 않게 종료될 때가 있다. 크게 3가지 상황에서 발생할 수 있다.

*메모리 누수는 OOM(out of memory)를 발생시켜 프로그램을 예기치 못하게 종료시키거나 디스크 페이징(디스크를 메모리로 사용하는 기법)으로 성능을 저하시킨다. 이는 힙 프로파일러로 디버그 할 때 발견되기도 한다.

1. 메모리를 자체적으로 관리하는 객체

대표적으로 Stack이 있다. Stack은 ArrayList를 내부적으로 가지며 Stack 내부 메모리 크기를 초과하면 ArrayList를 2배 늘려서 사용한다. 그리고 내부에 참조됐던 객체들은 GC에서 수거 대상인지 아닌지 판단하기 힘들다.

사용하지 않는 객체에 null 참조로 변경하는 것으로 GC의 대상이 되도록 할 수 있다. 하지만 이는 예외적인 경우에서만 사용하고 일반적으로는 변수를 사용하는 범위를 제한하는 것으로 조치할 수 있다.

2. 캐시

Key를 참조할 때만 Key-value가 살아있어야 한다면, WeakHashMap으로 캐시를 구현하자. Key 참조가 끝나면 즉시 GC의 대상이 될 것이다.

3. 리스너와 콜백

리스너와 콜백을 명확히 해지해주지 않는 것도 메모리 누수의 원인이다. WeakReference로 등록할 경우에 GC의 대상이 되게 할 수 있다.

*만약 어떤 객체가 WeakReference만 가지고 있다면, 가비지 컬렉터는 그 객체를 더 이상 사용되지 않는다고 판단

# Item 8. finalizer와 cleaner 사용을 피하라

## finalizer와 cleaner란?
- finalizer와 cleaner는 모두 GC에 의해 실행되는 리소스 해제 로직이다.
  - C++ destructor와 달리, 비메모리 자원의 해제가 아닌 메모리 자원에 대한 해제를 담당한다.

## finalizer와 cleaner를 쓰지 말아야 할 이유
- GC에 의존하는 특성상 언제 리소스가 해제될지 모르기 때문에 메모리 누수의 원인이 되므로 기피하는 것이 좋다.
  - 락 해제를 finalizer와 cleaner가 담당하면 락은 어떤 시점에 해제될지 모르고 락과 연관된 분산 시스템은 문제가 될 것이다.
- 그리고 성능의 이슈가 있는데 단순히 finalizer, cleaner 안정망 추가로 객체 생성, 정리, 파과 과정이 약 5배~50배 가량 느려진다.
  - finalize()가 구현된 객체는 GC가 두 번 실행되어야 완전히 제거됩니다. 처음에는 "준비 완료" 상태로 설정된 후 finalize() 메서드를 실행하고, 그 후에야 실제로 메모리에서 제거됩니다. 이로 인해 객체의 수명이 불필요하게 길어지고, GC의 성능을 저하시킬 수 있습니다.
- finalizer는 공격의 취약한데, 직렬화나 생성자 과정에서 finalizer는 자신의 인스턴스를 static 필드에 할당하여 GC를 방지할 수 있다. 이런 과정에서 훼손된 객체를 남겨 악의적인 공격을 할 수 있다.
- finalizer 예외 발생 시에 자원 회수 로직을 하다 말기 때문에 훼손된 객체를 남긴다. 대신 cleaner는 자신의 스레드를 통제하기 때문에 예외처리 가능하다.
  - 이런 이유로 쓸 꺼면 cleaner를 쓰는 것이 낫다.
- 결국에는 자원 해제는 finalizer와 cleaner보다 try-with-resource 구문과 함께 Autoclosable을 같이 쓰는 것이 좋다.

## finalizer와 cleaner를 사용할 때는 언제인가?
- 2차 방어로직으로 사용할 때 -> FileInputStream, FileOutputStream, ThreadPoolExecutor
- 네이티브 피어(native peer) 다른 언어로 작성된 코드를 가진 객체일 때는 해당 언어에 대한 자원 해제를 GC가 담당하지 못하기 때문에 finalizer와 cleaner에 회수 로직을 작성한다.
  - 하지만 성능 이슈가 클 경우에는 Autoclosable의 close()를 사용해야 한다.

# Item 9. try-finally보다는 try-with-resources를 사용하라
- try-finally의 기존의 문제점
  - 기기에 물리적 문제가 있을 때 try의 예외가 finally의 예외에게 잡아 먹힌다(?) -> 디버깅을 어렵게 한다 (이해가 안 됨)
  - 해제해야 되는 자원이 여러개 일 때 try-finally 중첩 구문이 되어 코다가 복잡해진다.
- try-with-resources 구문으로 위 문제를 해결할 수 있다.

# Item 10. equals는 일반 규약을 지켜 재정의하라
- equals 재정의가 필요하지 않는 상황이라면 최대한 피하는 게 좋다
  - 각 인스턴스가 본질적으로 고유하다
  - 인스턴스의 논리적 동치성을 검사할 일이 없다
  - 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다
  - 클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없다
- equals 메서드를 구현한다면 아래와 같은 동치 관계를 만족해야 한다.
  - 반사성(reflexivity): x.equals(x) == true
  - 대칭성(symmetry): x.equals(y) == y.equals(x)
  - 추이성(transitivity): x.equals(y) == y.equals(z) == true -> x.equals(z) == true
  - 일관성(consistency): x.equals(y) 여러번 수행하더라도 결과가 동일
  - null-아님: x.equals(null) == false
- equals 올바른 재정의
  1. 자기 참조인지 검사
  2. 자기 타입이지 검사
  3. 자기 타입으로 형변환
  4. 핵심 필드 일치여부 검사
  *hash code 재정의 필요
- equals에서의 리스코프 치환 원칙
  - Point의 하위 클래스는 정의상 여전히 Point이므로 어디서든 Point로써 활용될 수 있어야 한다.
  - 하위 클래스에서 equals가 정상 동작하기 위해서는 getClass가 아닌 instanceof 기반으로 eqauls를 구현해야 한다.
  - field 값 추가는 리스코프 치환 원칙을 어기기 쉽다. (그 예로 java.sql.timestamp, java.util.date가 있다.)
  - 대신에 상속 대신에 조합을 써서 우회할 수 있다.

# Item 11. equals를 재정의하려거든 hashcode도 재정의하라
- equals 객체에서 hashcode를 제대로 재정의 하지 않으면 발생할 수 있는 문제가 크게 두 가지다.
  - 특정 해쉬 코드로 여러 인스턴스가 몰려 성능 이슈 발생
    - 해쉬 알고리즘에 따라 데이터 접근의 시간복잡도가 O(1)가 아닌 cardinality에 의존하는 선형 O(n)으로 높아진다.
    - 핵심 필드를 유지하여 hashcode()가 각 인스턴스 별로 다른 값을 내면 좋다.
  - equals로 동등성이 보장되지만 hashcode는 다른 값을 내는 경우
    - 이런 경우 HashMap, HashSet과 같은 자료구조에서 비정상 동작할 수 있다. 
