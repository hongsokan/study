# 자바스크립트

## 모듈 패턴
- 자바스크립트에서 클래스를 모방하여 서로 관련 있는 변수와 함수를 모아 즉시 실행 함수로 감싼 모듈.

```
// 예제
var Counter = (function() {
    var num = 0; // private

    return { // public
        increase() {
            return ++num;
        },
        decrease() {
            return --num;
        }
    };
}());

console.log(Counter.num); // 외부에서 접근 불가.
console.log(Counter.increase()); // 외부에서 접근 가능.
console.log(Counter.decrease()); // 외부에서 접근 가능.
```
> 위 예제에서 Counter는 객체를 반환하는데, 반환 객체의 프로퍼티는 외부에 노출되는 퍼블릭 멤버이다. (public)
>
> 외부로 노출하고 싶지 않은 변수나 함수는 반환하는 객체에 추가하지 않으면 프라이빗 멤버가 된다. (private)

## [커링](https://ko.javascript.info/currying-partials)
- f(a,b,c)처럼 단일 호출로 처리되는 함수를 f(a)(b)(c)처럼 인수를 개별로 호출하여 병합되도록 변환하는 프로그래밍 기법

```js
// 예제
function curry(f) {
    return function(a) { // 1
        return function(b) { // 2
            return f(a, b);
        }
    }
}

function sum(a, b) {
    return a + b;
}

let curriedSum = curry(sum);
alert( curriedSum(1)(2) );
```
1. curriedSum(1)에서 인수 1은 렉시컬 환경에 저장되고, function(b) 반환
2. function(b)가 2를 인수로 호출, 그리고 f(a, b)가 sum으로 넘겨져 호출됨.


## [디바운싱과 쓰로틀링](https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa)

## 쓰로틀링
- 마지막 함수가 호출된 후 일정 시간이 지나기 전에 다시 호출되지 않도록 하는 것
## 디바운싱
- 연이어 호출되는 함수들 중 마지막 함수(또는 제일 처음)만 호출하도록 하는 것