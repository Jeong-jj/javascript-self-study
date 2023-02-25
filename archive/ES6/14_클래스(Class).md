**[ES6초급강의](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/ES6/01_ES6_%EA%B8%B0%EC%B4%88.md) 수강후 이어지는 강의 정리입니다.**

이전에 공부했던 [JS기초공부](https://github.com/Jeong-jj/javascript-self-study/tree/main/archive/Javascript%EA%B8%B0%EC%B4%88)에 이어서 하는 공부이기에 부족했던 부분에 대해서만 작성하고, 강의에서 이전 공부와 중복되는 부분은 생략하고 작성하였습니다.

<br/>

이번 시간 class를 이해하기 위해선 `생성자 함수`와 `상속`, `prototype`에 대하여 알고 있어야 하기에 모른다면 링크를 눌러 이전 포스트를 참조하자.

- [JS기초 - 객체지향(1)](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/Javascript%EA%B8%B0%EC%B4%88/15.1_%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5(1).md) : 생성자 함수
- [JS기초 - 객체지향(2)](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/Javascript%EA%B8%B0%EC%B4%88/15.2_%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5(2).md) : 상속 & prototype
- [ES6 - 상속](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/ES6/13_%EC%83%81%EC%86%8D_prototype.md) : 상속

<br/>

# 📌 Class

`class`는 ES6에서 추가된 스펙이다.

이전에 비슷한 객체를 만들기 위해 생성자 함수를 이용했는데 class를 이용해서도 비슷한 객체를 만드는 것이 가능하다.

**_그렇다면 생성자 함수를 이용한 것과 어떠한 공통점과 차이점이 있는지 살펴보며 class에 대해 알아보자._**

<br/>

## 1. 생성자 함수와의 공통점

### 🌱 생성자 함수
```javascript
const User1 = function(name, age) {
  this.name = name;
  this.age = age;
};
User1.prototype.showName = function() {
    console.log(this.name);
};

const mike = new User1("Mike", 30);
```

### 🌱 class

```javascript
class User2 {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  showName() {
    console.log(this.name);
  }
};

const mike = new User2("Mike", 30);
```

위의 생성자 함수에 의해 만들어진 mike와 class에 의해 만들어진 mike는 동일하다.

생성자 함수에서 만드는 객체의 내용을 class함수에서는 `constructor`라는 부분에서 처리하게 된다. constructor는 인수를 전해 받을 수도 있다.

> 즉, `new Uer2`에 의해 만들어진 mike는 constructor에 의해 그 안의 내용을 담은 객체로 만들어지는 것이다.

<br/>
<br/>

## 2. 생성자 함수와의 차이점

그렇다면 둘의 차이는 무엇일까? class라는 새로운 기능을 사용해서 얻는 이점은 무엇일까?

### 1) prototype 작성의 편의성(상속세팅)

생성자 함수에서는 `.prototype`을 이용해 상속하고자 하는 내용을 작성해주어야 했다.

```javascript
class User2 {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  showName() {
    console.log(this.name);
  }
};
```

그러나 `class`에서는 그럴 필요없이 class안에 작성되었지만 constructor에 담기지 않은 내용이 자동으로 상속하고자 하는 내용이 된다.

**_즉, 위의 showName( )메소드가 상속하는 내용이 되는 것이다._**

---

<br/>

### 2) new 미작성에 대한 경고 !

우리는 생성자 함수를 쓰고자할 때 생성자 앞에 `new`를 붙여야 한다는 것을 배웠었다. 이는 class를 쓸 때도 마찬가지이다.

**_class명 앞에 new를 붙여 실행해준다._**

그리고 생성자 함수를 실행할 때 new를 붙이지 않는다면 어떻게 되는지도 배웠다.
일반 함수가 실행되는 것이기 때문에 return값이 없는 생성자 함수의 일반 실행은 아무것도 넘겨주지 못한다.

<br/>

#### 🌱 생성자 함수

```javascript
const User1 = function(name, age) {
  this.name = name;
  this.age = age;
};
User1.prototype.showName = function() {
    console.log(this.name);
};

const mike = User1("Mike", 30);
```

이러한 실수는 전체적인 코드 실행에 치명적일 수 있을 것이다. 그러나 우리가 new없이 생성자 함수를 실행해도 어떠한 경고도 띄우지 않는다.

<br/>

![생성자함수](https://user-images.githubusercontent.com/96231175/221205976-0a8f4b3c-72f1-428f-bcb8-92d5ec9e67e3.png)

<br/>

왜냐? 그저 일반 함수를 처리하는 것으로 인식하기 때문에 문제삼지 않기 때문이다. 단지 받는 값이 없어 undefined로 뜰 뿐이다.

**_그렇다면 class는 어떨까?_**

<br/>

#### 🌱 class

```javascript
class User2 {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  showName() {
    console.log(this.name);
  }
};

const mike = User2("Mike", 30);
```

![class실행](https://user-images.githubusercontent.com/96231175/221206001-e6aa39cf-f827-41b1-96d8-714a18066d7f.png)

<br/>

보다시피 mike를 찍어볼 필요도 없이 이미 new없이는 class를 실행할 수 없다는 `typeError 경고문`을 띄우고 있다.

이를 통해 우리가 작성한 코드의 오류를 줄일 수 있게 된다.

<br/>

>### **constructor 표기**
>
>이는 생성자 함수와 class로 만든 객체를 들여다보면 보다 명확히 알 수 있는데,
>생성자 함수로 만든 객체에는 constructor가 그냥 `f( )`함수로 표기되는 것에 반해, class에서는 constructor에 `class 클래스명`이 표기되어 나타나기 때문이다.
>
>따라서 실행할 때 class라는 것이 인식되면 new없이는 error경고창을 띄운다.

---

<br/>

### 3) 반복문의 편의성

이전에 상속을 배울 때 `생성자 함수`에서는 `for in문` 실행시 본인이 소유한 property외에 상속받은 prototype의 property까지 순회한다는 것을 배웠다.

<br/>

![생성자함수의 for-in문](https://user-images.githubusercontent.com/96231175/221205999-ec2b1cbd-9981-4ce1-9a35-f0a433fd72a1.png)

<br/>

때문에 상속받은 것을 제외하고 싶을 때는 `.hasOwnProperty()`메소드를 사용해야만 했었다.

**_그러나 class에서는 이런 번거로움을 줄일 수 있다._**

<br/>

![class의 for-in문](https://user-images.githubusercontent.com/96231175/221205997-b31e2daa-b15c-4123-b31a-db58d9e1dd16.png)

<br/>

보이는 바와 같이 `class`로 만든 객체의 `for in문`에서는 상속되는 property는 순회하지 않는다.

그렇기 때문에 직접 본인 소유와 상속 property를 구분해줄 번거로움이 적다.

---

<br/>

### 4) 상속 방법

위에서 class에서 상속 세팅을 하는 방법을 알아보았다. 그렇다면 이제 상속을 시키는 방법을 알아보며 생성자 함수와의 차이점을 보자.

<br/>

#### 🌱 extends

class에서는 `extends`라는 것을 이용해서 상속을 시킨다.

```javascript
class Car {
  constructor(color) {
    this.color = color;
    this.wheels = 4;
  }
  drive() {
    console.log("drive..");
  }
  stop() {
    console.log("STOP!");
  }
};

class Bmw extends Car {
  park() {
    console.log("PARK");
  }
};

const z4 = new Bmw("blue");
```

<br/>

위 코드와 같이 작성하면 **_Bmw가 extends를 이용해 Car를 상속한 것이다._**

<br/>

![상속내용](https://user-images.githubusercontent.com/96231175/221205994-13e42a9f-9d76-4a17-b1f5-e117048699a6.png)

<br/>

이렇게 하므로써 z4는 Bmw class의 내용뿐 아니라 Car의 상속 내용인 drive와 stop메소드까지 사용할 수 있게 된다.

<br/>
<br/>

# 📌 class의 Overriding 

>### 상속 내용 덮어쓰기
>
>만약 class를 사용할 때 상속 과정에서 prototype과 같은 이름의 메소드를 만들면 어떻게 될까?
>
>![상속 덮어쓰기](https://user-images.githubusercontent.com/96231175/221205990-88db1917-e155-47f7-95e6-117e5fa570d6.png)
>
>위에 보이는 바와 같이 이전 내용을 덮어쓰게 된다. 때문에 `z4.stop()`을 실행했을 시 "STOP!"이 아니라 "OFF!"가 나오는 것을 볼 수 있다.

<br/>
<br/>

## 상속 확장하기 feat. super
### 메소드의 overriding

그렇다면 상속되는 메소드의 내용을 덮어쓰는 것이 아니라 확장하고 싶다면 어떻게 해야할까? 바로 `super`라는 것을 사용하면 된다.

```javascript
class Car {
  constructor(color) {
    this.color = color;
    this.wheels = 4;
  }
  drive() {
    console.log("drive..");
  }
  stop() {
    console.log("STOP!");
  }
};

class Bmw extends Car {
  park() {
    console.log("PARK");
  }
  stop() {
    super.stop();
    console.log("OFF!");
  }
};

const z4 = new Bmw("blue");
```

![class의 super](https://user-images.githubusercontent.com/96231175/221205986-f9d11af5-c5dd-4b92-999e-8aa613afbedd.png)

<br/>

위의 코드를 보면 Bmw의 stop메소드 안에 `super.stop();`를 작성해주었다.  
이렇게 하면 기존 Car의 stop메소드를 유지하게 되어 `z.stop()` 실행시 "STOP!"과 "OFF!"가 모두 나온 것이다.

_**이렇게 기존의 부모 class의 property를 유지하며 사용하는 것을 `overriding`이라고 하는 것이다.**_

<br/>

### constructor의 overriding

그렇다면 이번엔 constructor의 overriding을 살펴보자.  
아래를 보면 Bmw 클래스에 constructor 내용을 추가해주었다. _어떻게 될까?_

```javascript
class Car {
  constructor(color) {
    this.color = color;
    this.wheels = 4;
  }
  drive() {
    console.log("drive..");
  }
  stop() {
    console.log("STOP!");
  }
};

class Bmw extends Car {
  constructor () {
    this.navigation = 1;
  }
  park() {
    console.log("PARK");
  }
};

const z4 = new Bmw("blue");
```

![constructor의 super](https://user-images.githubusercontent.com/96231175/221205981-58498ac0-4487-4fcd-beb2-b96fa5b96305.png)

<br/>

보이다시피 반드시 `super`를 사용하라는 경고문이 뜬다. 이렇듯이 `extends`로 만든 class에서 constructor를 작성하고자 하면 상속받은 class의 constructor내용을 `super`를 통해 받아와야한다.

<br/>

```javascript
class Bmw extends Car {
  constructor () {
    super();
    this.navigation = 1;
  }
  park() {
    console.log("PARK");
  }
};
```

<br/>

이렇게 해야하는 이유는 그냥 Car와 같이 class를 작성했을 경우엔 constructor가 빈 객체`{}`를 만들고, 그 다음 this가 이 객체를 가르키는 과정을 갖는다.

그러나 `extends`로 class를 만들었을 경우엔 constructor가 이 과정을 건너뛰기 때문에 경고가 뜨는 것이다.

<br/>

### 인수 넘겨받기

그러나 한가지 중요한 과정이 남아 있다. **바로 Car에서 사용한 인수를 Bmw에서도 받아주는 과정이다.**

**_만약 이 과정을 해주지 않으면 Bmw로 만든 z4객체에서 color라는 key의 value값이 undefined로 뜨게 된다._**

<br/>

```javascript
class Bmw extends Car {
  constructor (color) {
    super(color);
    this.navigation = 1;
  }
  park() {
    console.log("PARK");
  }
};
```

> ### 내부 과정 살펴보기
>
>만약 자식 class에 constructor를 작성하지 않았을 땐 어떤 일이 일어나길래 error가 뜨지 않는 것일까?
>
>```javascript
>class Bmw extends Car {
>  constructor (...args) {
>    super(...args);
>  }
>  park() {
>    console.log("PARK");
>  }
>};
>```
>바로 위와 같은 constructor코드가 있는 것처럼 작동하여 자동적으로 기존의 부모 class의 constructor값을 가져오는 것이다.
>>
>>**즉, 우리가 입력하지 않을 때도 이미 자식 class는 부모 class의 constructor값을 가져오고 있다.**
>>
>>**_따라서 직접 constructor를 입력하면 저 위의 자동적인 과정이 진행되지 않기 때문에 인수를 받는 과정 또한 직접 작성해주어야 한다._**

<br/>
<br/>

# Reference

- **[코딩앙마님의 인프런 강의](https://www.inflearn.com/course/%EC%99%95%EC%B4%88%EB%B3%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)**