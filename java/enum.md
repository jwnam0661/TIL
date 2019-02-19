enum
-
> enum은 열거형(enumerated type)이라고 부른다. 열거형은 서로 연관된 상수들의 집합이라고 할 수 있다.
```java
package org.opentutorials.javatutorials.constant2;
 
enum Fruit{
    APPLE, PEACH, BANANA;
}
enum Company{
    GOOGLE, APPLE, ORACLE;
}
 
public class ConstantDemo {
     
    public static void main(String[] args) {
        /*
        if(Fruit.APPLE == Company.APPLE){
            System.out.println("과일 애플과 회사 애플이 같다.");
        }
        */
        Fruit type = Fruit.APPLE;
        switch(type){
            case APPLE:
                System.out.println(57+" kcal");
                break;
            case PEACH:
                System.out.println(34+" kcal");
                break;
            case BANANA:
                System.out.println(93+" kcal");
                break;
        }
    }
}
```
> 위의 코드를 하나씩 살펴보자.
```java
enum Fruit{
    APPLE, PEACH, BANANA;
}
```
> enum은 class, interface와 동급의 형식을 가지는 단위다. 하지만 enum은 사실상 class이다. 편의를 위해서 enum만을 위한 문법적 형식을 가지고 있기 때문에 구분하기 위해서 enum이라는 키워드를 사용하는 것이다. 위의 코드는 아래 코드와 사실상 같다.
```java
class Fruit{
    public static final Fruit APPLE  = new Fruit();
    public static final Fruit PEACH  = new Fruit();
    public static final Fruit BANANA = new Fruit();
    private Fruit(){}
}
```
> 생성자의 접근 제어자가 private이다. 그것이 클래스 Fruit를 인스턴스로 만들 수 없다는 것을 의미한다. 다른 용도로 사용하는 것을 금지하고 있는 것이다. 이에 대해서는 뒤에서 다시 설명하겠다. enum은 많은 곳에서 사용하던 디자인 패턴을 언어가 채택해서 문법적인 요소로 단순화시킨 것이라고 할 수 있다.

> 아래 코드는 컴파일 에러가 발생한다.
```java
/*
if(Fruit.APPLE == Company.APPLE){
    System.out.println("과일 애플과 회사 애플이 같다.");
}
*/
```
> enum이 서로 다른 상수 그룹에 대한 비교를 컴파일 시점에서 차단할 수 있다는 것을 의미한다. 상수 그룹 별로 클래스를 만든 것의 효과를 enum도 갖는다는 것을 알 수 있다.

> enum을 사용하는 이유를 정리하면 아래와 같다.

* 코드가 단순해진다.
* 인스턴스 생성과 상속을 방지한다.
* 키워드 enum을 사용하기 때문에 구현의 의도가 열거임을 분명하게 나타낼 수 있다.

enum과 생성자
-
> enum은 사실 클래스다. 그렇기 때문에 생성자를 가질 수 있다. 아래와 같이 코드를 수정해보자.
```java
package org.opentutorials.javatutorials.constant2;
 
enum Fruit{
    APPLE, PEACH, BANANA;
    Fruit(){
        System.out.println("Call Constructor "+this);
    }
}
 
enum Company{
    GOOGLE, APPLE, ORACLE;
}
 
public class ConstantDemo {
     
    public static void main(String[] args) {
     
        /*
        if(Fruit.APPLE == Company.APPLE){
            System.out.println("과일 애플과 회사 애플이 같다.");
        }
        */
        Fruit type = Fruit.APPLE;
        switch(type){
            case APPLE:
                System.out.println(57+" kcal");
                break;
            case PEACH:
                System.out.println(34+" kcal");
                break;
            case BANANA:
                System.out.println(93+" kcal");
                break;
        }
    }
}
```
> 결과는 아래와 같다.
```java
Call Constructor APPLE
Call Constructor PEACH
Call Constructor BANANA
57 kcal
```
> Call Constructor가 출력된 것은 생성자 Fruit가 호출되었음을 의미한다. 이것이 3번 호출되었다는 것은 필드의 숫자만큼 호출되었다는 뜻이다. 즉 enum은 생성자를 가질 수 있다.

> 하지만 코드를 아래와 같이 바꾸면 컴파일 에러가 발생한다.
```java
enum Fruit{
    APPLE, PEACH, BANANA;
    public Fruit(){
        System.out.println("Call Constructor "+this);
    }
}
```
> 이것은 enum의 생성자가 접근 제어자 private만을 허용하기 때문이다. 덕분에 Fruit를 직접 생성할 수 없다. 그렇다면 이 생성자의 매개변수를 통해서 필드(APPLE..)의 인스턴스 변수 값을 부여 할 수 있다는 말일까? 있다. 그런데 방식이 좀 생경하다.
```java
package org.opentutorials.javatutorials.constant2;
 
enum Fruit{
    APPLE("red"), PEACH("pink"), BANANA("yellow");
    public String color;
    Fruit(String color){
        System.out.println("Call Constructor "+this);
        this.color = color;
    }
}
 
enum Company{
    GOOGLE, APPLE, ORACLE;
}
 
public class ConstantDemo {
     
    public static void main(String[] args) {
        /*
        if(Fruit.APPLE == Company.APPLE){
            System.out.println("과일 애플과 회사 애플이 같다.");
        }
        */
        Fruit type = Fruit.APPLE;
        switch(type){
            case APPLE:
                System.out.println(57+" kcal, "+Fruit.APPLE.color);
                break;
            case PEACH:
                System.out.println(34+" kcal"+Fruit.PEACH.color);
                break;
            case BANANA:
                System.out.println(93+" kcal"+Fruit.BANANA.color);
                break;
        }
    }
}
```
> 실행결과는 아래와 같다.
```java
Call Constructor APPLE
Call Constructor PEACH
Call Constructor BANANA
57 kcal, red
```
> 아래 코드는 Fruit의 상수를 선언하면서 동시에 생성자를 호출하고 있다.
```java
APPLE("red"), PEACH("pink"), BANANA("yellow");
```
> 아래 코드는 생성자다. 생성자의 매개변수로 전달된 값은 this.color를 통해서 5행의 인스턴스 변수의 값으로 할당된다.
```java
Fruit(String color){
    System.out.println("Call Constructor "+this);
    this.color = color;
}
```
> 아래처럼 호출하면 APPLE에 할당된 Fruit 인스턴스의 color 필드를 반환하게 된다.
```java
System.out.println(57+" kcal, "+Fruit.APPLE.color);
```
> 열거형은 메소드를 가질수도 있다. 아래 코드는 이전 예제와 동일한 결과를 출력한다.
```java
package org.opentutorials.javatutorials.constant2;
 
enum Fruit{
    APPLE("red"), PEACH("pink"), BANANA("yellow");
    private String color;
    Fruit(String color){
        System.out.println("Call Constructor "+this);
        this.color = color;
    }
    String getColor(){
        return this.color;
    }
}
 
enum Company{
    GOOGLE, APPLE, ORACLE;
}
 
public class ConstantDemo {
     
    public static void main(String[] args) {
        Fruit type = Fruit.APPLE;
        switch(type){
            case APPLE:
                System.out.println(57+" kcal, "+Fruit.APPLE.getColor());
                break;
            case PEACH:
                System.out.println(34+" kcal"+Fruit.PEACH.getColor());
                break;
            case BANANA:
                System.out.println(93+" kcal"+Fruit.BANANA.getColor());
                break;
        }
    }
}
```
> enum은 맴버 전체를 열거 할 수 있는 기능도 제공한다.
```java
package org.opentutorials.javatutorials.constant2;
 
enum Fruit{
    APPLE("red"), PEACH("pink"), BANANA("yellow");
    private String color;
    Fruit(String color){
        System.out.println("Call Constructor "+this);
        this.color = color;
    }
    String getColor(){
        return this.color;
    }
}
 
enum Company{
    GOOGLE, APPLE, ORACLE;
}
 
public class ConstantDemo {
     
    public static void main(String[] args) {
        for(Fruit f : Fruit.values()){
            System.out.println(f+", "+f.getColor());
        }
    }
}
```
> 열거형의 특성을 정리해보자. 열거형은 연관된 값들을 저장한다. 또 그 값들이 변경되지 않도록 보장한다. 뿐만 아니라 열거형 자체가 클래스이기 때문에 열거형 내부에 생성자, 필드, 메소드를 가질 수 있어서 단순히 상수가 아니라 더 많은 역할을 할 수 있다.
