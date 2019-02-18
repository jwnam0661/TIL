예외란?
-
> 프로그래밍을 하면 많은 오류 상황에 직면하게 된다. 기능이 많아질수록 오류가 발생할 확률은 기하급수적으로 증가한다. 자연스럽게 오류를 잘 처리하기 위한 방법들이 필요해지게 된다. 예외(Exception)란 프로그램을 만든 프로그래머가 상정한 정상적인 처리에서 벗어나는 경우에 이를 처리하기 위한 방법이다. 자바는 예기치 못한 오류를 어떻게 처리하는가를 알아보자.

> 계산기 예제로 시작해보자. 아래는 기존의 더하기(sum), 평균(avg) 메소드를 제거하고 나누기 메소드를 추가한 예제다.

```java
package org.opentutorials.javatutorials.exception;
class Calculator{
    int left, right;
    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    }
    public void divide(){
        System.out.print("계산결과는 ");
        System.out.print(this.left/this.right);
        System.out.print(" 입니다.");
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
> '계산결과'가 출력되었다는 의미는 9행은 실행이 되었다는 의미다. 10행에서 오류가 발생한 이유는 10을 0으로 나누려고 했기 때문이다.
```java
계산결과는 Exception in thread "main" java.lang.ArithmeticException: / by zero
```
> 아래의 내용은 오류가 발생한 순서를 추적하고 있는데 제일 아래쪽이 문제의 로직을 호출한 부분이고 가장 위쪽이 문제의 원인이다.
```java
at org.opentutorials.javatutorials.exception.Calculator.divide(CalculatorDemo.java:12)
at org.opentutorials.javatutorials.exception.CalculatorDemo.main(CalculatorDemo.java:22)
```
> 에러 메시지는 시스템에서 어떤 오류가 발생했을 때 그 오류를 이해하고 파악하는데 큰 도움이 된다.

> 다시 코드로 돌아와서 이 문제를 어떻게 해결할까 생각해보자. 아래와 같이 코드를 변경해보자.
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
            System.out.println("오류가 발생했습니다 : "+e.getMessage());
        }
    }
} 
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator c1 = new Calculator();
        c1.setOprands(10, 0);
        c1.divide();
         
        Calculator c2 = new Calculator();
        c2.setOprands(10, 5);
        c2.divide();
    }
}
```
> 결과는 다음과 같다.
```java
계산결과는 오류가 발생했습니다 : / by zero
계산결과는 2 입니다.
```
> 즉 10행은 실행이 되었지만 11행은 실행되지 않았다. 그런데 이전 예제와 다른 점이 있다. 오류가 발생하지 않았다는 점이다. 마치 정상적인 에플리케이션인 것처럼 동작하고 있다. 그 이유는 try...catch 때문이다.

try...catch
-
> try...catch는 예외에서 핵심적인 역할을 담당하는 문법적인 요소다. 형식을 살펴보자.

try
-
> try 안에는 예외 상황이 발생할 것으로 예상되는 로직을 위치시킨다. 예제에서는 사용자가 setOprands의 두 번째 인자로 숫자 0을 입력했을 때 문제가 발생할 수 있음을 예측할 수 있다. 그래서 이 로직을 try 구문으로 감싼 것이다.

catch
-
> catch 안에는 예외가 발생했을 때 뒷수습을 하기 위한 로직이 위치한다. 예제의 실행결과를 다시 살펴보자. 아래와 같다.
```java
계산결과는 오류가 발생했습니다
```
> 이것은 아래 구문에서 오류가 발생하면서 try 내의 실행이 중단되고 catch 구문 안의 내용이 실행되었음을 의미한다.
```java
System.out.print(this.left/this.right);
```
예외 클래스와 인스턴스
-
```java
} catch(Exception e){
    System.out.println("오류가 발생했습니다 : "+e.getMessage());
}
```
> e는 변수다. 이 변수 앞의 Exception은 변수의 데이터 타입이 Exception이라는 의미다. Exception은 자바에서 기본적으로 제공하는 클래스로 java.lang에 소속되어 있다. 예외가 발생하면 자바는 마치 메소드를 호출하듯이 catch를 호출하면서 그 인자로 Exception 클래스의 인스턴스를 전달하는 것이다.

> e.getMessage()는 자바가 전달한 인스턴스의 메소드 중 getMessage를 호출하는 코드인데, getMessage는 오류의 원인을 사람이 이해하기 쉬운 형태로 리턴하도록 약속되어 있다.

뒷수습의 방법
-
> 예외의 핵심은 뒷수습이다. 하지만 제대로 된 수습은 대단히 어려운 문제이다. 여기서는 자바에서 기본적으로 제공하는 뒷수습의 방법만 알아본다.

> 코드를 조금 바꿔보자.
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
> 결과는 아래와 같다.
```java
계산결과는 
 
e.getMessage()
/ by zero
 
 
e.toString()
java.lang.ArithmeticException: / by zero
 
 
e.printStackTrace()
java.lang.ArithmeticException: / by zero
    at org.opentutorials.javatutorials.exception.Calculator.divide(CalculatorDemo.java:11)
    at org.opentutorials.javatutorials.exception.CalculatorDemo.main(CalculatorDemo.java:25)
```
e.getMessage();
-
> 오류에 대한 기본적인 내용을 출력해준다. 상세하지 않다.

e.toString();
-
> e.toString()을 호출한 결과는 java.lang.ArithmeticException: / by zero 이다. e.toString()은 e.getMessage()보다 더 자세한 예외 정보를 제공한다. java.lang.ArithmeticException은 발생한 예외가 어떤 예외에 해당하는지에 대한 정보라고 지금을 생각하자. ArithmeticException 수학적인 계산의 과정에서 발생하는 예외상황을 의미한다. (우리는 어떤 숫자를 0으로 나누려고 하고 있다는 것을 상기하자)

e.printStackTrace();
-
> 메소드 getMessage, toString과는 다르게 printStackTrace는 리턴값이 없다. 이 메소드를 호출하면 메소드가 내부적으로 예외 결과를 화면에 출력한다. printStackTrace는 가장 자세한 예외 정보를 제공한다.

다양한 예외들
-
> 이번에는 좀 더 다양한 예외 상황을 처리하는 방법을 알아보자. 코드를 보자.
```java
package org.opentutorials.javatutorials.exception;
 
class A{
    private int[] arr = new int[3];
    A(){
        arr[0]=0;
        arr[1]=10;
        arr[2]=20;
    }
    public void z(int first, int second){
        System.out.println(arr[first] / arr[second]);
    }
}
 
public class ExceptionDemo1 {
    public static void main(String[] args) {
        A a = new A();
        a.z(10, 1);
    }
}
```
> 위의 결과는 다음과 같다.
```java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 10
    at org.opentutorials.javatutorials.exception.A.z(ExceptionDemo1.java:11)
    at org.opentutorials.javatutorials.exception.ExceptionDemo1.main(ExceptionDemo1.java:18)
```
> 이유를 따져보자. 배열 arr은 3개의 값을 담을 수 있다.
```java
private int[] arr = new int[3];
```
> 하지만 10번째 인덱스를 호출하고 있다.
```java
a.z(10, 1);
```
```java
System.out.println(arr[first] / arr[second]);
```
> 따라서 존재하지 않는 값을 가져오려고 시도하고 있기 때문에 자바에서는 ArrayIndexOutOfBoundsException을 발생시킨 것이다.

> 위의 코드를 조금 변경해보자.
```java
package org.opentutorials.javatutorials.exception;
 
class A{
    private int[] arr = new int[3];
    A(){
        arr[0]=0;
        arr[1]=10;
        arr[2]=20;
    }
    public void z(int first, int second){
        System.out.println(arr[first] / arr[second]);
    }
}
 
public class ExceptionDemo1 {
    public static void main(String[] args) {
        A a = new A();
        a.z(1, 0);
    }
}
```
> 결과는 아래와 같다.
```java
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at org.opentutorials.javatutorials.exception.A.z(ExceptionDemo1.java:11)
    at org.opentutorials.javatutorials.exception.ExceptionDemo1.main(ExceptionDemo1.java:18)
```
> 위의 코드는 메소드 z 내부적으로 10/0을 실행하게 된다. 0으로 나누는 것은 불가능하기 때문에 자바는 ArithmeticException을 발생시킨다.

> 위의 예제를 통해서 보여주고 싶은 것은 같은 로직이지만 상황에 따라서 다른 예외가 발생할 수 있다는 것이다. 이런 경우는 어떻게 예외를 처리해야 할까? 예외 처리를 추가한 아래의 예제를 보자.
```java
package org.opentutorials.javatutorials.exception;
 
class A{
    private int[] arr = new int[3];
    A(){
        arr[0]=0;
        arr[1]=10;
        arr[2]=20;
    }
    public void z(int first, int second){
        try {
            System.out.println(arr[first] / arr[second]);
        } catch(ArrayIndexOutOfBoundsException e){
            System.out.println("ArrayIndexOutOfBoundsException");
        } catch(ArithmeticException e){
            System.out.println("ArithmeticException");
        } catch(Exception e){
            System.out.println("Exception");
        }
         
    }
}
 
public class ExceptionDemo1 {
    public static void main(String[] args) {
        A a = new A();
        a.z(10, 0);
        a.z(1, 0);
        a.z(2, 1);
    }
}
```
> 결과는 다음과 같다.
```java
ArrayIndexOutOfBoundsException
ArithmeticException
2
```
> 예제는 다중 catch를 보여준다. 조건문의 else if처럼 여러 개의 catch를 하나의 try 구문에서 사용할 수 있다. 이를 통해서 보다 간편하게 다양한 상황에 대응할 수 있다. 위의 코드는 try 구문에서 예외가 발생했을 때 그 예외의 종류가 ArrayIndexOutOfBoundsException이라면 14행이 실행되고, ArithemeticException이라면 16행이 실행되고 그 외의 것이라면 18행이 실행된다는 의미다.

finally
-
>  finally는 try 구문에서 예외가 발생하는 것과 상관없이 언제나 실행되는 로직이다. 예제를 조금 변경해보자.
```java
package org.opentutorials.javatutorials.exception;
 
class A{
    private int[] arr = new int[3];
    A(){
        arr[0]=0;
        arr[1]=10;
        arr[2]=20;
    }
    public void z(int first, int second){
        try {
            System.out.println(arr[first] / arr[second]);
        } catch(ArrayIndexOutOfBoundsException e){
            System.out.println("ArrayIndexOutOfBoundsException");
        } catch(ArithmeticException e){
            System.out.println("ArithmeticException");
        } catch(Exception e){
            System.out.println("Exception");
        } finally {
            System.out.println("finally");
        }
    }
}
 
public class ExceptionDemo1 {
    public static void main(String[] args) {
        A a = new A();
        a.z(10, 0);
        a.z(1, 0);
        a.z(2, 1);
    }
}
```
> 실행결과는 다음과 같다.
```java
ArrayIndexOutOfBoundsException
finally
ArithmeticException
finally
2
finally
```
> 예외와 상관없이 try 내의 구문이 실행되면 finally가 실행되고 있다.

> 그럼 finally는 언제 사용하는 것일까? 어떤 작업의 경우는 예외와는 상관없이 반드시 끝내줘야 하는 작업이 있을 수 있다. 하지면 지금 우리의 상황에서는 그런 사례를 살펴보는 것이 오히려 부담이 될 수 있을 것 같다. 그래서 여기서는 케이스를 설명만 하고 예제를 만들지는 않겠다.

> 예를 들어 데이터베이스를 사용한다면 데이터베이스 서버에 접속해야 한다. 이때 데이터베이스 서버와 여러분이 작성한 에플리케이션은 서로 접속상태를 유지하게 되는데 데이터베이스를 제어하는 과정에서 예외가 발생해서 더 이상 후속 작업을 수행하는 것이 불가능한 경우가 있을 수 있다. 예외가 발생했다고 데이터베이스 접속을 끊지 않으면 데이터베이스와 연결 상태를 유지하게 되고 급기야 데이터베이스는 더 이상 접속을 수용할 수 없는 상태에 빠질 수 있다. 접속을 끊는 작업은 예외 발생여부와 상관없기 때문에 finally에서 처리하기에 좋은 작업이라고 할 수 있다. 말하자면 finally는 작업의 뒷정리를 담당한다고 볼 수 있다.
