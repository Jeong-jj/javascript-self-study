**[ES6초급강의](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/ES6/01_ES6_%EA%B8%B0%EC%B4%88.md) 수강후 이어지는 강의 정리입니다.**

이전에 공부했던 [JS기초공부](https://github.com/Jeong-jj/javascript-self-study/tree/main/archive/Javascript%EA%B8%B0%EC%B4%88)에 이어서 하는 공부이기에 부족했던 부분에 대해서만 작성하고, 강의에서 이전 공부와 중복되는 부분은 생략하고 작성하였습니다.

<br/>

이전에 [객체지향(2)](https://github.com/Jeong-jj/javascript-self-study/blob/main/archive/Javascript%EA%B8%B0%EC%B4%88/15.2_%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5(2).md)에서 생성자 함수에서의 `상속`에 대해 알아보았다. 이번엔 이전 방법과 다른 방식의 상속 방법에 대해서 알아보자.

<br/>

# 📌 상속

```javascript
const car1 = {
  color: "red",
  wheels:4,
  navigation:1,
  drive() {
    console.log("drive..");
  },
};

const car2 = {
  color: "green",
  wheels:4,
  navigation:1,
  drive() {
    console.log("drive..");
  },
};

const car3 = {
  color: "blue",
  wheels:4,
  navigation:1,
  drive() {
    console.log("drive..");
  },
};
```

위 세개의 객체를 보면 같은 내용의 property를 소유하고 있는 것을 볼 수 있다. 이걸 매번 새로운 car객체를 만들 때마다 작성한다면 매우 비효율적일 것이다.

이미 보이다시피 코드가 매우 길다..  
**_그래서 이러한 비효율적인 코드를 줄이기 위해 상속을 이용하는 것이다._**

<br/>
<br/>

## 사용방법

```javascript
const carProto = {
  wheels:4,
  drive() {
    console.log("drive..");
  },
};

const car1 = {
  color: "red",
  navigation: 1,
};
const car2 = {
  color: "green",
};
const car3 = {
  color: "blue",
};

car1.__proto__ = carProto;
car2.__proto__ = carProto;
car3.__proto__ = carProto;
console.log(car1);
```

위를 보면 이전에 중복되던 내용을 `carProto`라는 객체에 넣어 만들어 주었다.

그리고 각각 다른 내용을 담은 객체에 `__proto__`라는 것을 이용해 각 객체의 prototype을 carProto로 지정해준 것이다.  
그래서 `console.log(car1);`을 보면 다음과 같이 나오는 것을 볼 수 있다.

<br/>

![prototype](https://user-images.githubusercontent.com/96231175/221204626-3d78c204-052f-4016-8b27-05cd11ab75d2.png)

<br/>

사진에서 보이다시피 car1이 기본적으로 지닌 color와 navigation이라는 property 외에 `[[prototype]]`이라는 것이 생겼고 이를 눌러보면 우리가 상속한 `carProto`의 property가 담긴 것을 볼 수 있다.

이렇게 하면 다른 각 car1~3객체는 carProto의 property에 접근할 수 있고, 메소드도 사용할 수 있다.

<br/>
<br/>

## prototype chain

이 방법으로 진행한 상속도 연속적으로 이어질 수 있다. 즉 상속의 연쇄고리가 생기는 것이다.

**_위 코드에 아래의 내용을 추가해보자_**

```javascript
const newCar = {
  color: "black",
  name: "new one",
};

newCar.__proto__ = car1;
```

이렇게 하면 `newCar`는 car1의 내용뿐 아니라 car1이 상속받았던 carProto의 내용까지도 상속받게된다.

<br/>

이렇게 상속이 이뤄진 객체의 실행은 다음 예시와 같다.

1. `newCar.name`을 찍으면 바로 보인의 객체에서 그 값을 찾아서 `"new one"`을 출력하고,

2. `newCar.navigation`을 찍으면 상위 prototype인 car1에서 찾고 그 값을 찾으면 `1`을 출력한다.

3. `newCar.drive()`를 실행한다면? 이제 예상할 수 있듯이 본인에게 없기때문에 먼저 상위 proto인 car1에서 찾고, 이곳에도 없으면 그 상위 proto인 carProto에서 찾아 실행하여 `"drive.."`를 출력하게 된다.

_**이것이 바로 prototype chain이다.**_

<br/>

> **※ 주의할 점**
>
>이렇게 상속을 받은 객체에서 반복문이나 객체 메소드를 사용하면 어떻게 될까?
>
>1. for in문
>```javascript
>for (p in newCar) {
>  console.log(p);
>};
>/**
>	color
>    name
>    navigation
>    wheels
>    drive
>*/
>```
>반복문에서는 상속받은 property 또한 돌게 된다.
>만약 상속 내용은 돌고 싶지 않다면 `.hasOwnProperty()`메소드를 이용하면 된다. 이를 이용하면 자신이 실제로 지닌 property만 true로 반환해준다.
>
>2. .keys( ), .values( ) 등
>```javascript
>Object.keys(newCar);	// ["color", "name"]
>Object.values(newCar);	// ["black", "new one"]
>```
>객체 메소드에서는 상속받은 property는 나오지 않는다.

<br/>
<br/>

## instance

가볍게 참고할 부분으로써, 우리가 생성자를 통해 객체를 만들면 그 객체를 해당 생성자의 instance라고 부른다.

그리고 이를 `instanceof`라는 것을 이용해 확인해 볼 수 있다. 만약 일치하는 생성자라면 true를, 아니라면 false를 반환한다.

```javascript
const Car = function(name) {
  this.name = name;
};

const bmw = new Car();

bmw instanceof Car;		// true
```

<br/>
<br/>

# 📢 기타 응용 : 상속과 클로저

상속된 내용을 통해 작성된 property를 바꾸지 못하게 하고 싶다면? 이전에 배웠던 클로저를 이용하면 된다.

```javascript
const Car = function(color) {
  const c = color;
  this.getColor = function() {
    console.log(c);
  };
};

const car1 = new Car();
car.getColor();		// red
```

이렇게 하면 초기에 세팅한 color값을 얻을 수만 있고 변경하는 것은 불가능하게 된다.

<br/>
<br/>

# Reference

- **[코딩앙마님의 인프런 강의](https://www.inflearn.com/course/%EC%99%95%EC%B4%88%EB%B3%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)**