참조
=
복제
-
> 전자화된 시스템의 가장 중요한 특징은 복제다. 현실의 사물과 다르게 전자화된 시스템 위의 데이터를 복제 하는데는 비용이 거의 들지 않는다. 바로 이러한 특징이 소프트웨어를 기존의 산업과 구분하는 가장 큰 특징일 것이다. 프로그래밍에서 복제가 무엇인가를 살펴보자.
```java
package org.opentutorials.javatutorials.reference;
 
public class ReferenceDemo1 {
 
    public static void runValue(){
        int a = 1;
        int b = a;
        b = 2;
        System.out.println("runValue, "+a); 
    }
 
    public static void main(String[] args) {
        runValue();
    }
 
}
```
결과
```java
runValue, 1
```
> 결과는 당연하다. 값을 변경한 것은 변수 b이기 때문에 변수 a에 담겨있는 값은 그대로이다. 변수 b의 값에 변수 a의 값이 복제된 것이다. 이를 그림으로 표시하면 아래와 같다.

![2131](https://user-images.githubusercontent.com/23206749/53066501-81369700-3513-11e9-840e-72b11e9963de.png)

참조
-
> 그런데 자연의 산물이 아니라 거대한 약속의 집합인 소프트웨어의 세계에서 당연한 것은 없다. 이것이 당연하지 않은 이유는 다음 예제를 통해서 좀 더 분명하게 드러난다.
```java
package org.opentutorials.javatutorials.reference;
class A{
    public int id;
    A(int id){
        this.id = id;
    }
}
public class ReferenceDemo1 {
 
    public static void runValue(){
        int a = 1;
        int b = a;
        b = 2;
        System.out.println("runValue, "+a); 
    }
     
    public static void runReference(){
        A a = new A(1);
        A b = a;
        b.id = 2;
        System.out.println("runReference, "+a.id);      
    }
 
    public static void main(String[] args) {
        runValue();
        runReference();
    }
 
}
```
결과
```java
runValue, 1
runReference, 2
```
> 이 코드의 주인공은 아래와 같다.
```java
b.id = 2;
System.out.println("runReference, "+a.id);  
```
> 놀라운 차이점이 있다. 변수 b에 담긴 인스턴스의 id 값을 2로 변경했을 뿐인데 a.id의 값도 2가 된 것이다. 이것은 변수 b와 변수 a에 담긴 인스턴스가 서로 같다는 것을 의미하다. 참조(reference)의 세계에 온 것을 환영한다.

> 복제만으로 전자화된 시스템을 설명하는 것은 조금 부족하다. 비유하자면 복제는 파일을 복사하는 것이고 참조는 심볼릭 링크(symbolic link) 혹은 바로가기(윈도우)를 만드는 것과 비슷하다. 원본 파일에 대해서 심볼릭 링크를 만들면 원본이 수정되면 심볼릭 링크에도 그 내용이 실시간으로 반영되는 것과 같은 효과다. 심볼릭 링크를 통해서 만든 파일은 원본 파일에 대한 주소 값이 담겨 있다. 누군가 심볼릭 링크에 접근하면 컴퓨터는 심볼릭 링크에 저장된 원본의 주소를 참조해서 원본의 위치를 알아내고 원본에 대한 작업을 하게 된다. 다시 말해서 원본을 복제한 것이 아니라 원본 파일을 참조(reference)하고 있는 것이다. 덕분에 저장 장치의 용량을 절약할 수 있고, 원본 파일을 사용하고 있는 모든 복제본이 동일한 내용을 유지할 수 있게 된다. 참조는 전자화된 세계의 극치라고 할 수 있다.

> 프로그래밍에서 광범위하게 사용하는 라이브러리라는 개념도 일종의 참조라고 할 수 있다. 공용 라이브러리를 사용하게 되면 하나의 라이브러리를 여러 애플리케이션에서 공유해서 사용하게 된다. 라이브러리의 내용이 변경되면 이를 참조하고 있는 애플리케이션에도 내용이 반영되게 된다. 또 우리가 변수를 사용하는 이유도 말하자면 참조를 위해서라고 할 수 있을 것이다. 본질을 파악하면 이해력도 높아지고 암기할 것도 줄어든다.

> 아래 두 개의 구문의 차이점을 생각해보자.
```java
int a = 1;
```
```java
A a = new A(1);
```
> 무엇일까? 전자는 데이터형이 int이고 후자는 A이다. int는 기본 데이터형(원시 데이터형, Primitive Data Types)이다. 자바에서는 기본 데이터형을 제외한 모든 데이터 타입은 참조 데이터형(참조 자료형)이라고 부른다. 기본 데이터형은 위와 같이 복제 되지만 참조 데이터형은 참조된다. new를 사용해서 객체를 만드는 모든 데이터 타입이 참조 데이터형이라고 생각해도 된다. (단 String은 제외다) 이를 그림으로 나타내면 아래와 같다.

![2133](https://user-images.githubusercontent.com/23206749/53066590-e9857880-3513-11e9-9ae6-3b18fe9e5900.png)

> 정리하면 변수에 담겨있는 데이터가 기본형이면 그 안에는 실제 데이터가 들어있고, 기본형이 아니면 변수 안에는 데이터에 대한 참조 방법이 들어있다고 할 수 있다.

참조 데이터 형과 매개 변수
-
> 그럼 일종의 변수할당이라고 할 수 있는 메소드의 매개변수는 어떻게 동작하는가를 살펴보자. 조금 복잡하므로 꼼꼼하게 살펴봐야 한다. 예제를 보자.
```java
package org.opentutorials.javatutorials.reference;
 
public class ReferenceParameterDemo {
     
    static void _value(int b){
        b = 2;
    }
     
    public static void runValue(){
        int a = 1;
        _value(a);
        System.out.println("runValue, "+a);
    }
     
    static void _reference1(A b){
        b = new A(2);
    }
     
    public static void runReference1(){
        A a = new A(1);
        _reference1(a);
        System.out.println("runReference1, "+a.id);     
    }
     
    static void _reference2(A b){
        b.id = 2;
    }
 
    public static void runReference2(){
        A a = new A(1);
        _reference2(a);
        System.out.println("runReference2, "+a.id);     
    }
     
    public static void main(String[] args) {
        runValue(); // runValue, 1
        runReference1(); // runReference1, 1
        runReference2(); // runReference2, 2
    }
 
}
```
> 결과는 아래와 같다.
```java
runValue, 1
runReference1, 1
runReference2, 2
```
> 위의 예제는 3가지 경우를 설명하고 있다. 하나씩 살펴보자. 

> 아래 코드는 _value의 매개변수로 기본 데이터형(int)를 전달했다. 
```java
runValue();
```
> 메소드 _value의 인자로 a를 전달했다. 인자 a는 매개변수 b가 되어서 _value 안으로 전달되고 있다. _value 안에서 b의 값을 변경했다. _value가 실행된 후에 runValue에서 a값을 출력해본 결과 값이 변경되지 않았다. 호출된 메소드의 작업이 호출한 메소드에 영향을 미치지 않고 있다. 어찌보면 자연스러운 결과다.

> 두번째 코드를 보자. 아래 코드는 _reference1의 매개변수로 참조 데이터 타입을 전달하고 있다. 
```java
runReference1();
```
> 메소드 _reference1 안에서 매개변수 b의 값을 다른 객체로 변경하고 있다. 이것은 지역변수인 b의 데이터를 교체한 것일 뿐이기 때문에 runReference1의 결과에는 영향을 미치지 않는다. 

> 그런데 다음의 코드는 호출된 메소드의 작업이 호출한 메소드의 변수에 영향을 미친다. 
```java
runReference2();
```
> 매개변수 b의 값을 다른 객체로 교체한 것이 아니라 매개변수 b의 인스턴스 변수 id 값을 2로 변경하고 있다. 이러한 맥락에서 _reference2의 변수 b는 runReference2의 변수 a와 참조 관계로 연결되어 있는 것이기 때문에 a와 b는 모두 같은 객체를 참조하고 있는 변수다.

> 매개변수를 다른 객체로 변경하는 것과 참조 데이터 타입의 매개변수에 담겨 있는 객체에 접근하는 것은 완전히 다른 의미를 가지기 때문에 두가지 경우의 차이점을 확실하게 이해하도록 하자.
