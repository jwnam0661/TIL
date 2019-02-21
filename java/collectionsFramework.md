컬렉션즈 프래임워크란?
-
> 그럼 이제부터 컬렉션즈 프래임워크가 무엇인가 본격적으로 알아보자. 컬렉션즈 프래임워크라는 것은 다른 말로는 컨테이너라고도 부른다. 즉 값을 담는 그릇이라는 의미이다. 그런데 그 값의 성격에 따라서 컨테이너의 성격이 조금씩 달라진다. 자바에서는 다양한 상황에서 사용할 수 있는 다양한 컨테이너를 제공하는데 이것을 컬렉션즈 프래임워크라고 부른다. ArrayList는 그중의 하나다.

![2154](https://user-images.githubusercontent.com/23206749/53144850-4307a900-35e1-11e9-840c-85b04d08cdf9.png)
> 위의 그림은 컬렉션즈 프래임워크의 구성을 보여준다. Collection과 Map이라는 최상위 카테고리가 있고, 그 아래에 다양한 컬렉션들이 존재한다. 그럼 구체적인 컬렉션즈 프래임워크 클래스들을 살펴보자.

![2160](https://user-images.githubusercontent.com/23206749/53144851-43a03f80-35e1-11e9-986c-636e36080c90.png)
> ArrayList를 찾아보자. Collection-List에 속해있다. ArrayList는 LIst라는 성격으로 분류되고 있는 것이다. List는 인터페이스이다. 그리고 List 하위의 클래스들은 모두 List 인터페이스를 구현하기 때문에 모두 같은 API를 가지고 있다. 클래스의 취지에 따라서 구현방법과 동작방법은 다르지만 공통의 조작방법을 가지고 있는 것이다. 익숙한 ArrayList를 바탕으로 나머지 컬렉션들의 성격을 파악해보자.

> List와 Set의 차이점은 List는 중복을 허용하고, Set은 허용하지 않는다.
```java
package org.opentutorials.javatutorials.collection;
 
import java.util.ArrayList;
import java.util.HashSet;
 
import java.util.Iterator;
 
public class ListSetDemo {
 
    public static void main(String[] args) {
        ArrayList<String> al = new ArrayList<String>();
        al.add("one");
        al.add("two");
        al.add("two");
        al.add("three");
        al.add("three");
        al.add("five");
        System.out.println("array");
        Iterator ai = al.iterator();
        while(ai.hasNext()){
            System.out.println(ai.next());
        }
         
        HashSet<String> hs = new HashSet<String>();
        hs.add("one");
        hs.add("two");
        hs.add("two");
        hs.add("three");
        hs.add("three");
        hs.add("five");
        Iterator hi = hs.iterator();
        System.out.println("\nhashset");
        while(hi.hasNext()){
            System.out.println(hi.next());
        }
    }
 
}
```
> 결과는 아래와 같다.
```java
array
one
two
two
three
three
five
 
hashset
two
five
one
three
```
> 우선 값을 가져오는 방법이 조금 달라졌다. (ArrayList에서도 이 방법을 사용할 수 있다)
```java
Iterator ai = al.iterator();
while(ai.hasNext()){
    System.out.println(ai.next());
}
```
> 메소드 iterator는 인터페이스 Collection에 정의되어 있다. 따라서 Collection을 구현하고 있는 모든 컬렉션즈 프레임웍크는 이 메소드를 구현하고 있음을 보증한다. 메소드 iterator의 호출 결과는 인터페이스 iterator를 구현한 객체를 리턴한다. 인터페이스 iterator는 아래 3개의 메소드를 구현하도록 강제하고 있는데 각각의 역할은 아래와 같다.

* hasNext : 반복할 데이터가 더 있으면 true, 더 이상 반복할 데이터가 없다면 false를 리턴한다.
* next    : hasNext가 true라는 것은 next가 리턴할 데이터가 존재한다는 의미다. 

> 이러한 기능을 조합하면 for 문을 이용하는 것과 동일하게 데이터를 순차적으로 처리할 수 있다.

> Set은 중복을 허용하지 않고 순서가 없지만, List는 중복을 허용하고 저장되는 순서가 유지된다는 것을 알 수 있다. 이러한 특징을 고려해서 컬렉션을 선택해야 한다.
