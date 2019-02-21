set
-
> Set은 한국어로 집합이라는 뜻이다. 여기서의 집합이란 수학의 집합과 같은 의미다. 수학에서의 집합도 순서가 없고 중복되지 않는 특성이 있다는 것이 기억날 것이다. (기억나지 않아도 상관없다) 수학에서 집합은 교집합(intersect), 차집합(difference), 합집합(union)과 같은 연산을 할 수 있었다. Set도 마찬가지다.

> 아래와 같이 3개의 집합 hs1, hs2, hs3이 있다고 하자. h1은 숫자 1,2,3으로 이루어진 집합이고, h2는 3,4,5 h3는 1,2로 구성되어 있다. set의 API를 이용해서 집합 연산을 해보자.
```java
package org.opentutorials.javatutorials.collection;
 
import java.util.ArrayList;
import java.util.HashSet;
 
import java.util.Iterator;
 
public class SetDemo {
 
    public static void main(String[] args) {
        HashSet<Integer> A = new HashSet<Integer>();
        A.add(1);
        A.add(2);
        A.add(3);
         
        HashSet<Integer> B = new HashSet<Integer>();
        B.add(3);
        B.add(4);
        B.add(5);
         
        HashSet<Integer> C = new HashSet<Integer>();
        C.add(1);
        C.add(2);
         
        System.out.println(A.containsAll(B)); // false
        System.out.println(A.containsAll(C)); // true
         
        //A.addAll(B);
        //A.retainAll(B);
        //A.removeAll(B);
         
        Iterator hi = A.iterator();
        while(hi.hasNext()){
            System.out.println(hi.next());
        }
    }
 
}
```
부분집합(subset)
-
```java
System.out.println(A.containsAll(B)); // false
System.out.println(A.containsAll(C)); // true
```
![2155](https://user-images.githubusercontent.com/23206749/53145261-dee5e480-35e2-11e9-89ae-8c10e976c5f5.png)
합집합(union)
-
```java
A.addAll(B);
```
![2156](https://user-images.githubusercontent.com/23206749/53145263-df7e7b00-35e2-11e9-9b5a-cfd517f21025.png)
교집합(intersect)
-
```java
A.retainAll(B);
```
![2157](https://user-images.githubusercontent.com/23206749/53145264-df7e7b00-35e2-11e9-8018-dbe365646f4a.png)
차집합(difference)
-
```java
A.removeAll(B);
```
![2158](https://user-images.githubusercontent.com/23206749/53145265-df7e7b00-35e2-11e9-9394-018c4bed841f.png)
