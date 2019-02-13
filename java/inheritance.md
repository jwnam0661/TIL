> 아래와 같은 경우에 속한다면 객체에 메소드를 추가하는 것이 어렵다.

1. 객체를 자신이 만들지 않았다. 그래서 소스를 변경할 수 없다. 변경 할 수 있다고 해도 원 소스를 업데이트 하면 메소드 substarct이 사라진다. 이러한 문제가 일어나지 않게 하기 위해서는 지속적으로 코드를 관리해야 한다.
2. 객체가 다양한 곳에서 활용되고 있는데 메소드를 추가하면 다른 곳에서는 불필요한 기능이 포함될 수 있다. 이것은 자연스럽게 객체를 사용하는 입장에서 몰라도 되는 것까지 알아야 하는 문제가 된다.

> 이제부터 언어의 개발자가 되어 보자. 기존의 객체를 그대로 유지하면서 어떤 기능을 추가하는 방법이 없을까? 이런 맥락에서 등장하는 것이 상속이다. 즉 기존의 객체를 수정하지 않으면서 새로운 객체가 기존의 객체를 기반으로 만들어지게 되는 것이다. 이때 기존의 객체는 기능을 물려준다는 의미에서 부모 객체가 되고 새로운 객체는 기존 객체의 기능을 물려받는다는 의미에서 자식 객체가 된다. 그 관계를 반영해서 실제 코드로 클래스를 정의해보자.

> **부모 클래스와 자식 클래스의 관계를 상위(super) 클래스와 하위(sub) 클래스라고 표현하기도 한다. 또한 기초 클래스(base class), 유도 클래스(derived class)라고도 부른다. **
```java
package org.opentutorials.javatutorials.Inheritance.example1;
 
class Calculator {
    int left, right;
 
    public void setOprands(int left, int right) {
        this.left = left;
        this.right = right;
    }
 
    public void sum() {
        System.out.println(this.left + this.right);
    }
 
    public void avg() {
        System.out.println((this.left + this.right) / 2);
    }
}
 
class SubstractionableCalculator extends Calculator {
    public void substract() {
        System.out.println(this.left - this.right);
    }
}
 
public class CalculatorDemo1 {
 
    public static void main(String[] args) {
 
        SubstractionableCalculator c1 = new SubstractionableCalculator();
        c1.setOprands(10, 20);
        c1.sum();
        c1.avg();
        c1.substract();
    }
 
}
```
> 실행결과는 알아래와 같다.
```java
30
15
-10
```
> 위의 코드를 하나씩 분해해서 생각해보자.
```java
class SubstractionableCalculator extends Calculator {
    public void substract() {
        System.out.println(this.left - this.right);
    }
}
```
> 우선 새로운 클래스인 SubstractionableCalculator을 정의했다. 이 클래스의 본체에는 sbstract라는 메소드만이 존재한다. 하지만 이 클래스를 인스턴스화한 c1은 아래와 같이 정의하지 않은 메소드들을 호출하고 있다. 물론 잘 동작한다.
```java
SubstractionableCalculator c1 = new SubstractionableCalculator();
c1.setOprands(10, 20);
c1.sum();
c1.avg();
c1.substract();
```
> 이것이 가능한 이유는 extends Calculator 때문이다. 이것은 클래스 Calculator를 상속 받는다는 의미다. 따라서 SubstaractableCalculator는 Calculator에서 정의한 메소드 setOprands, sub, avg를 사용할 수 있게 된다. 이것이 프로그래밍의 역사에서 대단한 진전으로 평가받는 상속의 기본적인 의미다. 상속을 통해서 코드의 중복을 제거할 수 있었고, 또 부모 클래스을 개선하면 이를 상속받고 있는 모든 자식 클래스들에게 그 혜택이 자동으로 돌아간다. 다시 말해서 유지보수가 편리해진다는 것이다.  재활용성과 중복의 제거, 그리고 유지보수의 편의는 서로 다른 목적으로 가지고 있지만, 하나가 좋아지면 자연스럽게 다른 쪽도 좋아지는 관계에 있다는 것을 다시 한 번 환기해주는 대목이다.

> 
