throw와 throws
-
> throw는 예외처리를 다음 사용자에게 넘기는 것이다. 다음 사용자는 누구일까? 코드를 보자.
```java
package org.opentutorials.javatutorials.exception;
 
class B{
    void run(){
    }
}
class C{
    void run(){
        B b = new B();
        b.run();
    }
}
public class ThrowExceptionDemo {
    public static void main(String[] args) {
         C c = new C();
         c.run();
    }   
}
```
> ThrowExceptionDemo.main(클래스 ThrowExceptionDem의 메소드 main)은 C.run의 사용자이다. C.run은 B.run의 사용자이다. 반대로 B.run의 다음 사용자는 C.run이고 C.run의 다음 사용자는 ThrowExceptionDem.main이 되는 셈이다. 파일을 읽은 로직을 추가해보자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
class B{
    void run(){
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
class C{
    void run(){
        B b = new B();
        b.run();
    }
}
public class ThrowExceptionDemo {
    public static void main(String[] args) {
         C c = new C();
         c.run();
    }   
}
```
> 위의 코드는 B.run이 FileReader의 생성자와 BufferedReader.readLine가 던진 예외를 try...catch로 처리한다. 즉 B.run이 예외에 대한 책임을 지고 있다.

> 그런데 B.run이 예외 처리를 직접 하지 않고 다음 사용자 C.run에게 넘길 수 있다. 아래의 코드를 보자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
class B{
    void run() throws IOException, FileNotFoundException{
        BufferedReader bReader = null;
        String input = null;
        bReader = new BufferedReader(new FileReader("out.txt"));
        input = bReader.readLine();
        System.out.println(input); 
    }
}
class C{
    void run(){
        B b = new B();
        b.run();
    }
}
public class ThrowExceptionDemo {
    public static void main(String[] args) {
         C c = new C();
         c.run();
    }   
}
```
> B 내부의 try...catch 구문은 제거되었고 run 옆에 throws IOException, FileNotFoundException이 추가되었다. 이것은 B.run 내부에서 IOException, FileNotFoundException에 해당하는 *예외가 발생하면 이에 대한 처리를 B.run의 사용자에게 위임*하는 것이다. 위의 코드에서 B.run의 사용자는 C.run이다. 따라서 C.run은 아래와 같이 수정돼야 한다.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
class B{
    void run() throws IOException, FileNotFoundException{
        BufferedReader bReader = null;
        String input = null;
        bReader = new BufferedReader(new FileReader("out.txt"));
        input = bReader.readLine();
        System.out.println(input);
    }
}
class C{
    void run(){
        B b = new B();
        try {
            b.run();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
public class ThrowExceptionDemo {
    public static void main(String[] args) {
         C c = new C();
         c.run();
    }   
}
```
> 이 책임을 다시 main에게 넘겨보자.
```java
package org.opentutorials.javatutorials.exception;
import java.io.*;
class B{
    void run() throws IOException, FileNotFoundException{
        BufferedReader bReader = null;
        String input = null;
        bReader = new BufferedReader(new FileReader("out.txt"));
        input = bReader.readLine();
        System.out.println(input);
    }
}
class C{
    void run() throws IOException, FileNotFoundException{
        B b = new B();
        b.run();
    }
}
public class ThrowExceptionDemo {
    public static void main(String[] args) {
         C c = new C();
         try {
            c.run();
        } catch (FileNotFoundException e) {
            System.out.println("out.txt 파일은 설정 파일 입니다. 이 파일이 프로잭트 루트 디렉토리에 존재해야 합니다.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }   
}
```
> out.txt 파일을 찾을 수 없는 상황은 B.run 입장에서는 어떻게 할 수 있는 일이 아니다. 엔드유저인 애플리케이션의 사용자가 out.txt 파일을 루트 디렉토리에 위치시켜야 하는 문제이기 때문에 애플리케이션의 진입점인 메소드 main으로 책임을 넘기고 있다.

> 예외 처리는 귀찮은 일이다. 그래서 예외를 다음 사용자에게 전가(throw)하거나 try...catch로 감싸고 아무것도 하지 않고 싶은 유혹에 빠지기 쉽다. 하지만 예외는 API를 사용하면서 발생할 수 있는 잠재적 위협에 대한 API 개발자의 강력한 암시다. 이 암시를 무시해서는 안 된다. 물론 더욱 고민스러운 것은 예외 처리 방법에 정답이 없다는 것이겠지만 말이다.
