# 전략패턴(Strategy Pattern)  

### 개념

객체의 행동(알고리즘)을 캡슐화하여 동적으로 선택하거나 변경할 수 있도록 설계하는 **행동(Behavioral)디자인 패턴** 이다

---

### 핵심

1. 알고리즘은 독립된 클래스들로 분리하여 관리
2. 실행중 특정 알고리즘을 선택할수있도록 유연성을 제공
3. 공통인터페이스를 사용해 서로다른 알고리즘을 동일하게 취급

---

### 구조

- Context : 전략을 사용하는 클라이언트 객체. 실행할 알고리즘을 동적으로 설정후 실행
- Strategy : 알고리즘의 공통인터페이스를 정의
- Concrete Strategy : Strategy 인터페이스를 구현한 구체적인 알고리즘 클레스

---

### 흐름

1. Context는 Strategy 인터페이스를 사용해 알고리즘을 실행
2. 알고리즘 구현체(Concrete Strategy)는 Strategy 인터페이스를 통해 교체가능하도록 설계
3. 클라이언트는 Context에 적절한 Concrete Strategy를 주입해 원하는 알고리즘 실행

---

### 장점

- 유연성(Flexibility)
    - 알고리즘(전략)을 독립적으로 분리하여 캡슐화하기 때문에 기존 코드를 수정하지 않고 새로운 전략을 쉽게 추가가능
- 코드 재사용성 증가(Reusability)
    - 공통된 인터페이스를 통해 다양한 알고리즘을 정의하므로 동일한 전략 구현을 여러 컨텍스트에서 재사용 가능
- 유지보수 용이성(Maintainability)
    - 알고리즘을 캡슐화하여 분리했기 때문에, 특정 전략의 로직을 변경하더라도 다른 코드에 영향을 주지 않음
- 개방-폐쇄 원칙(OCP) 준수
    - 새로운 전략을 추가할 때 기존 코드를 변경하지 않아도 됨
- 조건문 제거(Reduction of Conditionals)
    - if-else 또는 switch 문을 사용하여 알고리즘을 선택하는 대신, 객체 지향적인 방식으로 전략을 교체가능
    - 
---

### 단점

- 복잡도 증가(Increased Complexity)
    - 전략 클래스를 추가로 정의해야 하므로 클래스와 객체의 수가 늘어나 코드 구조가 복잡해질 수 있음
- 클라이언트의 설정 책임(Client Configuration)
    - 클라이언트가 어떤 전략을 사용할지 선택해야 하므로, 잘못된 전략을 설정할 가능성이 있음
- 단순한 경우에는 오히려 비효율적(Inefficiency in Simple Cases)
    - 단순한 알고리즘이라면 전략 패턴을 사용하는 것이 과도한 설계가 될 수 있음
- 모든 전략 간 인터페이스 일치 요구(Interface Uniformity)
    - 전략 간 공통 인터페이스를 정의해야 하기 때문에 일부 전략의 구현이 어색하거나 불필요한 메서드를 포함할 수도 있음

---

### 언제 사용하면 좋은가?

- 다양한 알고리즘을 동적으로 선택해야 할 때
    
    - 예: 결제 수단(신용카드, PayPal, 계좌 이체 등) 선택
    
- 알고리즘이 자주 변경되거나 확장 가능성이 있을 때
    
    - 예: 데이터 압축 방식, 데이터의 크기와 종류에 따라 다른 정렬 알고리즘을 선택
    
- if-else 또는 switch 문이 과도하게 사용될 때
    
    - 예: 조건문이 많아지면 유지보수가 어렵기 때문에 전략 패턴이 적합

### 예시 코드

```java
// 1. Strategy 인터페이스 정의
public interface PaymentStrategy {
    void pay(int amount);
}
```

```java
// 2. Concrete Strategy 구현 (다양한 결제 방식)
public class CreditCardPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card.");
    }
}

public class PayPalPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal.");
    }
}

public class BitcoinPayment implements PaymentStrategy {
    @Override
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Bitcoin.");
    }
}
```

```java
// 3. Context 정의 (결제 실행을 담당)
public class PaymentContext {
    private PaymentStrategy strategy;

    // 동적으로 전략 설정(composition - 구성)
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void executePayment(int amount) {
        if (strategy == null) {
            throw new IllegalStateException("Payment strategy is not set.");
        }
        strategy.pay(amount);
    }
}
```

```java
// 4. 클라이언트 코드
public class StrategyPatternExample {
    public static void main(String[] args) {
        PaymentContext context = new PaymentContext();

        // Credit Card로 결제
        context.setPaymentStrategy(new CreditCardPayment()); // 구성(Composition)으로 동적 변경
        context.executePayment(100);

        // PayPal로 결제
        context.setPaymentStrategy(new PayPalPayment()); // 구성(Composition)으로 동적 변경
        context.executePayment(200);

        // Bitcoin으로 결제
        context.setPaymentStrategy(new BitcoinPayment()); // 구성(Composition)으로 동적 변경
        context.executePayment(300);
    }
}
```