# -JavaScript Questions 154-

@URL :[GitHub - lydiahallie/javascript-questions: A long list of (advanced) JavaScript questions, and their explanations](https://github.com/lydiahallie/javascript-questions)

**1. What's the output?**

```jsx
function sayHi() {
  console.log(name);
  console.log(age);
  var name = 'Lydia';
  let age = 21;
}

sayHi();
```

- A: `Lydia` and `undefined`
- B: `Lydia` and `ReferenceError`
- C: `ReferenceError` and `21`
- D: `undefined` and `ReferenceError`


<details markdown="1">
<summary>Answer</summary>
D

이 문제는 **변수의 호이스팅을 물어보고 있다.** 자바스크립트의 모든 선언문은 호이스팅을 통해 자신의 스코프의 최상단으로 이동해 런타임 전 가장 먼저 선언된다. var의 경우에는 선언과 동시에undefined로 초기화가 이루어져 출력이 가능하지만 let으로 선언된 변수의 경우 호이스팅을 통해 선언은 되어도 자신의 선언문에 도달했을 때 비로소 초기화가 이루어진다. 그 전까지는 변수에 접근하지 못하게 된다. 이때 선언되고 선언문에 도달하기전까지 접근할 수 없는 이 사각지대를 일시적 사각지대(TDZ)라고 한다. 
</details>



**2. What's the output?**

```jsx
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` and `0 1 2`
- B: `0 1 2` and `3 3 3`
- C: `3 3 3` and `0 1 2`



<details markdown="1">
<summary>Answer</summary>
C

이 문제는 **비동기로 작동하게 되는 콜백함수와 클로저에 대한 이해**와 더불어 **블록레벨 스코프, 함수레벨 스코프**에 대한 지식을 필요로 한다. 

 우선 두 반복문 모두 0.001초라는 짧은 시간을 지정해도 for문의 속도가 더 빠르기 때문에 for문이 작동하고 setTimeout함수가 뒤이어 3번을(0,1,2)실행하게 된다. (선언은 코드가 실행되며 for과 같이 되지만 실제로 동작하는 속도가 느린 것 뿐이다.)

 이때 첫번째 반복문의 경우 var로 선언한 변수 i는 함수레벨 스코프이기 때문에 바깥 전역에 변수를 선언하게 되고 반복문 실행 후 결과 3으로 남아있는 상태이다. 세번의 반복동안 생성된 화살표함수내 i는 각자 클로저를 형성하여 본인이 선언되는 시점의 i를 가리키는데 그것은 결국 전역 변수로 선언된 같은 i를 가리키는 것이다. 즉 세 화살표함수 모두 반복문을 마친 3값을 가르키게 되고 실행이 작동할 때는 이미 반복문을 모두 마친 3을 가진 i를 출력하게 된다. 

두번째의 경우 let은 블록레벨스코프로 반복문이 돌때마다 새로운 let을 생성하게 된다. 즉 화살표 함수들은 각자 자신의 선언시점의 for블록안에 선언된 각각의 변수 i를 가리키게 되어 0,1,2를 출력할 수 있게 되는 것이다. 

참조:[https://www.youtube.com/watch?v=RZ3gXcI1MZY](https://www.youtube.com/watch?v=RZ3gXcI1MZY)
</details>




**3. What's the output?**

```jsx
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius,
};

console.log(shape.diameter());
console.log(shape.perimeter());
```

  

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`



<details markdown="1">
<summary>Answer</summary>
B


이 **문제는 화살표함수에는 this가 없다는 걸 알아야 이해할 수 있다.** this는 실행 컨텍스트가 생성될 때 함께 바인딩 됩니다. 즉 함수를 호출할 때 결정이 된다는 것인데 this는 기본적으로는 전역객체를(브라우저: window, node.js: global)을 가리킵니다. 객체내 메소드의 경우 호출시 객체에 의해 호출되고 this는 객체 shape을 가리키게 됩니다. 즉 this.radius가 10이 되는 것입니다. 하지만 화살표 함수는 this가 존재하지 않고 결국 this를 찾기위해 자신의 스코프체인의 상위로 올라가게 됩니다. 결국 자신의 상위 스코프인 전역으로 가서 this를 찾게 되고 그 this가 의미하는 것은 전역객체이기 때문에 존재하지 않는 radius를 가리키게 됩니다. 따라서 2 * undefined는 NaN이 되는 것입니다. 

-참조:[https://blog.naver.com/websearch/222122306928](https://blog.naver.com/websearch/222122306928)(자바스크립트Math객체 예제)


</details>


**4. What's the output?**

```jsx
+true;
!'Lydia';
```

- A: `1` and `false`
- B: `false` and `NaN`
- C: `false` and `false`


<details markdown="1">
<summary>Answer</summary>
A

문제는 **암묵적 타입변환을 물어보고 있다.** 자바스크립트는 암묵적으로 데이터 타입을 변환하는 경우가 있는데 true의 경우 + 단항 연산자를 만나면서 number타입으로 변환되고 1을 출력합니다.

(true는 1, false는 0을 의미)

두번째의 경우 문자열의 값에 부정을 의미하는 논리연산자 !를 사용하여 문자열의 데이터타입은 암묵적으로 boolen타입이 되고 “”빈값이 아닌 'Lydia'는 truthy를 의미하여 그의 부정이 false가 출력됩니다.
</details>


**5. Which one is true?**

```jsx
const bird = {
  size: 'small',
};

const mouse = {
  name: 'Mickey',
  small: true,
};
```

- A: `mouse.bird.size` is not valid
- B: `mouse[bird.size]` is not valid
- C: `mouse[bird["size"]]` is not valid
- D: All of them are valid


<details markdown="1">
<summary>Answer</summary>
A

이 문제는 **점 표기법과 대괄호 표기법의 차이를 이해해야 한다.** 점 표기법의 경우 그 뒤에 오는 것을 프로퍼티 키로 인식하고 찾게 된다. 즉 1번의 경우 mouse라는 객체안에 bird라는 키에 그 안에 또 size라는 프로퍼티 키를 가진 객체가 있는지를 물어보게되고 이는 존재하지 않기 때문에 유효하지 않다. (밑에 그림을 물어보는 것이라 할 수 있다.)

const mouse = {
name: 'Mickey',
small: true,
bird:{size : undefined}
};

하지만 대괄호 표기법은 []안에서 “”문자열이라는 것을 표시해주지 않으면 객체의 key를 찾는 것이 아니라 식별자로 선언된 것을 찾게 된다. 따라서 B,C의 경우에는 mouse자신 객체 밖에 있는 변수 bird로부터 값을 찾아온 것이다.

- 참고로 대괄호 표기법을 사용할 시에는 키값을 찾을 때 무조건 “”표시를 해줘야 하며 키값이 숫자라면 “”를 생략할 수 있다. 또 어떤 값을 할당하든 프로퍼티 키로 될 때는 암묵적으로 모두 문자열로 타입변환이 이루어진다. 즉 프로퍼티 키는 모두 문자타입이라고 할 수 있다.(Symbol제외)

- 참고:프로퍼티 키가 식별자 네이밍 규칙을 지키지 않는다면 키에 필수적으로 “”를 넣어준다.

예시) 

const mouse = {
  name: 'Mickey',
  “small-bird”: true,
};


</details>




**6. What's the output?**

```jsx
let c = { greeting: 'Hey!' };
let d;

d = c;
c.greeting = 'Hello';
console.log(d.greeting);
```

- A: `Hello`
- B: `Hey!`
- C: `undefined`
- D: `ReferenceError`
- E: `TypeError`

<details markdown="1">
<summary>Answer</summary>
A

이번 문제는 **값의 데이터 타입을 이해해야 한다.** 그중에서도 값이 전달(복사)될 때 값에 의한 전달이 이루어지는 원시타입의 값과 참조에 의한 전달이 이루어지는 객체타입의 값에 대한 이해를 해야한다. 자바스크립트의 객체 타입의 값은 변경가능한 값으로 이루어져있다.

- **여기서 변경 가능하다는 것은 객체 내부의 원시타입의 값에 변화를 말하는 것이지 객체 값 그 자체를 의미하는 것은 아니다.**

즉, 원시타입의 경우 메모리공간의 값이 있는 주소를 그대로 복사하여 넘겨주지만 객체타입의 값의 경우 복사하여 넘겨주는 주소가 값이 들어있는 주소가 아닌 메모리공간 어딘가에 그 객체가 존재하는 곳의 주소를 복사해서 넘겨준다. **쉽게 말해 객체의 주소를 복사해 주는 것이다.** 따라서 객체 타입의 값을 복사하게 되면 **결국 같은 주소를 참조하게 되고 하나의 객체를 가리키는 형태이다.**

![image](https://user-images.githubusercontent.com/98815511/161744226-961a32d3-8da4-46f8-a2e4-996381537d47.png)

</details>



**7. What's the output?**

```jsx
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);
console.log(a === b);
console.log(b === c);
```

- A: `true` `false` `true`
- B: `false` `false` `true`
- C: `true` `false` `false`
- D: `false` `true` `true`
- 
<details markdown="1">
<summary>Answer</summary>
C

이 문제는 **일치연산자(===)와 동등연산자(==)의 개념을 이해해야 한다.** 동등연산자(==)의 경우 비교하는 두 피연산자의 값만 비교하고 타입을 비교하진 않는다. 즉 3==”3” 은 true가 나오는 형태로 암묵적으로 같은 타입으로 만들어 비교하게 된다. 

일치연사자(===)의 경우는 값과 타입을 모두 비교하여 타입도 같아야만 true로 인정하게 된다.

//참조

//NaN === NaN  > false 

//Nan은 자신과 일치하지 않는 유일한 값이다. 따라서 숫자값이 NaN인지 조사하려면

//isNaN(NaN) 빌트인함수 isNaN을 사용하거나 object.is(NaN,NaN) 메서드를 사용하는 것이 좋다.

//object.is(+0,-0), object.is(NaN,NaN)

</details>



**8. What's the output?**

```
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = 'green' } = {}) {
    this.newColor = newColor;
  }
}

//{ newColor = 'green' } = {} 이렇게하면 기본값이 들어있는 형태가됨
//그냥 인수에 기본값 넣어준것을 객체로 표현했다고 보면 된다. ex) a=3 이런식으로 인수준 느낌
const freddie = new Chameleon({ newColor: 'purple' });
console.log(freddie.colorChange('orange'));
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details markdown="1">
<summary>Answer</summary>
D


이번 문제는 **클래스 내 정적메소드인 static의 의미를 알아야 한다.** 자바스크립트의 클래스 문법에서 클래스 블록안에서 static으로 정의된 함수는 정적메소드로 입력된다. 즉 클래스 Chameleon이라는 생성자 함수(클래스는 곧 생성자 함수와 같다)자체의 메소드이다. static을 붙이지 않고 메소드 축약표현으로 작성되는 함수는 Chameleon클래스의 프로토타입 메소드가 되고 constructor 함수는 생성자로서 생성하게 되는 인스턴스의 프로퍼티가 입력된다.

정적메소드는 클래스 자체의 프로퍼티 메소드이기 때문에 인스턴스는 상속받아 사용할 수 없다. 

인스턴스가 상속받는 메소드 및 프로퍼티는 오직 프로토타입 체인에 연결이 된 것만 가능하다.

정적메소드를 사용하기 위해서는 인스턴스를 생성하지 않고 바로 Chameleon.colorChange() 하면된다.

- 기본 매개변수 참조:[https://starkying.tistory.com/entry/Javascript-기본-매개변수Default-Function-Parameters](https://starkying.tistory.com/entry/Javascript-%EA%B8%B0%EB%B3%B8-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98Default-Function-Parameters)

//위에 constructor에 사용된 인수는 기본 매개변수의 새로운 방법으로 배열도 저런식으로 사용 가능하며 위에 문제의 경우 인수를 객체 형태로 주어야 한다. { newColor: 'purple' } 하지만 ={}이 사용된다면 그냥 () 인수없이도 호출할 수 있다.(={}없는데 인수없이 호출하면 에러가 뜨게된다.)
</details>



**9. What's the output?**

```
let greeting;
greetign = {}; // Typo!
console.log(greetign);
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`


<details markdown="1">
<summary>Answer</summary>
A


자세히보면 let으로 선언한 변수와 값을 할당한 변수가 다르다. 즉 greetign으로 잘못 입력한 변수는

let을 통해서 선언된 변수가 아니라 자바스크립트가 **암묵적으로 전역에 생성한 변수**가 된다.

이런걸 **암묵적 전역**이라고 한다. 

즉 변수 window.greetign의 형태가 된다.

</details>


**10. What happens when we do this?**

```
function bark() {
  console.log('Woof!');
}

bark.animal = 'dog';
```

- A: Nothing, this is totally fine!
- B: `SyntaxError`. You cannot add properties to a function this way.
- C: `"Woof"` gets logged.
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>
A


**자바스크립트의 함수는 특별한 형식의 객체이다!** 즉 프로퍼티를 가질수 있다는 말이다.

함수 내용의 추가가 아니라 함수 객체로서의 프로퍼티로 animal:”dog”가 추가된다.

console.log(bark)

결과: `{ [Function: bark] animal: 'dog' }`
</details>


**11. What's the output?**

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person('Lydia', 'Hallie');
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`



<details markdown="1">
<summary>Answer</summary>
A

**이 문제는 정적메소드와 프로토타입 메소드**의 개념을 알아야한다. 위에 문제에서 Person.getFullname은 함수 자체 메소드로 getFullname을 등록한 것이다(정적메소드). 즉 프로토타입 체인이 아니라 그냥 함수에 프로퍼티를 추가한게 된다. 따라서 생성된 인스턴스로 접근하는 것이 아니라 함수자체로 호출해야 나온다. 만약 생성된 인스턴스 객체로 호출하고 싶다면 getFullname함수를 person.prototype.getFullname으로 호출하면 가능하다. 
</details>


**12. What's the output?**

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person('Lydia', 'Hallie');
const sarah = Person('Sarah', 'Smith');

console.log(lydia);
console.log(sarah);
```

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` and `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` and `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` and `{}`
- D: `Person {firstName: "Lydia", lastName: "Hallie"}` and `ReferenceError`

<details markdown="1">
<summary>Answer</summary>
A

이번 문제는 **생성자 함수를 new연산자로 호출했을때와 일반 함수처럼 호출 했을 때**의 차이를 물어보고 있다. 생성자 함수는 특별할 것이 없다. 단지 생성되는 인스턴스와 바인딩되는 this를 식별자(미래의 프로퍼티 키) 앞에 붙여준 것 뿐이다. new 연산자와 함께 호출한다면 새로운 인스턴스 객체를 생성하고 인자로 입력해준  'Lydia', 'Hallie'가 각각 lydia객체의 프로퍼티 값으로 할당이 된다. 하지만 일반함수처럼 호출했을 때는 그냥 평범한 함수처럼 동작하게 된다. 즉 this.firstName, this.lastName
객체 window에 프로퍼티로 등록이 되게 된다. 결국 함수 결과값(return)은 없으므로 undefined가 암묵적으로 반환된다.


</details>




