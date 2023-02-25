**[ES6초급강의](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/ES6/01_ES6_%EA%B8%B0%EC%B4%88.md) 수강후 이어지는 강의 정리입니다.**

이전에 공부했던 [JS기초공부](https://github.com/Jeong-jj/javascript-self-study/tree/main/archive/Javascript%EA%B8%B0%EC%B4%88)에 이어서 하는 공부이기에 부족했던 부분에 대해서만 작성하고, 강의에서 이전 공부와 중복되는 부분은 생략하고 작성하였습니다.

<br/>

JS에서는 일반적인 방법 외에도 함수의 호출 방식과 상관없이 this를 지정할 수 있는 방법이 있다. 이번에 알아보려는 것이 이것과 관련된 기능들이다.

<br/>

# 📌 call
## 1. 사용 방법

`call` 메소드는 모든 함수에서 사용 가능하며, this를 특정 값으로 지정할 수 있다.

```javascript
const mike = {
  name:"Mike",
};

const tom = {
  name: "Tom",
};

function showThisName() {
  console.log(this.name);
}

showThisName();				// 아무것도 나타나지 않는다.
showThisName.call(mike);	// "Mike"
```

위 코드를 보면 showThisName이라는 함수를 만들었고, 내부에서 this를 통해 해당 객체의 name값을 가져오도록 했다.

그런데 만약 그냥 실행만 한다면 이 함수의 this는 window이기 때문에 아무것도 나타나지 않는다.

<br/>

>window.name은 `""`빈 문자열

<br/>

이럴 때 실행시킬 함수에 call메소드를 이용해서 인자로 this로 사용할 객체를 넘기면, 이 함수를 해당 객체의 메소드인 것처럼 사용할 수 있다.

**_때문에 함수 내의 this가 인자로 넣은 mike가 된 것이다._**

<br/>
<br/>

## 2. call의 매개변수

- 첫번째 매개변수 : 해당 함수의 this가 된다.
- 두번째 이후의 매개변수 : 해당 함수에서 사용할 매개변수 값에 순서대로 들어간다.

```javascript
const mike = {
  name: "Mike",
};

function update(birthYear, occupation) {
  this.birthYear = birthYear;
  this.occupation = occupation;
};

update.call(mike, 1999, "doctor");
console.log(mike);		// {birthYear:1999, name:"Mike", occupation:"doctor"}
```

<br/>

update함수를 통해 인자를 받아 객체에 새로운 값을 추가하도록 만들었다.

그리고 여기에 call로 인자를 받았는데 받아온 인자는 다음과 같이 사용된 것이다.

- mike : update함수의 this
- 1999, "doctor" : update의 매개변수에 들어갈 값


<br/>
<br/>

# 📌 apply

`apply`는 함수의 매개변수를 처리하는 방법만 제외하면 call과 완전히 동일하다.

다른 점인 매개변수의 처리에 대해서 보자면, call은 일반적인 함수와 마찬가지로 직접 매개변수를 받는 반면, **_apply는 매개변수를 배열로 받는다._**

<br/>

```javascript
const mike = {
  name: "Mike",
};

function update(birthYear, occupation) {
  this.birthYear = birthYear;
  this.occupation = occupation;
};

update.apply(mike, [1999, "doctor"]);
console.log(mike);
```

**call파트**에서 call로 처리했던 코드를 apply로 바꿔주면 위와 같이 되는 것이다.

보이는 바와 같이 함수의 매개변수로 사용할 인자를 `[1999, "doctor"]` 배열로 만들었다. 결과는 이전과 동일하다.

<br/>

> ## call 과 apply의 매개변수 처리 비교
> ### feat. spread 연산자
>
>```javascript
>const num = [3, 1, 7, 10];
>
>const applyMin = Math.min.apply(null, num);
>			// = Math.min.apply(null, [3, 1, 7, 10])
>const callMin = Math.min.call(null, ...num);
>			// = Math.min.apply(null, 3, 1, 7, 10)
>```
> #### 위 두 함수는 this값이 없기 때문에 null을 넣어주었다. _(아무값이나 넣어도 된다)_
> 위에서 `apply`는 배열을 함수 매개변수로 받기 때문에 그대로 작성하였고, `call`에서는 풀어넣어주어야 하기 때문에 스프레드 연산자 `...`를 이용하여 넣어준 것을 볼 수 있다.

<br/>
<br/>

# 📌 bind

`bind`를 이용하면 this의 값을 영구히 바꿀 수가 있다.

위에서 사용했던 예문을 이용해서 이번엔 update함수의 this가 mike로 고정되도록 해보자.

```javascript
const mike = {name: "Mike"};

function update(birthYear, occupation) {
  this.birthYear = birthYear;
  this.occupation = occupation;
};

const updateMike = update.bind(mike);	// this값 고정

updateMike(1980, 'police');
console.log(mike);		// {{birthYear:1980, name:"Mike", occupation:"police"}
```

보다시피 updateMike함수에 update에 대한 인자만 넣어주고 this값을 알려주지 않아도 이미 mike로 고정되었기 때문에 mike객체의 내용이 바뀐 것을 볼 수 있다.

<br/>
<br/>

# 📢 실용 예제
### 아래에서 fn( )호출이 제대로 되지 않은 원인을 찾고 이를 수정해보아라(call, apply, bind를 이용).

```javascript
const user = {
  name:"Mike",
  showName: function() {
    console.log(`hello, ${this.name}`)
  },
};

user.showName();		// "hello, Mike"
let fn = user.showName

fn();		// "hello, "
```

<br/>

> **정답**
> 1. 원인
>	- fn안에 user.showName을 담았지만 이 과정에서 this인 user를 잃어버려서 fn( )호출 시 this.name을 못찾고 출력된 것이다.
>
>2. 수정
>```javascript
>const user = {
>  name:"Mike",
>  showName: function() {
>    console.log(`hello, ${this.name}`)
>  },
>};
>
>user.showName();
>let fn = user.showName
>
>fn.call(user);			// "hello, Mike"
>fn.apply(user);			// "hello, Mike"
>
>let bindFn = fn.bind(user);
>
>bindFn();				// "hello, Mike"
>```

<br/>
<br/>

# Reference

- **[코딩앙마님의 인프런 강의](https://www.inflearn.com/course/%EC%99%95%EC%B4%88%EB%B3%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)**