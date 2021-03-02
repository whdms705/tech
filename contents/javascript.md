# JAVASCRIPT

**:Contents**
* [이벤트전파방식](#이벤트전파방식)
* [클로저](#클로저)
* [promise](#promise)
* [async await](#async await)
* [var let const](#var_let_const)



### 이벤트전파방식

`이벤트 버블링`<br>
>>  하위에서 상위 요소로의 이벤트 전파 방식을 이벤트 버블링(Event Bubbling)

`이벤트 캡쳐`<br>
>> 이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식

`stopPropagation`<br>
 ```javascript
    div.addEventListener('click', event, {
		capture: true // default 값은 false입니다.
	});
```


그렇다면 전체적으로 이벤트를 전달하는게 아니고 해당요소에 이벤트만 주고 싶다면?<br>
`stopPropagation`<br>
 ```javascript
function logEvent(event) {
	event.stopPropagation();
}
```


`이벤트 위임`<br>
>> 하위 요소에 각각 이벤트를 붙이지 않고 상위 요소에서 하위 요소의 이벤트들을 제어하는 방식

- cf) https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/