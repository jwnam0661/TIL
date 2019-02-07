> JavaScript에서는 함수도 객체다. 다시 말해서 일종의 값이다. 거의 모든 언어가 함수를 가지고 있다. JavaScript의 함수가 다른 언어의 함수와 다른 점은 함수가 값이 될 수 있다는 점이다. 다음 예제를 통해서 그 의미를 알아보자.
```javascript
function a(){}
```
> 위의 예제에서 함수 a는 변수 a에 담겨진 값이다. 또한 함수는 객체의 값으로 포함될 수 있다. 이렇게 객체의 속성 값으로 담겨진 함수를 메소드(method)라고 부른다.
```javascript
a = {
    b:function(){
    }
};
```
> 함수는 값이기 때문에 다른 함수의 인자로 전달 될수도 있다. 아래 예제를 보자.
```javascript
function cal(func, num){
    return func(num)
}
function increase(num){
    return num+1
}
function decrease(num){
    return num-1
}
alert(cal(increase, 1));
alert(cal(decrease, 1));
```
> 10행을 실행하면 함수 increase와 값 1이 함수 cal의 인자로 전달된다. 함수 cal은 첫번째 인자로 전달된 increase를 실행하는데 이 때 두번째 인자의 값이 1을 인자로 전달한다. 함수 increase은 계산된 결과를 리턴하고 cal은 다시 그 값을 리턴한다.
---------------------------
> 함수는 함수의 **리턴 값**으로도 사용할 수 있다.
```javascript
function cal(mode){
    var funcs = {
        'plus' : function(left, right){return left + right},
        'minus' : function(left, right){return left - right}
    }
    return funcs[mode];
}
alert(cal('plus')(2,1)); // 3
alert(cal('minus')(2,1)); // 1
```
> 당연히 **배열의 값**으로도 사용할 수 있다.
```javascript
var process = [
    function(input){ return input + 10;},
    function(input){ return input * input;},
    function(input){ return input / 2;}
];
var input = 1;
for(var i = 0; i < process.length; i++){
    input = process[i](input);
}
alert(input);
```
