다형성
=
> 다형성이란 하나의 메소드나 클래스가 있을 때 이것들이 다양한 방법으로 동작하는 것을 의미한다. 키보드의 키를 통해서 비유를 들어보겠다. 키보드의 키를 사용하는 방법은 '누른다'이다. 하지만 똑같은 동작 방법의 키라고 하더라도 ESC는 취소를 ENTER는 실행의 목적을 가지고 있다. 다형성이란 동일한 조작방법으로 동작시키지만 동작방법은 다른 것을 의미한다.

overloading과 다형성
-
>오버로딩은 가장 이해하기 쉬운 다형성의 예라고 할 수 있다. 아래의 코드를 보자.
```java
package org.opentutorials.javatutorials.polymorphism;
class O{
    public void a(int param){
        System.out.println("숫자출력");
        System.out.println(param);
    }
    public void a(String param){
        System.out.println("문자출력");
        System.out.println(param);
    }
}
public class PolymorphismOverloadingDemo {
    public static void main(String[] args) {
        O o = new O();
        o.a(1);;
        o.a("one");
    }
}
```
> 클래스 O의 메소드 a는 두개의 본체를 가지고 있다. 동시에 두개의 본체는 하나의 이름인 a를 공유하고 있다. 같은 이름이지만 서로 다른 동작 방법을 가지고 있기 때문에 오버로딩은 다형성의 한 예라고 할 수 있다.

클래스와 다형성
-
```java
package org.opentutorials.javatutorials.polymorphism;
class A{}
class B extends A{}
public class PolymorphismDemo1 {
    public static void main(String[] args) {
        A obj = new B();
    }
}
```
> 이상하게 보이겠지만 클래스 B의 데이터 형이 클래스 A이다. 클래스 B는 클래스 A를 상속하고 있다. 이런 경우에 클래스 B는 클래스 A를 데이터 형으로 삼을 수 있다. 그럼 이렇게 하는 이유가 무엇인가 궁금해 질 것이다. 위의 코드를 변경한 아래의 코드를 보자.
```java
package org.opentutorials.javatutorials.polymorphism;
class A{
    public String x(){return "x";}
}
class B extends A{
    public String y(){return "y";}
}
public class PolymorphismDemo1 {
    public static void main(String[] args) {
        A obj = new B();
        obj.x();
        obj.y();
    }
}
```
> 아래 코드는 실행이 된다.
```java
obj.x();
```
> 하지만 아래 코드는 실행되지 않는다.
```java
obj.y();
```
> 클래스 B는 메소드 y를 가지고 있다. 그럼에도 불구하고 메소드 y가 마치 존재하지 않는 것처럼 실행되지 않고 있다. 10행의 코드를 아래와 같이 변경해보자.
```java
B obj = new B();
```
> 그럼 아래 코드가 실행될 것이다.
```java
obj.y();
```
> 즉 클래스 B의 데이터 형을 클래스 A로 하면 클래스 B는 마치 클래스 A인것처럼 동작하게 되는 것이다. 클래스 B를 사용하는 입장에서는 클래스 B를 클래스 A인것처럼 사용하면 된다. 여전히 왜 이런 기능이 있는지 의구심이 풀리지 않을 것이다. 아래 코드를 보자.
```java
package org.opentutorials.javatutorials.polymorphism;
class A{
    public String x(){return "A.x";}
}
class B extends A{
    public String x(){return "B.x";}
    public String y(){return "y";}
}
public class PolymorphismDemo1 {
    public static void main(String[] args) {
        A obj = new B();
        System.out.println(obj.x());
    }
}
```
> 클래스 A의 메소드 x를 클래스 B에서 오버라이딩하고 있다. 실행 결과는 아래와 같다.
```java
B.x
```
> 찬찬히 따져보자.

1. 클래스 B의 데이터 타입을 클래스 A로 인스턴스화 했을 때 클래스 B의 메소드 y는 마치 존재하지 않는 것처럼 실행되지 않았다. => 클래스 B가 클래스 A화 되었다.
2. 클래스 B의 데이터 타입을 클래스 A로해서 인스턴스화 했을 때 클래스 B의 메소드 x를 실행하면 클래스 A에서 정의된 메소드가 아니라 클래스 B에서 정의된 메소드가 실행 되었다. => 클래스 B의 기본적인 성질은 그대로 간직하고 있다.

> 정리해보면 아래와 같다.

> 클래스 B를 클래스 A의 데이터 타입으로 인스턴스화 했을 때 클래스 A에 존재하는 맴버만이 클래스 B의 맴버가 된다. 동시에 클래스 B에서 오버라이딩한 맴버의 동작방식은 그대로 유지한다. 아래의 코드를 보자.
```java
package org.opentutorials.javatutorials.polymorphism;
class A{
    public String x(){return "A.x";}
}
class B extends A{
    public String x(){return "B.x";}
    public String y(){return "y";}
}
class B2 extends A{
    public String x(){return "B2.x";}
}
public class PolymorphismDemo1 {
    public static void main(String[] args) {
        A obj = new B();
        A obj2 = new B2();
        System.out.println(obj.x());
        System.out.println(obj2.x());
    }
}
```
> 실행결과는 아래와 같다.
```java
B.x
B2.x
```
> 아래의 코드는 서로 다른 클래스 B와 B2가 동일한 데이터 타입 A로 인스턴스화 되었다.
```java
A obj = new B();
A obj2 = new B2();
```
> 하지만 두 인스턴스의 메소드 x를 호출한 결과는 서로 다르다.

> 이것이 상속과 오버라이딩 그리고 형변환을 이용한 다형성이다.

> 하위 클래스를 상위 클래스의 데이터 타입으로 인스턴스화 했을 때 어떤 일이 일어나는지에 대해서는 어느정도 이해했을꺼라고 생각한다. 하지만 가장 큰 틀의 질문은 이걸 어디에 사용하는가?일것이다. 정당한 질문이다. abstract 수업의 예제 코드를 조금 변경해보자.
```java
package org.opentutorials.javatutorials.polymorphism;
abstract class Calculator{
    int left, right;
    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    } 
    int _sum() {
        return this.left + this.right;
    }
    public abstract void sum();  
    public abstract void avg();
    public void run(){
        sum();
        avg();
    }
}
class CalculatorDecoPlus extends Calculator {
    public void sum(){
        System.out.println("+ sum :"+_sum());
    }
    public void avg(){
        System.out.println("+ avg :"+(this.left+this.right)/2);
    }
} 
class CalculatorDecoMinus extends Calculator {
    public void sum(){
        System.out.println("- sum :"+_sum());
    }
    public void avg(){
        System.out.println("- avg :"+(this.left+this.right)/2);
    }
} 
public class CalculatorDemo {
    public static void main(String[] args) { 
        Calculator c1 = new CalculatorDecoPlus();
        c1.setOprands(10, 20);
        c1.run();
         
        Calculator c2 = new CalculatorDecoMinus();
        c2.setOprands(10, 20);
        c2.run();
    }
   
}
```
> 차이점은 Calculator를 상속 받은 클래스들을 인스턴스화 할 때 Calculator를 데이터 타입으로 하고 있다. 이렇게 되면 인스턴스 c1과 c2를 사용하는 입장에서 두개의 클래스 모두 Calculator인 것처럼 사용할 수 있다. 예제를 조금 수정해보자.
```java
package org.opentutorials.javatutorials.polymorphism;
abstract class Calculator{
    int left, right;
    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    } 
    int _sum() {
        return this.left + this.right;
    }
    public abstract void sum();  
    public abstract void avg();
    public void run(){
        sum();
        avg();
    }
}
class CalculatorDecoPlus extends Calculator {
    public void sum(){
        System.out.println("+ sum :"+_sum());
    }
    public void avg(){
        System.out.println("+ avg :"+(this.left+this.right)/2);
    }
} 
class CalculatorDecoMinus extends Calculator {
    public void sum(){
        System.out.println("- sum :"+_sum());
    }
    public void avg(){
        System.out.println("- avg :"+(this.left+this.right)/2);
    }
} 
public class CalculatorDemo {
    public static void execute(Calculator cal){
        System.out.println("실행결과");
        cal.run();
    }
    public static void main(String[] args) { 
        Calculator c1 = new CalculatorDecoPlus();
        c1.setOprands(10, 20);
         
        Calculator c2 = new CalculatorDecoMinus();
        c2.setOprands(10, 20);
         
        execute(c1);
        execute(c2);
    }
}
```
> 클래스 CalculatorDemo의 execute 메소드는 CalculatorDecoPlus와 CalculatorDecoMinus 클래스의 메소드 run을 호출하면서 그것이 '실행결과'라는 사실을 화면에 표시하는 기능을 가지고 있다. 이 때 메소드 execute 내부에서는 매개변수로 전달된 객체의 메소드 run을 호출하고 있다.

> 만약 메소드 execute의 매개변수 데이터 타입이 Calculator가 아니라면 어떻게 해야할까? 위와 같은 로직을 처리 할 수 없을 것이다. 메소드 execute 입장에서는 매개변수로 전달된 값이 Calculator이거나 그 자식이라면 메소드 run을 가지고 있다는 것을 보장 받을 수 있게 되는 것이다.

> 이 맥락에서의 다형성이란 하나의 클래스(Calculator)가 다양한 동작 방법(ClaculatorDecoPlus, ClaculatorDecoMinus)을 가지고 있는데 이것을 다형성이라고 할 수 있겠다. 

인터페이스와 다형성
-
> 위의 예제는 클래스의 상속 관계를 통해서 다형성을 설명하고 있는데, 다형성의 세계에서는 인터페이스도 중요한 수단이다. 인터페이스 수업에서는 언급하지 않은 것이 있다. 특정한 인터페이스를 구현하고 있는 클래스가 있을 때 이 클래스의 데이터 타입으로 인터페이스를 지정 할 수 있다. 말이 어렵다면 코드를 통해서 무슨 말인지 알아보자.
```java
package org.opentutorials.javatutorials.polymorphism;
interface I{}
class C implements I{}
public class PolymorphismDemo2 {
    public static void main(String[] args) {
        I obj = new C();
    }
}
```
> 위의 코드를 통해서 알 수 있는 것은 클래스 C의 데이터 타입으로 인터페이스 I가 될 수 있다는 점이다. 이것은 다중 상속이 지원되는 인터페이스의 특징과 결합해서 상속과는 다른 양상의 효과를 만들어낸다. 아래 코드를 보자.
```java
package org.opentutorials.javatutorials.polymorphism;
interface I2{
    public String A();
}
interface I3{
    public String B();
}
class D implements I2, I3{
    public String A(){
        return "A";
    }
    public String B(){
        return "B";
    }
}
public class PolymorphismDemo3 {
    public static void main(String[] args) {
        D obj = new D();
        I2 objI2 = new D();
        I3 objI3 = new D();
         
        obj.A();
        obj.B();
         
        objI2.A();
        //objI2.B();
         
        //objI3.A();
        objI3.B();
    }
}
```
> 주석처리된 메소드 호출은 오류가 발생하는 것들이다. objI2.b()에서 오류가 발생하는 이유는 objI2의 데이터 타입이 인터페이스 I이기 때문이다. 인터페이스 I는 메소드 A만을 정의하고 있고 I를 데이터 타입으로 하는 인스턴스는 마치 메소드 A만을 가지고 있는 것처럼 동작하기 때문이다.

> 이것은 인터페이스의 매우 중요한 특징 중의 하나를 보여준다. 인스턴스 objI2의 데이터 타입을 I2로 한다는 것은 인스턴스를 외부에서 제어할 수 있는 조작 장치를 인스턴스 I2의 맴버로 제한한다는 의미가 된다. 인스턴스 I2와 I3로 인해서 하나의 클래스가 다양한 형태를 띄게 되는 것이다.

비유
-
> 사람은 다면적인 존재다. Steve라는 사람이 있다. 이 사람은 집에서는 아버지이고 직업적으로는 프로그래머이고 또 종교단체 내에서는 신도(believer)가 될 수 있다. 하나의 사람이지만 그가 어디에 있는가? 누구와 관계하는가에 따라서 아버지이면서 프로그래머이고 또 신도인 것이다.

> Rachel는 집에서는 엄마고 직장에서는 프로그래머다.

> Steve와 Rachel이 같은 직장(Workspace)에 다니고 있다고 한다면 직장 입장에서는 두사람이 프로그래머라는 점이 중요할 뿐 이들의 가족관계나 종교성향에는 관심이 없다. 직장 입장에서 두사람은 프로그래머이고 프로그래머는 코딩을 통해서 무엇인가를 창조하는 사람들이다. 따라서 이들에게 업무를 요청할 때는 코딩을 요청하면 된다. 하지만 두 사람의 실력이나 성향에 따라서 코딩의 결과물은 달라질 것이다. 이러한 관계를 굳이 코드로 만들면 아래와 같다.
```java
package org.opentutorials.javatutorials.polymorphism;
 
interface father{}
interface mother{}
interface programmer{
    public void coding();
}
interface believer{}
class Steve implements father, programmer, believer{
    public void coding(){
        System.out.println("fast");
    }
}
class Rachel implements mother, programmer{
    public void coding(){
        System.out.println("elegance");
    }
}
public class Workspace{
    public static void main(String[] args){
        programmer employee1 = new Steve();
        programmer employee2 = new Rachel();
         
        employee1.coding();
        employee2.coding();
    }
}
```
> 위의 코드를 보면 알겠지만 Steve와 Rachel의 사용자인 직장에서는 Steve와 Rachel의 인터페이스인 programmer를 통해서 두사람과 관계하게 된다. 두 사람이 어떤 종교나 가족관계를 가졌건 인터페이스 programmer을 가지고 있다면 고용할 수 있다. 회사에서는 코딩을 할 수 있는 사람이 필요하고 어떤 사람이 programmer라는 인터페이스를 구현하고 있다면 그 사람은 반드시 coding이라는 메소드를 구현하고 있을 것이기 때문이다. 또 두 사람에게 업무를 요청 할 때는 programmer라는 인터페이스의 메소드인 coding을 통해서 요청하면 된다. 하지만 두 사람의 성향이나 능력에 따라서 그 업무를 수행한 결과는 다른데 Steve는 빠르게 코딩하고 Rachel은 우아하게 코딩하고 있다.
