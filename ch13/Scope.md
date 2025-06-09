# 13.1 Scope란?
모든 식별자(함수명, 변수명, 클래스 명 등)가 선언된 위치에 따른 자신을 참조할 수 있는 코드의 **유효 범위**를 뜻함

# 13.2 스코프의 종류

|구분|설명|스코프|변수|
|---|-----|-----|----|
|전역|코드의 가장 바깥 영역|전역 스코프|전역변수
|지역|함수 몸체 내부|지역 스코프|지역변수


```javascript
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x) // (1)
}

foo();

console.log(x) // (2)

// 1번 console.lgo 결과: local 
// 2번 console.log 결과 : global
```
- (1)의 경우 함수 foo() 안에서 선언된 변수이므로 함수 몸체 안에서만 유효함 -> 함수 foo() 안에서 선언된 변수를 **지역변수** 라고 하며, 지역변수의 스코프는 함수 몸체만 해당되므로 이를 **지역 스코프** 라고 함
- (2)의 경우 전역에서 선언된 변수이므로 어디에서든 사용할 수 있음 -> 함수 foo()안에서도 사용 가능하며 소스코드 내 어디에서든 참조가 가능한 변수를 **전역변수** 라고 하며 전역 변수의 스코프를 **전역 스코프** 라고 함
- **중요햔것은 같은 x라는 변수명을 갖고 있지만 스코프에 따라 변수에 할당된 값이 다름**

## 13.2.1 전역과 전역 스코프
```javascript
var x = 'global x';
var y = 'global y';

function outer() {
  var z = 'outer() local z';

  console.log(x); // (1) global x
  console.lgo(y); // (2) global y
  console.log(z); // (3) outer() local z

  function inner() {
    var x = 'inner() local x';

    conosle.log(x); // (4) inner() local x
    console.log(y); // (5) global y;
    console.log(z); // (6) outer() local z
  }

  inner();
}

outer();

console.log(x); // (7) global x
console.log(z); // (8) ReferenceError: z is not defined
```
- 전역이란 코드의 가장 바깥 영역을 의미하며 전역은 **전역 스코프(Scope)** 를 만듬
- **전역 변수는 어디에서든 참조가 가능함**

## 13.2.2 지역과 지역 스코프
- 지역이란 **함수 몸체 내부**를 뜻하며 지역은 **지역 스코프(Scope)** 를 만듬
  > *반드시 함수만 해당되는건 아님, 조건문, 반복문 블럭 내부에 선언된 변수도 지역 변수로 취급됨*
- 지역변수는 자신이 선언된 지역과 하위 지역(중첩 함수 등)에서만 참조할 수 있음

함수 outer()에 선언된 z의 경우 지역 변수로, outer() 내부 및 중첩 함수인 inner()에서 z에 선언된 값을 참조할 수 있음
또한, 함수 inner()에 선언된 x도 함수 inner()의 지역변수임

# 13.3 스코프 체인
- 함수는 전역에서 정의할 수도 있고 함수 내부에서 정의할 수 있음
- 함수 내부에서 정의된 또다른 함수를 **중첩 함수** 라고 하며, 중첩 함수를 포함하는 함수를 **외부 함수**라고 함
```javascript
function outer() {
  var z = 'outer() local z';

  console.log(x); // (1) global x
  console.lgo(y); // (2) global y
  console.log(z); // (3) outer() local z

  function inner() {
    var x = 'inner() local x';

    conosle.log(x); // (4) inner() local x
    console.log(y); // (5) global y;
    console.log(z); // (6) outer() local z
  }
// 함수 inner()는 함수 outer() 안에 정의 되어 있으므로 중첩 함수임
// 중첩 함수 inner()를 포함하고 있는 함수 outeer()는 외부 함수임
```
- 함수의 지역 스코프도 중첩 함수가 있을 수 있으므로 중첩이 가능함
- 이렇게 스코프가 계층적으로 연결된것을 **스코프 체인(Scope Chain)** 이라고 함

## 13.3.1 스코프 체인에 의한 변수 검색
- 스코프 체인으로 연결된 스코프의 계층척 구조는 부모/자식 관계의 상속 구조와 유사함
- 상위 스코프(부모)에서 유효한 변수는 하위 스코프(자식)에서 참조가 가능하지만 하위 스코프의 변수는 상위 스코프에서 참조가 불가능함

# 13.4 함수 레벨 스코프
`var` 키워드로 선언한 변수는 함수 코드 블럭(함수 내부)가 아닌 이상 모두 전역 변수로 취급됨
```javascript
var x = 10;
if(true){
  var x = 1;
}
console.log(x); // 1
// var 키워드를 통한 변수 선언은 전역 변수이지만 함수 내부에서 선언하게 되면 지역변수로 취급됨
// 하지만 이는 함수 내부에서만 var를 사용했을때의 경우이고 함수가 아닌 다른 코드블럭에서 선언하면 모두 전역변수임
```
# 13.5 렉시컬 스코프
함수를 **어디서 정의**했는지에 따라 함수의 상위 스코프를 결정
