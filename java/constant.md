상수
-
> 상수는 변하지 않는 값이다. 아래에서 좌항이 변수이고 우항이 상수이다.
```java
int x = 1;
```
> 아래와 같은 구문은 있을 수 없다. 1은 2가 될 수 없다.
```java
1 = 2;
```
> 상수의 이런 특성을 이용해서 아래와 같은 로직을 작성할 수 있다.
```java
package org.opentutorials.javatutorials.constant2;
 
public class ConstantDemo {
    public static void main(String[] args) {
        /*
         * 1. 사과
         * 2. 복숭아
         * 3. 바나나
         */
        int type = 1;
        switch(type){
            case 1:
                System.out.println(57);
                break;
            case 2:
                System.out.println(34);
                break;
            case 3:
                System.out.println(93);
                break;
        }
    }
 
}
```
> 위와 같은 로직에서 숫자 1에 해당하는 과일은 언제나 사과여야 한다. 그러므로 변하지 않는 값인 상수값에 따라서 그 값에 해당하는 과일의 의미를 고정하고 있다. 그런데 주석으로 상수의 의미를 전달하고 있지만 주석이 없어졌거나, 주석이 상수를 사용하는 코드와 멀어진다면 각 숫자에 해당하는 과일이 무엇을 나타내는지 알아보기거 어렵거나 불가능해질 수 있다.

> 이런 때는 이름이 있다면 더 좋을 것이다. 변수도 상수가 될 수 있다. 변수를 지정하고 그 변수를 final로 처리하면 한번 설정된 변수의 값은 더 이상 바뀌지 않는다. 또한 바뀌지 않는 값이라면 인스턴스 변수가 아니라 클래스 변수(static)로 지정하는 것이 더 좋을 것이다.
```java
package org.opentutorials.javatutorials.constant2;
 
public class ConstantDemo {
    private final static int APPLE = 1;
    private final static int PEACH = 2;
    private final static int BANANA = 3;
    public static void main(String[] args) {
        int type = APPLE;
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
> 그런데 프로그램이 커지면서 누군가 기업에 대한 상수가 필요해졌다. 해서 아래와 같이 코드를 변경했다.
```java
package org.opentutorials.javatutorials.constant2;
 
public class ConstantDemo {
    // fruit
    private final static int APPLE = 1;
    private final static int PEACH = 2;
    private final static int BANANA = 3;
     
    // company
    private final static int GOOGLE = 1;
    //private final static int APPLE = 2;
    private final static int ORACLE = 3;
     
    public static void main(String[] args) {
        int type = APPLE;
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
> 과일 APPLE과 기업 APPLE이 서로 같은 이름을 가진다. 이렇게 되면 APPLE의 값이 2에서 1로 바뀐다. 프로그램은 오동작 할 것이다. 다행인 것은 final로 선언했기 때문에 컴파일이 되지 않고 이름이 중복되는 문제를 방지 할 수 있다. 그런데 이미 기업에 대한 상수를 작성했고 이것이 이미 다양한 영역에서 사용되고 있는 상태에서 APPLE을 추가하려면 낭패가 될 것이다. 이러한 문제를 회피하기 위해서 접두사를 붙여보자.
```java
package org.opentutorials.javatutorials.constant2;
 
public class ConstantDemo {
    // fruit
    private final static int FRUIT_APPLE = 1;
    private final static int FRUIT_PEACH = 2;
    private final static int FRUIT_BANANA = 3;
     
    // company
    private final static int COMPANY_GOOGLE = 1;
    private final static int COMPANY_APPLE = 2;
    private final static int COMPANY_ORACLE = 3;
     
    public static void main(String[] args) {
        int type = FRUIT_APPLE;
        switch(type){
            case FRUIT_APPLE:
                System.out.println(57+" kcal");
                break;
            case FRUIT_PEACH:
                System.out.println(34+" kcal");
                break;
            case FRUIT_BANANA:
                System.out.println(93+" kcal");
                break;
        }
    }
}
```
> 이름이 중복될 확률을 낮출 수 있다. 이러한 기법을 네임스페이스라고 한다. 그런데 상수가 너무 지저분하다. 좀 깔끔하게 바꿀 수 없을까? 이럴 때 *인터페이스*를 사용할 수 있다.
```java
package org.opentutorials.javatutorials.constant2;
 
interface FRUIT{
    int APPLE=1, PEACH=2, BANANA=3;
}
interface COMPANY{
    int GOOGLE=1, APPLE=2, ORACLE=3;
}
 
public class ConstantDemo {
     
    public static void main(String[] args) {
        int type = FRUIT.APPLE;
        switch(type){
            case FRUIT.APPLE:
                System.out.println(57+" kcal");
                break;
            case FRUIT.PEACH:
                System.out.println(34+" kcal");
                break;
            case FRUIT.BANANA:
                System.out.println(93+" kcal");
                break;
        }
    }
}
```
> 훨씬 깔끔하다. 접미사(FRUIT_, COMPANY_)로 이름을 구분했던 부분이 인터페이스로 구분되기 때문에 언어의 특성을 보다 잘 살린 구조가 되었다. 인터페이스를 이렇게 사용할 수 있는 것은 *인터페이스에서 선언된 변수는 무조건 public static final의 속성을 갖기 때문*이다.

> 그런데 type의 값으로 누군가 COMPANY_GOOGLE을 사용했다면 어떻게 될까? 13행을 아래와 같이 변경해보자.
```java
int type = COMPANY.GOOGLE;
```
> 결과는 57 Kcal이다! 구글이 57 Kcal인 것이다!

> 우리 코드는 과일과 기업이라는 두 개의 상수 그룹이 존재한다. 위의 코드는 서로 다른 상수그룹의 비교를 시도했고 양쪽 모두 값이 정수 1이기 때문에 오류를 사전에 찾아주지 못하고 있다. 컴파일러가 이런 실수를 사전에 찾아줄 수 있게 하려면 어떻게 해야 할까?
```java
package org.opentutorials.javatutorials.constant2;
 
class Fruit{
    public static final Fruit APPLE  = new Fruit();
    public static final Fruit PEACH  = new Fruit();
    public static final Fruit BANANA = new Fruit();
}
class Company{
    public static final Company GOOGLE = new Company();
    public static final Company APPLE = new Company();
    public static final Company ORACLE = new COMPANY(Company);
}
 
public class ConstantDemo {
     
    public static void main(String[] args) {
        if(Fruit.APPLE == Company.APPLE){
            System.out.println("과일 애플과 회사 애플이 같다.");
        }
    }
}
```
> Fruit와 Company 클래스를 만들고 클래스 변수로 해당 클래스의 인스턴스를 사용하고 있다. 각각의 변수가 final이기 때문에 불변이고, Static이므로 인스턴스로 만들지 않아도 된다. 결과는 17행에서 에러가 발생한다. 이것이 우리가 바라던 것이다. 서로 다른 카테고리의 상수에 대해서는 비교조차 금지하게 된 것이다. 언제나 오류는 컴파일 시에 나타나도록 하는 것이 바람직하다. 실행 중에 발생하는 오류는 찾아내기가 더욱 어렵다.

> 그런데 위의 코드는 두 가지 문제점이 있는데 하나는 switch 문에서 사용할 수 없고 또 하나는 선언이 너무 복잡하다는 것이다.
