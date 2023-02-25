**[ES6초급강의](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/ES6/01_ES6_%EA%B8%B0%EC%B4%88.md) 수강후 이어지는 강의 정리입니다.**

이전에 공부했던 [JS기초공부](https://github.com/Jeong-jj/javascript-self-study/tree/main/archive/Javascript%EA%B8%B0%EC%B4%88)에 이어서 하는 공부이기에 부족했던 부분에 대해서만 작성하고, 강의에서 이전 공부와 중복되는 부분은 생략하고 작성하였습니다.

<br/>

# 📌 심볼(Symbol)

>_property key : 문자형_
>
>본래 객체의 key값은 문자형으로 반환되며, 접근도 문자형으로 가능하다.
>
>```javascript
>const obj = {
>  1:"1입니다",
>  false:"거짓"
>};
>
>// key 반환
>Object.keys(obj);	// {"1", "false"}
>
>// key 접근
>obj["1"]		// "1입니다"
>```
>
>그런데 이러한 key로 들어올 수 있는 또 다른 값이 바로 `심볼(Symbol)`이다.

<br/>

## 1. Symbol 작성 & 이용

심볼을 만들 때는 아래와 같이 만들며 new는 붙이지 않는다.

	const a = Symbol();

이러한 심볼을 key값으로 사용하려면 다음과 같이 작성한다.

```javascript
const id = Symbol('id');

const user = {
  name:'Mike',
  age:30,
  [id]:'myId'
}
```

<br/>

### 👀 심볼의 효용

이러한 심볼은 어디에 사용하는 것일까?

**_바로 원본 데이터를 건드리지 않고 속성을 추가하고자 할 때이다._**

작업을 여러 사람이 사용하다보면 하나의 객체에 많은 사람이 접근해야할 수가 있다. 그런데 내가 마음대로 객체를 수정하면 다른 코드에 영향을 미칠 수 있기 때문에 본인의 작업에서만 추가된 속성을 작성하기 위해서 이러한 심볼을 이용하는 것이다.

<br/>
<br/>

## 2. Symbol의 특징
### 1) 유일성 보장

이러한 심볼은 유일한 식별자를 만들고자 할 때 사용한다. 아래를 보면 보다 쉽게 이해가 가능할 것이다.

```javascript
const a = Symbol();
const b = Symbol();

console.log(a);	// Symbol()
console.log(b);	// Symbol()
```

위를 보면 a와 b모두 console에 찍어봤을 때 Symbol( )이라고 나온다. 그러나 이를 비교연산자를 통해 비교해보면?

```javascript
a == b		// false
a === b		// false
```

전부 일치하지 않는다고 나온다. 즉, 둘은 별개의 유일한 **심볼**이 된 것이다.  
_**Symbol은 유일성이 보장된다 !**_

<br/>

### 2) 작업에서의 제외

이렇게 심볼로 만들어진 property는 Object Method와 for문 등의 작업에서 제외된다.

```javascript
const id = Symbol('id');

const user = {
  name:'Mike',
  age:30,
  [id]:'myId'
}

Object.keys(user);	// {"name", "age"}
```

위에서 보면 `.key()`메소드에서 \[id]로 작성된 심볼은 제외되고 객체가 만들어진 것을 볼 수 있다.

<br/>
<br/>

## 3. Symbol Method
### 1) Symbol.for( )

`Symbol.for()`를 이용해 심볼을 만들면 `전역심볼`을 만들 수가 있다. 전역심볼의 특징은 다음과 같다.

1. 하나의 심볼만을 보장한다.
2. 작성하고자 하는 심볼이 없으면 생성하고, 있으면 공유한다.

<br/>

위 특징에서 알 수 있듯이 Symbol( )과의 차이점은 `Symbol()`은 매번 다른 심볼을 만드는 데 비해 `Symbol.for()`은 하나를 생성하고 공유한다는 것이다.

```javascript
const id1 = Symbol.for('id');
const id2 = Symbol.for('id');

id1 === id2	// true
```

때문에 위에서 `id1 === id2`는 `true`가 나온다.

<br/>

### 2) Symbol.keyFor( )

이를 이용하면 심볼들이 지니는 key가 될 값의 이름을 얻을 수 있다.

```javascript
const id1 = Symbol.for('id');
Symbol.keyFor(id1);		// "id"
```

<br/>

### 3) Symbol.description

전역심볼이 아닐 경우엔 keyFor메소드를 사용할 수 없다. 때문에 전역심볼이 아닐 때는 description메소드를 이용한다.

```javascript
const id = Symbol('id');
id.description;			// "id"
```

> 이때 description뒤에 `()`는 붙이지 않는다.

<br/>
<br/>

## 4. Symbol 찾기

심볼을 유일한 property로써 이용하기 위해 만들고 본래 데이터에서는 숨겨진 것처럼 처리대상에서 제외되지만 아예 찾을 수 없는 것은 아니다. 다음과 같은 메소드로 심볼을 찾을 수 있다.

<br/>

### 1) Object.getOwnPropertySymbol( )

`()`안에 인수로 객체명을 작성하면 해당 객체가 지닌 심볼 값을 배열로 모아준다.

<br/>

### 2) Reflect.OwnKeys( )

`()`안에 인수로 객체명을 작성하면 객체의 key값을 심볼을 포함하여 보여준다.

> **_But !_** 대부분의 라이브러리, 내장함수는 이런 메소드를 거의 쓰지 않는다.
>
>_∴ 그러니 유일한 property를 추가하고자 할 때 맘편히 Symbol을 쓰자 !_

<br/>
<br/>

## 5. Symbol로 메소드 추가 & 사용

아래 코드로 간단히 이해하고 넘어가도록 하자.

```javascript
const a = Symbol('A');

let user = {
  name:'Mike',
  age:30
}

// 메소드 추가
user[a] = function() {/* 메소드 내용 */}

// 메소드 사용
user[a]();
```

<br/>
<br/>

# Reference

- **[코딩앙마님의 인프런 강의](https://www.inflearn.com/course/%EC%99%95%EC%B4%88%EB%B3%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)**