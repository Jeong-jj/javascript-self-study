**[ES6초급강의](https://velog.io/@rgfdds98/ECMAScript6%EA%B8%B0%EC%B4%88) 수강후 이어지는 강의 정리입니다.**

이전에 공부했던 [JS기초공부](https://github.com/Jeong-jj/javascript-self-study/tree/main/archive/Javascript%EA%B8%B0%EC%B4%88)에 이어서 하는 공부이기에 부족했던 부분에 대해서만 작성하고, 강의에서 이전 공부와 중복되는 부분은 생략하고 작성하였습니다.

<br/>

# 📌 setTimeout

`setTimeout`은 일정 시간이 지난 후 함수를 실행하도록 해주는 기능이다. 즉, setTimeout은 `스케쥴링` 기능이 있다.

```javascript
function fn() {
  console.log(3);
};

setTimeout(fn, 3000);
```

setTimeout은 두 개의 매개변수를 받는다. 그 의미는 다음과 같다.

- 첫번째 매개변수 : 일정 시간 후 실행할 함수
- 두번째 매개변수 : 시간 **_(밀리초 단위, 1000=1s)_**

또한 일정 시간 후 실행할 코드의 내용은 미리 선언하고 불러올 필요 없이 setTimeout내에서 직접 작성해도 된다.

```javascript
setTimeout(function fn() {
  console.log(3);
}, 3000);
```

- 만약 인수가 필요할 경우에는 시간 뒤에 적어준다.

```javascript
function showName(name) {
  console.log(name);
};

setTimeout(showName, 3000, "Mike");
```

> ### 만약 delay=0 이라면?
>
>```javascript
>setTimeout(function() {
>  console.log(2);
>}, 0);		// delay타임 0
>
>console.log(1);
>```
>
>위처럼 코드를 작성하면 콘솔창에 어떤 것이 먼저 찍힐까? delay시간이 0이니까 2가 먼저 찍힐까?
>
>사실 그렇지 않다. delay가 0이라 하더라도 바로 실행되는 것은 아니다.
>
>그 이유는 **_스케쥴링 함수는 실행 중인 스크립트가 종료된 후 실행되기 때문이다._**
>> _또한 브라우저는 기본적으로 약 4ms의 대기 시간이 존재한다._

<br/>
<br/>

## clearTimeout

`clearTimeout`은 예정된 작업을 없애주어 스케쥴링을 취소시킬 수 있다.

```javascript
function showName(name) {
  console.log(name);
};

const tId = setTimeout(showName, 3000, "Mike");
clearTimeout(tId);
```

위처럼 작성하면 3초가 지나기 전에 clearTimeout이 실행되어 아무 일도 일어나지 않는다.

<br/>
<br/>

# 📌 setInterval

`setInterval`은 일정 시간 간격으로 함수를 반복시켜 준다. 사용방법은 setTimeout과 동일하다.

```javascript
function showName(name) {
  console.log(name);
};

const tId = setInterval(showName, 3000, "Mike");
```

이렇게 작성하면 3초마다 "Mike"가 작성된다.

<br/>

## clear Interval

만약 중간에 중단하고 싶다면, 이것도 setTimeout과 비슷하게 `clearInterval`을 사용하면 된다.

	clearInterval(tId);

<br/>

### 📢 실용 예제
#### ▷ 아래 무한히 반복되는 setInterval이 5번만 실행 된 뒤 종료하도록 하는 코드를 작성해보자.

```javascript
let num=0;

function showTime() {
  console.log(`접속하신지 ${num++}초가 지났습니다.`);
};

setInterval(showTime, 1000);
```

> **정답**
>```javascript
>let num=0;
>
>function showTime() {
>  console.log(`접속하신지 ${num++}초가 지났습니다.`);
>  if (num>5) {
>    clearInterval(tId);
>  }
>};
>
>const tId = setInterval(showTime, 1000);
>```

<br/>
<br/>

# Reference

- **[코딩앙마님의 인프런 강의](https://www.inflearn.com/course/%EC%99%95%EC%B4%88%EB%B3%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)**