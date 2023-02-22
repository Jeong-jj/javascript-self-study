Boolean값, 즉 true와 false의 대체재로 사용될 수 있는 값을 짚고 넘어가자.

<br/>

# 📌 True값

```javascript
if(true)
= if(1)
= if('문자열')
= if(!undefined)
```

> 참고: `{}`와 `[]`는 true값을 가진다.

<br/>
<br/>

# 📌 False값

```javascript
if(false)
= if(0)	// 1 외의 다른 숫자들
= if('')
= if(undefined)
```

이를 이해하면 위의 false값을 부정 `!`를 이용해 true값으로 뒤집을 수 있다.

```javascript
if(true)	// 1 외의 다른 숫자들
= if(!'')
= if(!undefined)

var a; // 아무 값이 할당되어 있지 않기 때문에 데이터값은 undefined
if(!a)
= if(true)
```

> [if( )의 true / false값](https://dorey.github.io/JavaScript-Equality-Table/)
위 링크에서 보다 자세히 살펴볼 수 있다.

<br/>

# Reference

- **[생활코딩님의 인프런 강의](https://www.inflearn.com/course/%EC%A7%80%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%96%B8%EC%96%B4-%EA%B8%B0%EB%B3%B8)**
