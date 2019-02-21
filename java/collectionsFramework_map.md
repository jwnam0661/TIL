map
-
> 이번에는 Map 컬렉션에 대해서 알아보자. Map 컬렉션은 key와 value의 쌍으로 값을 저장하는 컬렉션이다. 아래 코드를 보자.
```java
package org.opentutorials.javatutorials.collection;
 
import java.util.*;
 
public class MapDemo {
 
    public static void main(String[] args) {
        HashMap<String, Integer> a = new HashMap<String, Integer>();
        a.put("one", 1);
        a.put("two", 2);
        a.put("three", 3);
        a.put("four", 4);
        System.out.println(a.get("one"));
        System.out.println(a.get("two"));
        System.out.println(a.get("three"));
         
        iteratorUsingForEach(a);
        iteratorUsingIterator(a);
    }
     
    static void iteratorUsingForEach(HashMap map){
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        for (Map.Entry<String, Integer> entry : entries) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
    }
     
    static void iteratorUsingIterator(HashMap map){
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        Iterator<Map.Entry<String, Integer>> i = entries.iterator();
        while(i.hasNext()){
            Map.Entry<String, Integer> entry = i.next();
            System.out.println(entry.getKey()+" : "+entry.getValue());
        }
    }
 
}
```
> Map에서 데이터를 추가할 때 사용하는 API는 put이다. put의 첫번째 인자는 값의 key이고, 두번째 인자는 key에대한 값이다. key를 이용해서 값을 가져올 수 있다.
```Java
System.out.println(a.get("one"));
```
> 이것이 Map의 가장 기본적인 사용법이다. 그럼 Map에 저장된 데이터를 열거할 때는 어떻게 해야할까?
```java
Set<Map.Entry<String, Integer>> entries = map.entrySet();
for (Map.Entry<String, Integer> entry : entries) {
    System.out.println(entry.getKey() + " : " + entry.getValue());
}
```
> 메소드 entrySet은 Map의 데이터를 담고 있는 Set을 반환한다. 반환한 Set의 값이 사용할 데이터 타입은 Map.Entry이다. Map.Entry는 인터페이스인데 아래와 같은 API를 가지고 있다.

* getKey
* getValue

> 위의 API를 이용해서 Map의 key, value를 조회할 수 있다.

> 앞서 Set이 수학의 집합을 프로그래밍적으로 구현한 것이라고 언급했다. map은 수학의 함수를 프로그래밍화한 것이다. 수학의 함수가 "정의역과 공역 원소들 사이의 단가 대응의 관계"라는 의미를 이해하고 있는 사람이라면 Map의 key와 value의 관계가 함수의 정의역과 공역의 관계와 같다는 것을 이해할 수 있을 것이다.  함수에 대한 이해가 없다면 이 내용은 몰라도 된다. 하지만 프로그래밍을 하게 되면 수학적인 지식들을 매우 구체적으로 경험할 수 있기 때문에 프로그래밍은 수학에 대한 좋은 실습 도구라고 할 수 있다. 수학이 너무 추상적이라서 배움에 어려움이 있는 독자라면 프로그래밍에 익숙해진 후에 수학공부를 시작해보자. 프로그래밍의 많은 장치들이 수학적인 장치들을 빌려온 것임을 알 수 있을 것이고, 수학이 보다 구체적으로 다가올 것이다.
