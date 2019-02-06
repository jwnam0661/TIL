라이브러리
-
> 라이브러리는 모듈과 비슷한 개념이다. 모듈이 프로그램을 구성하는 작은 부품으로서의 로직을 의미한다면 라이브러리는 자주 사용되는 로직을 재사용하기 편리하도록 잘 정리한 일련의 코드들의 집합을 의미한다고 할 수 있다. 프로그래밍의 세계에는 휼룡한 라이브러리가 많다. 좋은 라이브러리를 선택하고 잘 사용하는 것은 프로그래밍의 핵심이라고 할 수 있다. 

> [jQuery 홈페이지](http://jquery.com/)에서 파일을 다운로드 받는다.

> [jQuery 메뉴얼](http://api.jquery.com/)을 이용해서 사용법을 파악한다.
```javascript
<!DOCTYPE html>
<html>
<head>
    <script src="https://code.jquery.com/jquery-1.11.0.min.js"></script>
</head>
<body>
    <ul id="list">
        <li>empty</li>
        <li>empty</li>
        <li>empty</li>
        <li>empty</li>
    </ul>
    <input id="execute_btn" type="button" value="execute" />
    <script type="text/javascript">
     $('#execute_btn').click(function(){
        $('#list li').text('coding everybody');
     })
    </script>
</body>
</html>
```
> 다음은 jQuery를 이용하지 않고 동일한 기능을 구현한 예제이다.
```javascript
<!DOCTYPE html>
<html>
<body>
    <ul id="list">
        <li>empty</li>
        <li>empty</li>
        <li>empty</li>
        <li>empty</li>
    </ul>
    <input id="execute_btn" type="button" value="execute" />
    <script type="text/javascript">
    function addEvent(target, eventType,eventHandler, useCapture) {
        if (target.addEventListener) { // W3C DOM
            target.addEventListener(eventType,eventHandler,useCapture?useCapture:false);
        } else if (target.attachEvent) {  // IE DOM
            var r = target.attachEvent("on"+eventType, eventHandler);
        }
    }
    function clickHandler(event) {
        var nav = document.getElementById('list');
        for(var i = 0; i < nav.childNodes.length; i++) {
            var child = nav.childNodes[i];
            if(child.nodeType==3)
                continue;
            child.innerText = 'Coding everybody';
        }
    }
    addEvent(document.getElementById('execute_btn'), 'click', clickHandler);
    </script>
</body>
</html>
```
