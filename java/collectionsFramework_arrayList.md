배열과 컬렉션즈 프레임워크
-
> 배열은 연관된 데이터를 관리하기 위한 수단이었다. 그런데 배열에는 몇가지 불편한 점이 있었는데 그 중의 하나가 한번 정해진 배열의 크기를 변경할 수 없다는 점이다. 이러한 불편함을 컬렉션즈 프래임워크를 사용하면 줄어든다.
```java
package org.opentutorials.javatutorials.collection;
 
import java.util.ArrayList;
 
public class ArrayListDemo {
 
    public static void main(String[] args) {
        String[] arrayObj = new String[2];
        arrayObj[0] = "one";
        arrayObj[1] = "two";
        // arrayObj[2] = "three"; 오류가 발생한다.
        for(int i=0; i<arrayObj.length; i++){
            System.out.println(arrayObj[i]);
        }
         
        ArrayList al = new ArrayList();
        al.add("one");
        al.add("two");
        al.add("three");
        for(int i=0; i<al.size(); i++){
            System.out.println(al.get(i));
        }
    }
 
}
```
> 아래 코드처럼 배열은 그 크기를 한번 지정하면 크기보다 많은 수의 값을 저장할 수 없다.
```java
String[] arrayObj = new String[2];
arrayObj[0] = "one";
arrayObj[1] = "two";
// arrayObj[2] = "three"; 오류가 발생한다.
```
> 하지만 ArrayList는 크기를 미리 지정하지 않기 때문에 얼마든지 많은 수의 값을 저장할 수 있다.
```java
ArrayList al = new ArrayList();
al.add("one");
al.add("two");
al.add("three");
```
> ArrayList는 배열과는 사용방법이 조금 다르다. 배열의 경우 값의 개수를 구할 때 .length를 사용했지만 ArrayList는 메소드 size를 사용한다. 또한, 특정한 값을 가져올 때 배열은 [인덱스 번호]를 사용했지만 컬렉션은 .get(인덱스 번호)를 사용한다.
```java
for(int i=0; i<al.size(); i++){
    System.out.println(al.get(i));
}
```
> 그런데 ArrayList를 사용할 때 주의할 점이 있다. 위의 반복문을 아래처럼 바꿔보자.
```java
for(int i=0; i<al.size(); i++){
    String val = al.get(i);
    System.out.println(val);
}
```
> 위의 코드는 컴파일 오류가 발생한다. ArrayList의 메소드 add의 입장에서는 인자로 어떤 형태의 값이 올지 알 수 없다. 그렇기 때문에 모든 데이터 타입의 조상인 Object 형식으로 데이터를 받고 있다. 예를들면 아래와 같은 모습일 것이다. (실제와는 다르다)
```java
public boolean add(Object e) {
```
> 따라서 ArrayList 내에서 add를 통해서 입력된 값은 Object의 데이터 타입을 가지고 있고, get을 이용해서 이를 꺼내도 Object의 데이터 타입을 가지고 있게 된다. 그래서 위의 코드는 아래와 같이 바꿔야 한다.
```java
for(int i=0; i<al.size(); i++){
    String val = (String)al.get(i);
    System.out.println(val);
}
```
> get의 리턴값을 문자열로 형변환하고 있다. 원래의 데이터 타입이 된 것이다.

> 그런데 위의 방식은 예전의 방식이다. 이제는 아래와 같이 제네릭을 사용해야 한다.
```java
ArrayList<String> al = new ArrayList<String>();
al.add("one");
al.add("two");
al.add("three");
for(int i=0; i<al.size(); i++){
    String val = al.get(i);
    System.out.println(val);
}
```
> 제네릭을 사용하면 ArrayList 내에서 사용할 데이터 타입을 인스턴스를 생성할 때 지정할 수 있기 때문에 데이터를 꺼낼 때(String val = al.get(i);) 형변환을 하지 않아도 된다.
