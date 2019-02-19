예외 3 - 만들기
=
소비자에서 생산자로
-
> 지금까지 API의 소비자로서 API 측에서 던지는 예외를 처리하는 방법을 알아봤다. 이번에는 API의 생산자로서 소비자들에게 API를 제공하는 입장에 서보자. 전 시간에 사용했던 코드를 바탕으로 이야기를 풀어가자.
```java
package org.opentutorials.javatutorials.exception;
class Calculator{
    int left, right;
    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    }
    public void divide(){
        try {
            System.out.print("계산결과는 ");
            System.out.print(this.left/this.right);
            System.out.print(" 입니다.");
        } catch(Exception e){
            System.out.println("\n\ne.getMessage()\n"+e.getMessage());
            System.out.println("\n\ne.toString()\n"+e.toString());
            System.out.println("\n\ne.printStackTrace()");
            e.printStackTrace();
        }
    }
} 
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        c1.setOprands(10, 0);
        c1.divide();
    }
}
```
> 위의 코드에서 예외가 발생한 이유는 10을 0으로 나누려고 하기 때문이다. 즉 setOprands를 통해서 입력된 두번째 인자의 값이 0이기 때문에 발생한 문제다. 우리가 할 수 있는 조치는 두가지다.

* setOprands의 두번째 인자로 0이 들어오면 예외를 발생시킨다.
* 메소드 divide를 실행할 때 right의 값이 0이라면 예외를 발생시킨다.

> 첫번째 방법을 적용해보자.
```java
package org.opentutorials.javatutorials.exception;
class Calculator{
    int left, right;
    public void setOprands(int left, int right){
        if(right == 0){
            throw new IllegalArgumentException("두번째 인자의 값은 0이 될 수 없습니다.");
        }
        this.left = left;
        this.right = right;
    }
    public void divide(){
        try {
            System.out.print("계산결과는 ");
            System.out.print(this.left/this.right);
            System.out.print(" 입니다.");
        } catch(Exception e){
            System.out.println("\n\ne.getMessage()\n"+e.getMessage());
            System.out.println("\n\ne.toString()\n"+e.toString());
            System.out.println("\n\ne.printStackTrace()");
            e.printStackTrace();
        }
    }
} 
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        c1.setOprands(10, 0);
        c1.divide();
    }
}
```
> 실행 결과는 아래와 같다.
```java
Exception in thread "main" java.lang.IllegalArgumentException: 두번째 인자의 값은 0이 될 수 없습니다.
    at org.opentutorials.javatutorials.exception.Calculator.setOprands(CalculatorDemo.java:6)
    at org.opentutorials.javatutorials.exception.CalculatorDemo.main(CalculatorDemo.java:27)
```
> 두번째 인자의 값이 0이 되었을 때 setOprands의 사용자에게 예외 클래스인 IllegalArgumentException을 던지고 있다. 사용자인 main은 예외와 함께 '두번째 인자의 값은 0이 될 수 없습니다.'라는 메시지를 받게 되고 이 정보를 바탕으로 전달 값을 변경하게 된다.

> 아래와 같이 divide 내에서 예외를 처리할 수도 있다.
```java
package org.opentutorials.javatutorials.exception;
class Calculator{
    int left, right;
    public void setOprands(int left, int right){        
        this.left = left;
        this.right = right;
    }
    public void divide(){
        if(this.right == 0){
            throw new ArithmeticException("0으로 나누는 것은 허용되지 않습니다.");
        }
        try {
            System.out.print("계산결과는 ");
            System.out.print(this.left/this.right);
            System.out.print(" 입니다.");
        } catch(Exception e){
            System.out.println("\n\ne.getMessage()\n"+e.getMessage());
            System.out.println("\n\ne.toString()\n"+e.toString());
            System.out.println("\n\ne.printStackTrace()");
            e.printStackTrace();
        }
    }
} 
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        c1.setOprands(10, 0);
        c1.divide();
    }
}
```
> throw는 예외를 발생시키는 명령이다. throw 뒤에는 예외 정보를 가지고 있는 예외 클래스가 위치한다. 자바 가상 머신은 이 클래스를 기준으로 어떤 catch 구문을 실행할 것인지를 결정한다. 또 실행되는 catch 구문에서는 예외 클래스를 통해서 예외 상황의 원인에 대한 다양한 정보를 얻을 수 있다. 이 정보를 바탕으로 문제를 해결하게 된다.

> 필자가 사용한 예외인 IllegalArgumentException, ArithmeticException은 자바에서 기본적으로 제공하는 예외다. 이러한 예외들은 자바 가상머신이 사용하기도 하고 또 응용 프로그램 개발자가 사용할 수도 있다. 기본적으로 제공되는 어떤 예외들이 있는지를 파악하고 적당한 예외를 사용하는 것은 중요한 문제다. 클래스 Exception을 API 문서에서 찾아보고 그 하위 클래스로 어떤 것들이 있는지 살펴보는 것도 도움이 된다. 다음은 기억할만한 주요 Exception들의 리스트다. 

|예외|사용해야 할 상황
|:--------|:--------|
|IllegalArgumentException| 매개변수가 의도하지 않은 상황을 유발시킬 때| 
|IllegalStateException| 메소드를 호출하기 위한 상태가 아닐 때| 
|NullPointerException| 매개 변수 값이 null 일 때| 
|IndexOutOfBoundsException| 인덱스 매개 변수 값이 범위를 벗어날 때|
|ArithmeticException| 산술적인 연산에 오류가 있을 때| 

예외의 여러가지 상황들
-
> 예외에 대한 시야를 넓히기 위해서 몇가지 상황들을 간단한 코드로 만들어 보았다.
```java
package org.opentutorials.javatutorials.exception;
class E{
    void throwArithmeticException(){
        throw new ArithmeticException();
    }
}
```
> 문제없이 컴파일이 될 것이다. 이번엔 아래와 같이 코드를 바꿔보자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.IOException;
class E{
    void throwArithmeticException(){
        throw new ArithmeticException();
    }
    void throwIOException(){
        throw new IOException();
    }
}
```
> 이번에는 컴파일 되지 않을 것이다. 에러 내용은 아래와 같다. 왜 그럴까?
```java
src\org\opentutorials\javatutorials\exception\ThrowExceptionDemo2.java:8: error: unreported exception IOException; must be caught or declared to be thrown
                throw new IOException();
                ^
1 error
```
> 에러에는 아래와 같이 적혀있다.

> unreported exception IOException; must be caught or declared to be thrown

> 즉 IOException은 try...catch하거나 throw 해야 한다는 뜻이다.

> 이상하다 ArithmeticException, IOException 모두 Exception이다. 그럼에도 불구하고 유독 IOException만 try...catch 혹은 throw 를 해야 한다는 뜻이다. 자바는 이 두개의 예외를 다른 방법으로 대하고 있는 것이다. 우선 아래와 같이 코드를 변경해서 에러를 없애자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.IOException;
class E{
    void throwArithmeticException(){
        throw new ArithmeticException();
    }
    void throwIOException1(){
        try {
            throw new IOException();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    void throwIOException2() throws IOException{
        throw new IOException();
    }
}
```
> 예제의 핵심은 IOException은 예외처리를 강제하고 있지만 ArithmeticException은 그렇지 않다 점이다. 그 이유를 알아보자.

예외의 선조 - Throwable
-
> 우선 [ArithmeticException](https://docs.oracle.com/javase/7/docs/api/java/lang/ArithmeticException.html)의 API 문서를 통해서 예외들의 가계도를 살펴보자. 아래 그림은 API 문서의 일부를 캡처한 것이다.


![2061](https://user-images.githubusercontent.com/23206749/52991902-ffcb0000-3451-11e9-9c0a-c73aba1c1b1e.png)
> 이것을 통해서 ArithmeticException의 부모 클래스 중에 java.lang.Exception 클래스가 있다는 사실을 알 수 있다. ArithmeticException 클래스는 Exception 클래스의 하위 클래스였던 것이다. 그렇기 때문에 Exception 클래스가 더 많은 예외 상황을 포괄하는 것이고 ArithmeticException 클래스는 더 구체적인 상황을 특정하는 것이다. 잘 이해가 안 되면 이전에 소개했던 예제를 보자.
```java
try {
    System.out.println(arr[first] / arr[second]);
} catch(ArrayIndexOutOfBoundsException e){
    System.out.println("ArrayIndexOutOfBoundsException");
} catch(ArithmeticException e){
    System.out.println("ArithmeticException");
} catch(Exception e){
    System.out.println("Exception");
}
```
> 그리고 상속 관계를 자세히 보면 java.lang.Throwable 클래스가 있다. 이 클래스를 클릭해서 [Throwable 클래스의 페이지](https://docs.oracle.com/javase/7/docs/api/java/lang/Throwable.html)로 이동해보면 아래와 같은 내용을 발견할 수 있다.

![2063](https://user-images.githubusercontent.com/23206749/52991212-1754b980-344f-11e9-97a6-7da673d6348e.png)
> 우리가 지금까지 사용했던 getMessage, printStackTrace, toString이 Throwable 클래스에서 정의 되어 있었던 것이다! 또 이 클래스의 이름이 Throwable이다. '던질 수 있는'이라는 뜻이다. 즉 예외로 '던질 수 있는' 클래스는 반드시 Throwable 클래스를 상 받아야 한다.

예외의 종류
-
> 그럼 위에서 이야기한 것을 개념적으로 정리해보자. 우선 중요한 예외 클래스들은 아래와 같다.

* Throwable
* Error
* Exception
* Runtime Exception

> 이 클래스 간의 상속 관계를 그림으로 나타내면 아래와 같다.

![2099](https://user-images.githubusercontent.com/23206749/52992088-a6af9c00-3452-11e9-916b-1f93f885683e.png)

Throwable
-
> 클래스 Throwable은 범 예외 클래스들의 공통된 조상이다. 모든 예외 클래스들이 가지고 있는 공통된 메소드를 정의하고 있다. 중요한 역할을 하는 클래스임에는 틀림없지만 이 클래스를 직접 사용하지는 않기 때문에 우리에게는 중요하지 않다.

Error
-
> 에러는 여러분의 애플리케이션의 문제가 아니라 그 애플리케이션이 동작하는 가상머신에 문제가 생겼을 때 발생하는 예외다. 애플리케이션을 구동시키기에는 메모리가 부족한 경우가 이에 속한다. 이런 경우는 애플리케이션 개발자가 할 수 있는 것이 없다. 따라서 예외처리를 하지 말고 그냥 에러로 인해서 애플리케이션이 중단되도록 내버려둔다. 대신 자신의 애플리케이션이 메모리를 과도하게 사용하고 있다면 로직을 변경하거나 자바 가상머신에서 사용하는 메모리의 제한을 변경하는 등의 대응을 한다.

Exception
-
> 결국 우리의 관심사는 Exception 클래스와 RuntimeException 클래스로 좁혀진다. 우선 [Exception 클래스의 하위 클래스들의 목록](https://docs.oracle.com/javase/7/docs/api/java/lang/Exception.html)을 살펴보자. 아래 클래스들은 모두 Exception 클래스를 상속한 예외 클래스다.

![2065](https://user-images.githubusercontent.com/23206749/52992169-ebd3ce00-3452-11e9-9064-86b1be8423cb.png)

> 필자가 강조 표시한 부분을 보자. 저 많은 클래스 중의 하나가 RuntimeException이다. 도대체 RuntimeException 클래스는 어떤 특이점이 있길래 부모 클래스인 Exception 클래스와 함께 언급되는 것일까? RuntimeException을 제외한 Exception 클래스의 하위 클래스들과 RuntimeException 클래스의 차이를 자바에서는 checked와 unckecked라고 부른다. 관계를 정리하면 아래와 같다.

* checked 예외 - RuntimeException을 제외한 Exception의 하위 클래스
* unchekced 예외 - RuntimeException의 하위 클래스

> checked 예외는 반드시 예외처리를 해야 하는 되는 것이고, unchekced는 해도 되고 안 해도 되는 예외다. 바로 이 지점이 IOException과 ArithmeticException의 차이점이다. 아래는 두개 클래스들의 가계도를 보여준다

![2093](https://user-images.githubusercontent.com/23206749/52992276-61d83500-3453-11e9-9408-fe9db73ba08c.png)
> 강조 표시한 부분을 주의 깊게 살펴보자. ArithmeticException의 부모 중에 RuntimeException이 있다. 반면에 IOException은 Exception의 자식이지만 RuntimeException의 자식은 아니다. 이런 이유로 IOException은 checked이고 ArithmeticException은 unchekced이다. (Error도 unchecked이다)

나만의 예외 만들기
-
> 표준 예외 클래스로도 많은 예외 상황을 표현할 수 있다. 하지만 그렇지 않은 경우도 있을 것이다. 이런 때는 직접 예외를 만들면 된다.

> 예외를 만들기 전에 해야 할 것은 자신의 예외를 checked로 할 것인가? unchecked로 할 것인가를 정해야 한다. 그 기준은 모호한 문제다. 하지만 기준이 없는 것도 아니다.

> API 쪽에서 예외를 던졌을 때 API 사용자 쪽에서 예외 상황을 복구 할 수 있다면 checked 예외를 사용한다. checked 예외는 사용자에게 문제를 해결할 기회를 주는 것이면서 예외처리를 강제하는 것이다. 하지만 checked 예외를 너무 자주 사용하면 API 사용자를 몹시 힘들게 할 수 있기 때문에 적정선을 찾는 것이 중요하다.

> 사용자가 API의 사용방법을 어겨서 발생하는 문제거나 예외 상황이 이미 발생한 시점에서 그냥 프로그램을 종료하는 것이 덜 위험 할 때 unchecked를 사용한다.  기존의 ArithmeticException을 직접 만든 Exception으로 교체해보자.
```java
package org.opentutorials.javatutorials.exception;
class DivideException extends RuntimeException {
    DivideException(){
        super();
    }
    DivideException(String message){
        super(message);
    }
}
class Calculator{
    int left, right;
    public void setOprands(int left, int right){        
        this.left = left;
        this.right = right;
    }
    public void divide(){
        if(this.right == 0){
            throw new DivideException("0으로 나누는 것은 허용되지 않습니다.");
        }
        System.out.print(this.left/this.right);
    }
}
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        c1.setOprands(10, 0);
        c1.divide();
    }
}
```
> 만약 DivideException을 Exception으로 바꾸면 어떻게 될까? 아래 코드의 RuntimeException을 Exception으로 변경하면 된다.
```java
class DivideException extends RuntimeException {
```
```java
class DivideException extends Exception {
```
> 아래와 같이 컴파일 에러가 발생한다.
```java
src\org\opentutorials\javatutorials\exception\CalculatorDemo.java:18: error: unreported exception DivideException; must be caught or declared to be thrown
                throw new DivideException("0으로 나누는 것은 허용되지 않습니다.");
                ^
1 error
```
> 이를 해결하려면 두가지 방법이 있다. 하나는 예외처리를 하는 것이다. 문법을
```java
package org.opentutorials.javatutorials.exception;
class DivideException extends Exception {
    DivideException(){
        super();
    }
    DivideException(String message){
        super(message);
    }
}
class Calculator{
    int left, right;
    public void setOprands(int left, int right){        
        this.left = left;
        this.right = right;
    }
    public void divide(){
        if(this.right == 0){
            try {
                throw new DivideException("0으로 나누는 것은 허용되지 않습니다.");
            } catch (DivideException e) {
                e.printStackTrace();
            }
        }
        System.out.print(this.left/this.right);
    }
}
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        c1.setOprands(10, 0);
        c1.divide();
    }
}
```
> 혹은 사용자에게 예외를 던진다. 사용자는 반드시 예외에 대한 처리를 해야 한다.
```java
package org.opentutorials.javatutorials.exception;
 
class DivideException extends Exception {
    DivideException(){
        super();
    }
    DivideException(String message){
        super(message);
    }
}
class Calculator{
    int left, right;
    public void setOprands(int left, int right){        
        this.left = left;
        this.right = right;
    }
    public void divide() throws DivideException{
        if(this.right == 0){
            throw new DivideException("0으로 나누는 것은 허용되지 않습니다.");
        }
        System.out.print(this.left/this.right);
    }
}
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        c1.setOprands(10, 0);
        try {
            c1.divide();
        } catch (DivideException e) {
            e.printStackTrace();
        }
    }
}
```

