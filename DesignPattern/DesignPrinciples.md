### 디자인 원칙

---

#### 1. 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분과 분리한다.

- 상속 사용시 문제점(슈퍼클레스에 메소드를 추가)
    - 서브클레스에서 코드가 중복된다
    - 실행시 특징을 바꾸기 힘들다
    - 모든 서브클레스의 행동을 알기 힘들다
    - 슈퍼클레스 코드를 변경했을때 다른 서브클레스에 원치않은 영향을 끼칠수있다
    - 즉, 계속 유동적으로 규격이 바뀌게된다면, 상속이 올바른 방법이 아니게 된다
- 인터페이스 사용시 문제점
    - 구현된 코드가 없으므로 코드를 재사용할수없다
    - 한가지 행동을 바꿀때마다 행동이 정의되어있는 서브클레스를 전부찾아 수정해야한다
    - 그과정에서 새로운 버그 발생확률이 높다
- 이런 상황에서 디자인패턴 사용
    - 바뀌는 부분을 따로 뽑아 캡슐화한다
    - 의도치않게 발생하는 문제를 줄이고, 시스템유연성을 확장할수있다
    - 바뀌지않는 부분에 영향을 미치지않고 해당 부분만 고치거나 확장할수있다
    - 행동과 관련된 setter(세터)를 사용해 **행동을 동적으로 바꿀수**있다

---

#### 2. 구현보다는 인터페이스에 맞춰서 프로그래밍한다.


- 인터페이스에 맞춰서 프로그래밍한다 = 상위형식에 맞춰 프로그래밍 하다
- 객체가 코드에 고정되지 않도록 상위형식에 맞춰 프로그래밍해야한다
- 그리고, 변수를 선언할때 보통 추상클래스나 인터페이스 같은 상위형식으로 선언해야한다
- 상위형식 변수에 객체를 대입할때 상위형식을 구체적으로 구현한 어떤객체든 넣을수있다
- 즉, 변수를 선언하는 클레스는 실제객체의 형식을 몰라도 된다
```java
// 구현에 맞춰 프로그래밍
Dog d = new Dog();
d.bark();

// 인터페이스와 상위형식에 맞춰 프로그래밍
Animal animal = new Dog();
animal.makeSound();

// 더 바람직한 방식
a = getAnimal();
a.makeSound();
```
---

#### 3. 상속보다는 구성(composition)을 사용한다.
- 구성은 상속보다 낫다(Composition over Inheritance)
- “A에는 B가 있다”라는 관계를 생각하기
- 클레스A는 doBehavior라는 인터페이스가 있고, doBehavior를 구현한 구현체 do1 ,do2가있을때, A는 행동을 상속받는 대신, 올바른 행동객체로 Composition되어 행동을 부여받는다
- 이렇게 두개의 클레스를 합치는것을 “구성(Composition)”이라고 한다
- 구성은 유연성을 크게 향상시킬수있는 기술이다.
- 알고리즘군을 별도 클래스 집합으로 캡슐화할수있고, 실행시 행동을 바꿀수있다
- 이를통해 **런타임에 동적인 행동변경**이 가능하다
- 전략패턴, 데코레이터 패턴, 책임연쇄패턴

```java
// 전략패턴 예시
// Strategy 인터페이스
public interface PaymentStrategy {
    void pay(int amount);
}

// Concrete Strategy 구현
public class CreditCardPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card.");
    }
}

// Concrete Strategy 구현
public class PayPalPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal.");
    }
}

// Context 클래스
public class PaymentContext {
    private PaymentStrategy strategy; // 변수를 상위형식으로 선언

    // 전략을 구성(주입) - Composition
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void pay(int amount) {
        strategy.pay(amount);
    }
}

// 클라이언트 코드
public class Main {
    public static void main(String[] args) {
        PaymentContext context = new PaymentContext();

        // 구성으로 전략 설정(Composition)
        context.setPaymentStrategy(new CreditCardPayment());
        context.pay(100);

        context.setPaymentStrategy(new PayPalPayment());
        context.pay(200);
    }
}
```
---