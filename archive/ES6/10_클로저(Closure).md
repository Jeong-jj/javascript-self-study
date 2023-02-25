**[ES6초급강의](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/ES6/01_ES6_%EA%B8%B0%EC%B4%88.md) 수강후 이어지는 강의 정리입니다.**

이전에 공부했던 [JS기초공부](https://github.com/Jeong-jj/javascript-self-study/tree/main/archive/Javascript%EA%B8%B0%EC%B4%88)에 이어서 하는 공부이기에 부족했던 부분에 대해서만 작성하고, 강의에서 이전 공부와 중복되는 부분은 생략하고 작성하였습니다.

>이번 정리를 이해하기 위해서는 [ES6 기초](https://velog.io/@rgfdds98/ECMAScript6%EA%B8%B0%EC%B4%88)에 대한 선행이 필요하다. 이번 글을 읽기 전 변수, 호이스팅 등을 먼저 이해하고 오도록 하자.

<br/>

# 📌 클로저(Closure)
## 1. 어휘적 환경(Lexical Enviroment)

JS에는 어휘적 환경(Lexical Enviroment)이라는 것이 존재한다. 이전에 배웠다시피 스크립트에서 선언한 변수들은 호이스팅, 즉 Lexical 환경으로 올라간다.

다음 코드를 참고하여 일련의 과정을 살펴보자.

<br/>

### Lexical 환경 진행과정

```javascript
let one;
one=1;

function addOne(num) {
  console.log(one+num);
};

addOne(5)
```

1. 스크립트에서 선언된 변수들(변수 `one`과 함수 `addOne`)이 Lexical환경으로 이동한다.

	  > **전역 Lexical 환경**
	  >
    >  - one : let으로 선언되어서 초기화 되지 않은 상태**_(사용불가)_**
    >  - addOne : function _**(사용가능)**_

2. `let one`을 만나면 `undefined`값이 할당된다. 이 상태가 되면 사용이 가능하다.

3. `one=1`을 만나면 one에 1이라는 값이 할당된다.

4. 함수선언은 초기에 완료되었기 때문에 마지막 라인의 `addOne(5)`가 실행된다.

5. 함수가 실행되면서 새로운 Lexical 환경이 만들어진다. 이곳에서는 함수에서 넘겨받은 매개변수와 지역변수들이 저장된다.

	  > **함수 내부 Lexical 환경**
    >
    >- num : 5

6. 이때 내부 Lexical 환경은 함수 실행에 필요한 변수를 찾을 때 먼저 찾는 장소가 되고, 만약 없다면 점차 상위 Lexical 환경으로 넘어가 참조한다.

7. 본 예시에서는 `one`이라는 변수가 함수 내부에 없기 때문에 상위인 전역 Lexical에서 one을 참조하여 함수를 실행한다.

<br/>
<br/>

## 2. 클로저와 렉시컬 환경

이제 이해를 돕는 추가 예문를 살펴보며, 렉시컬 환경이 클로저와 어떠한 연관이 있는지 알아보도록 하자.

### 💡 추가 예문

```javascript
function makeAdder(x) {
  return function(y) {
    return x+y;
  }
};

const add3 = makeAdder(3);
console.log(add3(2));
```

1. 스크립트 실행시 선언된 변수들(함수 `makeAdder`와 변수 `add3`)이 전역 Lexical 환경으로 이동한다.

	  > **전역 Lexical 환경**
    >  - makeAdder : function
    >  - add3 : 초기화 X

2. 함수선언은 이미 되어 있으니 바로 `const add3 = makeAdder(3)`이 실행 되고, 이때 함수의 내부 Lexical 환경이 만들어진다.

	  >**_makeAdder_ Lexical 환경**
    >- x : 3

3. 그리고 `add3`에 대입된 makeAdder함수가 실행되면서 return으로 내부에 작성된 함수가 add3의 값으로 할당 된다.

	  >**전역 Lexical 환경**
    >- makeAdder : function
    >- add3 : function

4. 마지막 줄인 `console.log(add3(2))`이 실행되면 함수 `add3(2)`가 실행되고 Lexical 환경이 만들어지는데, 이 함수의 내용은 makeAdder의 안에 있다.

    즉, 다시 또 add3( )에 대한 Lexical 환경이 생성될 때 이 환경은 makeAdder의 하위 환경이다.

	  >**익명함수 Lexical 환경**
    >  - y : 2

5. 이제 변수를 찾으며 코드가 실행되는데 **_Lexical 환경의 서치 순서와 참조_**는 다음과 같다.

	  >익명함수 ─ 참조 → makeAdder ─ 참조 → 전역
    >  1. 익명함수 Lexical에서 x와 y를 찾는다.
    >  2. y를 makeAdder에서 찾는다.

<br/>

이제 위에서 본 예시를 다시 한번 정리해보자. `makeAdder 함수`의 내부에 작성된 함수는 변수 y를 지니고 있으면서 상위 함수의 x에 접근이 가능하다.

그리고 `add3 함수`가 생성된 이후에도 상위 함수인 makeAdder의 x에 접근이 가능하다.

**_이것이 바로 `클로저(Closure)`이다._**

<br/>

풀어 해석하면 클로저는 함수와 렉시컬 환경의 조합으로써, 함수가 생성될 당시의 외부 변수를 기억해두어서, 생성 이후에 함수의 실행이 종료되어 소멸되어도 계속 접근이 가능한 것이다.

만약 아래같이 새로운 함수를 makeAdder함수를 이용해 만들더라도 이전에 할당했던 add3함수에는 영향을 미치지 않는다.

```javascript
const add3 = makeAdder(3);
console.log(add3(2));		// 5

const add10 = makeAdder(10);
console.log(add10(5));		// 15
console.log(add3(1));		// 4
```

결과에서 보이듯이 add3에서는 `x=3` 그리고 add10에서는 `x=10`으로 별개의 값을 지니고 있다.  
**_그 이유는 add3과 add10이 서로 다른 환경을 지니고 있기 때문이다._**

<br/>

### 👀 한번 더 살펴보기

```javascript
function makeCounter() {
  let num=0;
  
  return function() {
    return num++;
  };
};

let counter = makeCounter();

console.log(counter());		// 0
console.log(counter());		// 1
console.log(counter());		// 2
```

위 코드를 보면 콘솔창에 `counter()`가 실행될 때마다 계속 숫자가 증가하는 것을 볼 수 있다.

이는 내부 함수에서 외부 함수의 변수인 `num`을 기억하여 실행될 때마다 **_접근하고 다시 기억하기 때문_** 이다.

<br/>

### 은닉화 성공?!

그렇다면 1씩 증가시키는 것이 아니라 마음대로 num을 수정할 수 있을까? 불가능하다. 오직 `counter()`로 접근하여 1씩 증가시키는 것만이 가능하다.

**_즉, 은닉화에 성공한 것이다._**

<br/>

**_이렇게 종료된 함수의 변수에 접근할 수 있으며, 변수를 은닉화 시킬 수 있는 것이 `클로저`이다._**

<br/>
<br/>

# Reference

- **[코딩앙마님의 인프런 강의](https://www.inflearn.com/course/%EC%99%95%EC%B4%88%EB%B3%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)**