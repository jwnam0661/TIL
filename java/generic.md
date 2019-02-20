제네릭이란?
-
> 제네릭(Generic)은 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법을 의미한다. 말이 어렵다. 아래 그림을 보자.

![2136](https://user-images.githubusercontent.com/23206749/53067306-fa83b900-3516-11e9-9cb5-ac638a39f86b.png)

> 위의 그림은 아래의 코드를 간략화한 것이다.
```java
package org.opentutorials.javatutorials.generic;
 
class Person<T>{
    public T info;
}
 
public class GenericDemo {
 
    public static void main(String[] args) {
        Person<String> p1 = new Person<String>();
        Person<StringBuilder> p2 = new Person<StringBuilder>();
    }
 
}
```
> 그림을 보자. p1.info와 p2.info의 데이터 타입은 결과적으로 아래와 같다.

* p1.info : String
* p2.info : StringBuilder

> 그것은 각각의 인스턴스를 생성할 때 사용한 <> 사이에 어떤 데이터 타입을 사용했느냐에 달려있다. 

> 클래스 선언부를 보자.
```java
public T info;
```
> 클래스 Person의 필드 info의 데이터 타입은 T로 되어 있다. 그런데 T라는 데이터 타입은 존재하지 않는다. 이 값은 아래 코드의 T에서 정해진다.
```java
class Person<T>{
```
> 위 코드의 T는 아래 코드의 <> 안에 지정된 데이터 타입에 의해서 결정된다. 
```java
Person<String> p1 = new Person<String>();
```
> 위의 코드를 나눠보자. 아래 코드는 변수 p1의 데이터 타입을 정의하고 있다.
```java
Person<String> p1
```
> 아래 코드는 인스턴스를 생성하고 있다. 
```java
new Person<String>();
```
> 즉 클래스를 정의 할 때는 info의 데이터 타입을 확정하지 않고 인스턴스를 생성할 때 데이터 타입을 지정하는 기능이 제네릭이다. 

> 제네릭이 무엇인가를 알았으니까 이제 제네릭을 사용하는 이유를 알아보자. 

제네릭을 사용하는 이유
=
타입 안정성
-
```java
package org.opentutorials.javatutorials.generic;
class StudentInfo{
    public int grade;
    StudentInfo(int grade){ this.grade = grade; }
}
class StudentPerson{
    public StudentInfo info;
    StudentPerson(StudentInfo info){ this.info = info; }
}
class EmployeeInfo{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
}
class EmployeePerson{
    public EmployeeInfo info;
    EmployeePerson(EmployeeInfo info){ this.info = info; }
}
public class GenericDemo {
    public static void main(String[] args) {
        StudentInfo si = new StudentInfo(2);
        StudentPerson sp = new StudentPerson(si);
        System.out.println(sp.info.grade); // 2
        EmployeeInfo ei = new EmployeeInfo(1);
        EmployeePerson ep = new EmployeePerson(ei);
        System.out.println(ep.info.rank); // 1
    }
}
```
> 그리고 아래 코드를 보자. 위의 코드는 StudentPerson과 EmployeePerson가 사실상 같은 구조를 가지고 있다. 중복이 발생하고 있는 것이다. 중복을 제거해보자.
```java
package org.opentutorials.javatutorials.generic;
class StudentInfo{
    public int grade;
    StudentInfo(int grade){ this.grade = grade; }
}
class EmployeeInfo{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
}
class Person{
    public Object info;
    Person(Object info){ this.info = info; }
}
public class GenericDemo {
    public static void main(String[] args) {
        Person p1 = new Person("부장");
        EmployeeInfo ei = (EmployeeInfo)p1.info;
        System.out.println(ei.rank);
    }
}
```
> 위의 코드는 성공적으로 컴파일된다. 하지만 실행을 하면 아래와 같은 오류가 발생한다.
```java
Exception in thread "main" java.lang.ClassCastException: java.lang.String cannot be cast to org.opentutorials.javatutorials.generic.EmployeeInfo
    at org.opentutorials.javatutorials.generic.GenericDemo.main(GenericDemo.java:17)
```
> 아래 코드를 보자.
```java
Person p1 = new Person("부장");
```
> 클래스 Person의 생성자는 매개변수 info의 데이터 타입이 Object이다. 따라서 모든 객체가 될 수 있다. 그렇기 때문에 위와 EmployeeInfo의 객체가 아니라 String이 와도 컴파일 에러가 발생하지 않는다. 대신 런타임 에러가 발생한다. 컴파일 언어의 기본은 모든 에러는 컴파일이 발생할 수 있도록 유도해야 한다는 것이다. 런타임은 실제로 애플리케이션이 동작하고 있는 상황이기 때문에 런타임에 발생하는 에러는 항상 심각한 문제를 초래할 수 있기 때문이다. 

> 위와 같은 에러를 타입에 대해서 안전하지 않다고 한다. 즉 모든 타입이 올 수 있기 때문에 타입을 엄격하게 제한 할 수 없게 되는 것이다. 

제네릭화
-
> 이것을 제네릭으로 바꿔보자.
```java
package org.opentutorials.javatutorials.generic;
class StudentInfo{
    public int grade;
    StudentInfo(int grade){ this.grade = grade; }
}
class EmployeeInfo{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
}
class Person<T>{
    public T info;
    Person(T info){ this.info = info; }
}
public class GenericDemo {
    public static void main(String[] args) {
        Person<EmployeeInfo> p1 = new Person<EmployeeInfo>(new EmployeeInfo(1));
        EmployeeInfo ei1 = p1.info;
        System.out.println(ei1.rank); // 성공
         
        Person<String> p2 = new Person<String>("부장");
        String ei2 = p2.info;
        System.out.println(ei2.rank); // 컴파일 실패
    }
}
```
> p1은 잘 동작할 것이다. 중요한 것은 p2다. p2는 컴파일 오류가 발생하는데 p2.info가 String이고 String은 rank 필드가 없는데 이것을 호출하고 있기 때문이다. 여기서 중요한 것은 아래와 같이 정리할 수 있다.

* 컴파일 단계에서 오류가 검출된다.
* 중복의 제거와 타입 안전성을 동시에 추구할 수 있게 되었다.

제네릭의 특성
=
복수의 제네릭
-
> 클래스 내에서 여러개의 제네릭을 필요로 하는 경우가 있을 것이다. 예제를 보자.
```java
package org.opentutorials.javatutorials.generic;
class EmployeeInfo{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
}
class Person<T, S>{
    public T info;
    public S id;
    Person(T info, S id){ 
        this.info = info; 
        this.id = id;
    }
}
public class GenericDemo {
    public static void main(String[] args) {
        Person<EmployeeInfo, int> p1 = new Person<EmployeeInfo, int>(new EmployeeInfo(1), 1);
    }
}
```
> 위의 코드는 예외를 발생시키지만 문제는 다음 예제에서 처리하고 형식만 보자. 

> 즉, 복수의 제네릭을 사용할 때는 <T, S>와 같은 형식을 사용한다. 여기서 T와 S 대신 어떠한 문자를 사용해도 된다. 하지만 묵시적인 약속(convention)이 있기는 하다. 그럼 예제의 오류를 해결하자.

기본 데이터 타입과 제네릭
-
> 제네릭은 참조 데이터 타입에 대해서만 사용할 수 있다. 기본 데이터 타입에서는 사용할 수 없다. 따라서 아래와 같이 코드를 변경한다.
```java
package org.opentutorials.javatutorials.generic;
class EmployeeInfo{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
}
class Person<T, S>{
    public T info;
    public S id;
    Person(T info, S id){ 
        this.info = info;
        this.id = id;
    }
}
public class GenericDemo {
    public static void main(String[] args) {
        EmployeeInfo e = new EmployeeInfo(1);
        Integer i = new Integer(10);
        Person<EmployeeInfo, Integer> p1 = new Person<EmployeeInfo, Integer>(e, i);
        System.out.println(p1.id.intValue());
    }
}
```
> new Integer는 기본 데이터 타입인 int를 참조 데이터 타입으로 변환해주는 역할을 한다. 이러한 클래스를 래퍼(wrapper) 클래스라고 한다. 덕분에 기본 데이터 타입을 사용할 수 없는 제네릭에서 int를 사용할 수 있다.

제네릭의 생략
-
> 제네릭은 생략 가능하다. 아래 두 개의 코드가 있다. 이 코드들은 정확히 동일하게 동작한다. e와 i의 데이터 타입을 알고 있기 때문이다.
```java
EmployeeInfo e = new EmployeeInfo(1);
Integer i = new Integer(10);
Person<EmployeeInfo, Integer> p1 = new Person<EmployeeInfo, Integer>(e, i);
Person p2 = new Person(e, i);
```
메소드에 적용
-
> 제네릭은 메소드에 적용할 수도 있다. 
```java
package org.opentutorials.javatutorials.generic;
class EmployeeInfo{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
}
class Person<T, S>{
    public T info;
    public S id;
    Person(T info, S id){ 
        this.info = info;
        this.id = id;
    }
    public <U> void printInfo(U info){
        System.out.println(info);
    }
}
public class GenericDemo {
    public static void main(String[] args) {
        EmployeeInfo e = new EmployeeInfo(1);
        Integer i = new Integer(10);
        Person<EmployeeInfo, Integer> p1 = new Person<EmployeeInfo, Integer>(e, i);
        p1.<EmployeeInfo>printInfo(e);
        p1.printInfo(e);
    }
}
```
제네릭의 제한
-
> *extends*

> 제네릭으로 올 수 있는 데이터 타입을 특정 클래스의 자식으로 제한할 수 있다.
```java
package org.opentutorials.javatutorials.generic;
abstract class Info{
    public abstract int getLevel();
}
class EmployeeInfo extends Info{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
    public int getLevel(){
        return this.rank;
    }
}
class Person<T extends Info>{
    public T info;
    Person(T info){ this.info = info; }
}
public class GenericDemo {
    public static void main(String[] args) {
        Person p1 = new Person(new EmployeeInfo(1));
        Person<String> p2 = new Person<String>("부장");
    }
}
```
> 위의 코드에서 중요한 부분은 다음과 같다.
```java
class Person<T extends Info>{
```
> 즉 Person의 T는 Info 클래스나 그 자식 외에는 올 수 없다.

> extends는 상속(extends)뿐 아니라 구현(implements)의 관계에서도 사용할 수 있다.
```java
package org.opentutorials.javatutorials.generic;
interface Info{
    int getLevel();
}
class EmployeeInfo implements Info{
    public int rank;
    EmployeeInfo(int rank){ this.rank = rank; }
    public int getLevel(){
        return this.rank;
    }
}
class Person<T extends Info>{
    public T info;
    Person(T info){ this.info = info; }
}
public class GenericDemo {
    public static void main(String[] args) {
        Person p1 = new Person(new EmployeeInfo(1));
        Person<String> p2 = new Person<String>("부장");
    }
}
```
