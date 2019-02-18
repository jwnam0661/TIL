예외의 강제
-
> API를 사용할 때 설계자의 의도에 따라서 예외를 반드시 처리해야 하는 경우가 있다. 아래의 예제를 보자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
public class CheckedExceptionDemo {
    public static void main(String[] args) {
        BufferedReader bReader = new BufferedReader(new FileReader("out.txt"));
        String input = bReader.readLine();
        System.out.println(input); 
    }
}
```
> 어려운 코드다. 하지만 지금의 맥락에서는 중요한 내용이 아니다. 그래서 예외와 관련되지 않은 부분에 대한 자세한 설명은 하지 않겠다. 그냥 out.txt 파일을 읽어서 그것을 화면에 출력하는 내용이라고만 이해하자. 이 코드를 실행시키려면 out.txt 파일을 프로젝트의 루트 디렉토리에 위치시켜야 한다.위의 코드를 컴파일해보면 아래와 같은 에러가 발생하면서 컴파일이 되지 않을 것이다.
```java
Exception in thread "main" java.lang.Error: Unresolved compilation problems: 
    Unhandled exception type FileNotFoundException
    Unhandled exception type IOException
 
    at org.opentutorials.javatutorials.exception.CheckedExceptionDemo.main(CheckedExceptionDemo.java:5)
```
> 우선 아래의 오류를 살펴보자.

> Unhandled exception type FileNotFoundException

> 이것은 아래 로직에 대한 예외처리가 필요하다는 뜻이다.
```java
new FileReader("out.txt")
```
> FileReader라는 클래스를 API문서에서 찾아보자. FileReader의 [생성자를 문서](https://docs.oracle.com/javase/7/docs/api/java/io/FileReader.html#FileReader%28java.io.File%29)에서 찾아보면 

> Throws는 한국어로는 '던지다'로 번역된다. 위의 내용은 생성자 FileReader의 인자 fileName의 값에 해당하는 파일이 디렉토리이거나 어떤 이유로 사용할 수 없다면 FileNotFoundException을 발생시킨다는 의미다.

> 이것은 FileReader의 생성자가 동작할 때 파일을 열 수 없는 경우가 생길 수 있고, 이런 경우 생성자 FileReader에서는 이 문제를 처리할 수 없기 때문에 이에 대한 처리를 생성자의 사용자에게 위임하겠다는 의미다. 그것을 던진다(throw)고 표현하고 있다. 따라서 API의 사용자 쪽에서는 예외에 대한 처리를 반드시 해야 한다는 의미다. 따라서 아래와 같이 해야 FileReader 클래스를 사용할 수 있다.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
public class CheckedExceptionDemo {
    public static void main(String[] args) {
        try {
            BufferedReader bReader = new BufferedReader(new FileReader("out.txt"));
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        String input = bReader.readLine();
        System.out.println(input); 
    }
}
```
> BufferedReader 클래스의 readLine 메소드는 IOException을 발생시킬 수 있다. 아래와 같이 코드를 수정하자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
public class CheckedExceptionDemo {
    public static void main(String[] args) {
        try {
            BufferedReader bReader = new BufferedReader(new FileReader("out.txt"));
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        try{
            String input = bReader.readLine();
        } catch (IOException e){
            e.printStackTrace();
        }       
        System.out.println(input); 
    }
}
```
> 그런데 위의 코드는 컴파일되지 않는다. 여기에는 함정이 있는데 변수 bReader를 보자. 이 변수는 try의 중괄호 안에서 선언되어 있다. 그리고 이 변수는 11행에서 사용되는데 bReader가 선언된 6행과 사용될 11행은 서로 다른 중괄호이다. 따라서 11행에서는 6행에서 선언된 bReader에 접근할 수 없다. 코드를 수정하자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
public class CheckedExceptionDemo {
    public static void main(String[] args) {
        BufferedReader bReader = null;
        String input = null;
        try {
            bReader = new BufferedReader(new FileReader("out.txt"));
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        try{
            input = bReader.readLine();
        } catch (IOException e){
            e.printStackTrace();
        }       
        System.out.println(input); 
    }
}
```
throw와 throws
-
