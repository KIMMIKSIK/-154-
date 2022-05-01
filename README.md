# -JavaScript Questions 154-

@URL :[GitHub - lydiahallie/javascript-questions: A long list of (advanced) JavaScript questions, and their explanations](https://github.com/lydiahallie/javascript-questions)


자바스크립트의 중요 개념을 이해하는 문제 154개를 풀어보고 있습니다.


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

- A: `Lydia` and `undefined`
- B: `Lydia` and `ReferenceError`
- C: `ReferenceError` and `21`
- D: `undefined` and `ReferenceError`


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

- A: `0 1 2` and `0 1 2`
- B: `0 1 2` and `3 3 3`
- C: `3 3 3` and `0 1 2`



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

  

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`



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

- A: `1` and `false`
- B: `false` and `NaN`
- C: `false` and `false`


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

- A: `mouse.bird.size` is not valid
- B: `mouse[bird.size]` is not valid
- C: `mouse[bird["size"]]` is not valid
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

- A: `Hello`
- B: `Hey!`
- C: `undefined`
- D: `ReferenceError`
- E: `TypeError`

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

- A: `true` `false` `true`
- B: `false` `false` `true`
- C: `true` `false` `false`
- D: `false` `true` `true`
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

//{ newColor = 'green' } = {} 에서 = {} 이렇게 추가하면 인수로 아무것도 안넘겨주어도 된다는 의미이다.
//{ newColor = 'green' }의 경우 그냥 매개변수에 기본값(초기값)을 넣어준 것이며 그것을 객체로 표현했다고 보면 된다. ex)constructor(a=3) 이런식으로 인수 준 느낌
const freddie = new Chameleon({ newColor: 'purple' });
console.log(freddie.colorChange('orange'));
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

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

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`


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
- B: `SyntaxError`. You cannot add properties to a function this way.
- C: `"Woof"` gets logged.
- D: `ReferenceError`

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

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`



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

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` and `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` and `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` and `{}`
- D: `Person {firstName: "Lydia", lastName: "Hallie"}` and `ReferenceError`

<details markdown="1">
<summary>Answer</summary>
A

이번 문제는 **생성자 함수를 new연산자로 호출했을때와 일반 함수처럼 호출 했을 때**의 차이를 물어보고 있다. 생성자 함수는 특별할 것이 없다. 단지 생성되는 인스턴스와 바인딩되는 this를 식별자(미래의 프로퍼티 키) 앞에 붙여준 것 뿐이다. new 연산자와 함께 호출한다면 새로운 인스턴스 객체를 생성하고 인자로 입력해준  'Lydia', 'Hallie'가 각각 lydia객체의 프로퍼티 값으로 할당이 된다. 하지만 일반함수처럼 호출했을 때는 그냥 평범한 함수처럼 동작하게 된다. 즉 this.firstName, this.lastName
객체 window에 프로퍼티로 등록이 되게 된다. 결국 함수 결과값(return)은 없으므로 undefined가 암묵적으로 반환된다.


</details>


### **13. What are the three phases of event propagation?(이벤트 개념정리 문제)**

- A: Target > Capturing > Bubbling
- B: Bubbling > Target > Capturing
- C: Target > Bubbling > Capturing
- D: Capturing > Target > Bubbling


<details markdown="1">
<summary>Answer</summary>
D

표준 [DOM 이벤트](http://www.w3.org/TR/DOM-Level-3-Events/)에서 정의한 이벤트 흐름엔 3가지 단계가 있습니다.

1. 캡처링 단계 – 이벤트가 하위 요소로 전파되는 단계
2. 타깃 단계 – 이벤트가 실제 타깃 요소에 전달되는 단계
3. 버블링 단계 – 이벤트가 상위 요소로 전파되는 단계

## **Bubbling**:

한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작합니다. 가장 최상단의 조상 요소를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작합니다. 마치 거품이 올라가는 것 같다 해서 버블링이 라고 부른다.
대부분의 이벤트는 거의 기본으로 버블링 되지만 focus이벤트와 같이 안되는 이벤트도 있습니다.

한마디로 정리-**하위에서 상위 요소로의 이벤트 전파방식을 이벤트 버블링이라 한다.**

ex)

<form onclick="alert('form')">FORM     //form창 1개가 나옴
<div onclick="alert('div')">DIV               //div나오고 form창 2개가 나옴
<p onclick="alert('p')">P</p>                //p나오고 div나오고 form창 3개가 나옴
</div>
</form>

![image](https://user-images.githubusercontent.com/98815511/162578949-d28f155f-76e6-4751-82ba-8ec603d093d4.png)

@ 만약 bubbling을 중단하고 싶다면 중단을 원하는 요소의 onclick에 이벤트 객체의 메소드인 **stopPropagation()**메소드를 사용한다.

<body onclick="alert(`버블링은 여기까지 도달하지 못합니다.`)">
<button onclick="event.stopPropagation()">클릭해 주세요.</button>
</body>

@ 추가로 한 요소에 여러 핸들러가 있다면 **event.stopImmediatePropagation()**을 통해 가능합니다.이 메서드를 사용하면 해당 포함되있는 해당 함수만 작동하고 요소에 할당된 다른 이벤트의 처리는  모두 동작하지 않습니다.
**버블링도 막고 해당 요소의 다른 핸들러도 모두 멈추게 합니다.(본인을 포함한 함수는 작동함)
@** 2개 모두 Capturing에도 적용됨.

## **Capturing**:

이벤트 버블링과 반대 방향으로 진행되는 이벤트 전파 방식

`on<event>` 프로퍼티나 HTML 속성, `addEventListener(event, handler)`를 이용해 할당된 핸들러는 캡처링에 대해 **전혀 알 수 없습니다.** 이 핸들러들은 타깃 단계와 버블링 단계만 동작합니다.
만약 캡처링 단계에서 이벤트를 잡아내려면 `addEventListener`의 `capture` 옵션을 `true`로 설정해야 합니다.

```
div.addEventListener("click", logEvent, {
    capture: true // default 값은 false입니다.
  });
```

이렇게 addEventListener인수로 true 혹은 capture:true를 입력해준다.

‣

- `event.target` – 이벤트가 발생한 가장 안쪽의 요소(정확히 어디서 이벤트가 발생했는지)
- `event.currentTarget` (=`this`) – 이벤트를 핸들링 하는 현재 요소 (핸들러가 실제 할당된 요소)
- `event.eventPhase` – 현재 이벤트 흐름 단계(캡처링=1, 타깃=2, 버블링=3)

## **@ event.currentTarget vs event.target**

만약 부모에만 이벤트가 있고 자식에게 없는 상황일때,

**currentTarget은 이벤트 핸들러가 부착된 부모를** 가리킨다!

즉, target은 부모로부터 이벤트가 위임되어 발생하는 자식의 위치, 내가 클릭한 자식 요소를 반환한다. 하지만 currentTarget은 이벤트가 부착된 부모의 위치를 반환한다.


![image](https://user-images.githubusercontent.com/98815511/162578929-3ed9da2e-8097-47fa-8475-072cd48394e9.png)

event.target은 자식 요소인 span을 리턴하고, event.currentTarget은 부모 요소인 button을 반환하게 된다.

전체내용 참조: [https://ko.javascript.info/bubbling-and-capturing](https://ko.javascript.info/bubbling-and-capturing)

</details>




**14. All object have prototypes.**

- A: true
- B: false


<details markdown="1">
<summary>Answer</summary>
B


**자바스크립트의 prototype 객체는 정확히는 함수의 프로퍼티 입니다.**  더 정확히 한다면 caller와 constructor내부슬롯을 가지고 생성되는 함수들에게 암묵적으로 생성되는 프로퍼티 입니다.
****모든 객체들은 __**proto**__ 접근자 프로퍼티를 가지고 있고 그것을 통해 자신의 상위 객체의 프로토타입 객체에 접근할 수 있습니다. 이것을 프로토타입 체인이라고 하며 객체의 최상위에는 결국 object.prototype객체가 존재하고 있습니다.

@ **prototype을 가지고 있지 않은 함수에는 메소드 축약표현과 화살표 함수가 있습니다.**


</details>



**15. What's the output?**

```
function sum(a, b) {
  return a + b;
}

sum(1, '2');
```

- A: `NaN`
- B: `TypeError`
- C: `"12"`
- D: `3`

<details markdown="1">
<summary>Answer</summary>
C


**자바스크립트는 동적타입의 언어입니다.** 즉 변수의 타입을 지정하고 할당하는 것이 아닌 할당되는 데이터 값에 의해 타입이 정해집니다. 위와같은 경우는 계산을 위해서 자바스크립트 자체적으로 1의 숫자타입을 문자열로 변환하게 되는데 이러한 것을 암묵적 변환이라고 부릅니다.

</details>



**16. What's the output?**

```
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

- A: `1` `1` `2`
- B: `1` `2` `2`
- C: `0` `2` `2`
- D: `0` `1` `2`

<details markdown="1">
<summary>Answer</summary>

C


이 문제는 **후치증가 연산자와 전치증가 연산자**의 개념을 물어보고 있습니다. 후치의 ++의 경우 출력후 연산이 이루어지게 되고 전치의 경우 연산 후 값을 출력하게 되어 답은 0 2 2가 됩니다.

</details>



**17. What's the output?**

```
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = 'Lydia';
const age = 21;

getPersonInfo`${person} is ${age} years old`;
```

- A: `"Lydia"` `21` `["", " is ", " years old"]`
- B: `["", " is ", " years old"]` `"Lydia"` `21`
- C: `"Lydia"` `["", " is ", " years old"]` `21`

<details markdown="1">
<summary>Answer</summary>

B


자바스크립트의 **함수호출 방법에는 백틱을 사용한 (Tagged Template Literal)방법이 존재한다.**

첫번째 인자는 표현식을 제외한 스트링값의 배열, 두번째 인자는 표현식들이 쓰인만큼 나열
즉 아래 예시의 두 함수호출은 같은 결과를 반환한다.

ex) 

getPersonInfo``${person}is ${age} years old``;
getPersonInfo(["", " is ", " years old"], name, age)

같다!

</details>



**18. What's the output?**

```
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log('You are an adult!');
  } else if (data == { age: 18 }) {
    console.log('You are still an adult.');
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });
```

- A: `You are an adult!`
- B: `You are still an adult.`
- C: `Hmm.. You don't have an age I guess`

<details markdown="1">
<summary>Answer</summary>

C


**이 문제는 원시타입과 객체 타입의 값을 참조하고 있는 형태를 알아야 이해할 수 있다.**

원시타입의 경우 바로 값이 들어있는 주소를 참조하고 있지만 객체타입의 경우는 객체 값이 들어있는 주소의 주소를 한단계 더 걸쳐 참조하고 있기 때문에 결국 객체 == 객체 혹은 객체 ===객체는 바로 값을 비교하는게 아니라 참조값(주소)를 비교하고 있다는 말이된다!

</details>


**19. What's the output?**

```
function getAge(...args) {
  console.log(typeof args);
}

getAge(21);
```

- A: `"number"`
- B: `"array"`
- C: `"object"`
- D: `"NaN"`

<details markdown="1">
<summary>Answer</summary>

C


**...args와 같은 것을 스프레드 연산자**라고 한다. 위와 같은 경우 매개변수로 ...args를 지정하면 인수로 들어가는 모든 값을 args라는 식별자의 배열로 저장하게 된다. 배열의 데이터 타입은 객체이므로 답은 object이다.

</details>


**20. What's the output?**

```
function getAge() {
  'use strict';
  age = 21;
  console.log(age);
}

getAge();
```

- A: `21`
- B: `undefined`
- C: `ReferenceError`
- D: `TypeError`

<details markdown="1">
<summary>Answer</summary>

C


이 문제는 **자바스크립트의 strict mode**의 개념을 알아야 한다. 오타나 문법의 지식의 미비로 인한 실수는 언제든 발생할 수 있다. 이 모드는 오류를 줄여 안정적인 코드를 생산하기 위한 해결책이라고 할 수 있다. 예를 들어 아래와 같은 경우가 있다.

function foo(){

x=10;

}

foo();

console.log(x);  // 자바스크립트는 이때 10이라고 하는 결과를 도출한다.
이유는 함수안에 x식별자가 없어 전역인 상위스코프를 확인하게 되고 전역에도 존재하지 않는 x는 자바스크립트가 암묵적으로 전역객체의 프로퍼티로 생성하기 때문이다. 따라서 마치 전역에 선언한 변수처럼 접근이 가능하게 된다. 이는 의도치 않은 결과이므로 이러한 오류들을 줄이기 위해서 strict mode를 사용한다. strict mode는 전역의 선두 또는 함수몸체의 선두에 ‘use strict’를 추가하여 사용한다. ESLint같은 도구를 사용해도 이와 유사하며 더 나은 효과를 얻을 수 있다는 것을 참조하자.

</details>



**21. What's the value of `sum`?**

`const sum = eval('10*10+5');`

- A: `105`
- B: `"105"`
- C: `TypeError`
- D: `"10*10+5"`

<details markdown="1">
<summary>Answer</summary>

A


eval함수는 문자열을 인자로 받아서 실행하는 함수이다. 다만 만약 표현식이 사용되었다면 표현식을 평가한 결과를 반환하게 되는데 그래서 답은 숫자 105가 된다.

</details>



### **22. How long is cool_secret accessible?**

`sessionStorage.setItem('cool_secret', 123);`

- A: Forever, the data doesn't get lost.
- B: When the user closes the tab.
- C: When the user closes the entire browser, not only the tab.
- D: When the user shuts off their computer.

<details markdown="1">
<summary>Answer</summary>

B


**이 문제는 웹 스토리지 객체 localStorage와 sessionStorage를 알아야한다.**
이 객체는 브라우저 내에 키-값 쌍을 저장할 수 있게 도와주는데 session의 경우 페이지 새로고침, 즉 탭을 제거했다가 다시 오픈한 경우에도 데이터를 기억하고 local의 경우 session보다 더 강력하게 브라우저를 껐다가 다시켜도 즉 브라우저나 OS가 재시작해도 데이터가 사라지지 않고 남아있는다. session의 경우 제한적이기에 많이 쓰이진 않으며 같은 페이지라도 탭이 다르면 다른곳에는 저장되지 않는다. 반면 local은 origin이 같다면 모든 탭과 창에서 공유된다.

- 쿠기와는 다른 개념이며 간단히는 네트워크 요청 시 서버로 전달되지 않으며 이런특징으로 쿠키보다 더 많은 자료를 보관할 수 있게된다.웹스토리지 객체 조작은 모두 자바스크립트 내에서 수행된다.
- 참조: [https://ko.javascript.info/localstorage](https://ko.javascript.info/localstorage)

두 스토리지 객체는 동일한 메서드와 프로퍼티를 제공한다.

- `setItem(key, value)` – 키-값 쌍을 보관합니다.
- `getItem(key)` – 키에 해당하는 값을 받아옵니다.
- `removeItem(key)` – 키와 해당 값을 삭제합니다.
- `clear()` – 모든 것을 삭제합니다.
- `key(index)` – 인덱스(`index`)에 해당하는 키를 받아옵니다.
- `length` – 저장된 항목의 개수를 얻습니다.

</details>



### **23. What's the output?**

`var num = 8;
var num = 10;

console.log(num);`

- A: `8`
- B: `10`
- C: `SyntaxError`
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>

B


var 키워드로 변수를 생성한다면 크게 3가지 문제를 겪을 수 있다. 첫쨰는 **중복 선언이 가능**하여 전에 선언된 변수가 있다면 덮어쓰게 된다는 것이고 둘째는 오직 **함수레벨스코프**만 따른다는 것이다. 셋쨰는 호이스팅을 통해 **변수의 선언과 초기화가 동시**에 이루어 지는 것인데 이 문제의 경우 첫번째 이유로 인해 답은 B가 된다.


</details>



### **24. What's the output?**

`const obj = { 1: 'a', 2: 'b', 3: 'c' };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty('1');
obj.hasOwnProperty(1);
set.has('1');
set.has(1);`

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`


<details markdown="1">
<summary>Answer</summary>


C

ES6의 새로운 자료구조에는 Map과 Set이 존재한다. 둘다 생성자 함수로서 객체를 생성하지만 Map의 경우 키가 있는 데이터를 저장한다는 점에서 객체와 유사하고 Set은 키가 없는 값을 저장한다는 점에서 배열과 유사하다. 둘다 순서를 기억하는 이터러블 객체를 생성하여 반복문을 사용할 수 있다. 특히 이번 문제의 Set의 경우 중복을 허용하지 않는 객체를 생성한다.(파이썬의 집합)

 일반적인 객체의 키 값은 직접 문자열로 입력하지 않아도 내부적으로 문자열입니다. 따라서 “1”도 가능하며 1도 가능합니다. 하지만 set의 경우 인수로 준 이터러블 객체의  요소를 그대로 가져오므로 객체 set에 들어있는 1은 문자열 “1”이 아니다. 

참조 : [https://mine-it-record.tistory.com/473](https://mine-it-record.tistory.com/473)

</details>


### **25. What's the output?**

`const obj = { a: 'one', b: 'two', a: 'three' };
console.log(obj);`

- A: `{ a: "one", b: "two" }`
- B: `{ b: "two", a: "three" }`
- C: `{ a: "three", b: "two" }`
- D: `SyntaxError`

<details markdown="1">
<summary>Answer</summary>

C


만약 객체에 같은 키 이름으로 두개의 값이 들어가게 된다면 처음에 입력된 순서에 있지만 값은 최신것으로 바뀌게 된다. 재할당의 느낌

</details>



### **26. The JavaScript global execution context creates two things for you: the global object, and the "this" keyword.**

- A: true
- B: false
- C: it depends

<details markdown="1">
<summary>Answer</summary>

A

자바스크립트의 전역 실행컨텍스트는 전역객체와 this키워드를 생성한다는 것이 문제의 질문인데 사실 내 개인적인 생각은 false인 것 같다. 왜냐하면 전역객체는 사실 전역컨텍스트에 의해 생성되는 것이 아닌 가장 프로그램 실행시 가장 먼저 실행되게 되고 전역컨텍스트가 이 전역객체를 참조하게 되는 것이기 때문이다. this키워드를 생성하여 바인딩하는 것은 맞다.

</details>



### **27. What's the output?**

```jsx
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

- A: `1` `2`
- B: `1` `2` `3`
- C: `1` `2` `4`
- D: `1` `3` `4`

<details markdown="1">
<summary>Answer</summary>

C


continue키워드는 반복문에서 내용을 건너뛰고 다음 반복을 진행하라는 의미이다. 따라서 위와 같은 경우 i가 3이 되었을 때는 console.log(i)를 거치지 않고 다음으로 넘어가게 된다.

</details>




### **28. What's the output?**

```jsx
String.prototype.giveLydiaPizza = () => {
  return 'Just give Lydia pizza already!';
};

const name = 'Lydia';

name.giveLydiaPizza();
```

- A: `"Just give Lydia pizza already!"`
- B: `TypeError: not a function`
- C: `SyntaxError`
- D: `undefined`

<details markdown="1">
<summary>Answer</summary>

A

String은 자바스크립트의 빌트인 객체로서 생성자 함수이다. 이 문제에서는 **래퍼객체의 개념과 프로토타입의 개념**을 물어보고 있다. 자바스크립트는 원시타입의 name 식별자를 객체처럼 사용할 때 문자열의 타입을 암묵적으로 객체로 변환시킨다.

(new String("hello")을 호출한 것처럼 string 자료형을 래퍼 객체로 임시로 변환하기 때문입니다.) 

이렇게 객체형태로 원시타입의 값을 감싸게 된 것을 래퍼객체라고 하며 이때 문자열은 프로토타입 체인을 따라 자신이 상속받는 프로퍼티나 메소드를 사용할 수 있게 된다.

- 원시타입에 대응하는 4개의 래퍼객체: String, Number, Boolean, Symbol

</details>



### **29. What's the output?**

```jsx
const a = {};
const b = { key: 'b' };
const c = { key: 'c' };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

- A: `123`
- B: `456`
- C: `undefined`
- D: `ReferenceError`

<details markdown="1">
<summary>Answer</summary>

B

객체의 키값으로 등록되는 것들은 모두 암묵적으로 문자열 화가 된다. 만약 그것이 객체라면 “[object Object]”형태의 문자열이 되는데 이때 B,C모두 같은 문자열 키 “[object Object]”의 형태이기 때문에 객체 c를 키로서 전해줄때 프로퍼티키 “[object Object]”의 값이 123에서 456으로 재할당되는 것이다.
즉 b,c 둘다 “[object Object]”를 의미하며 결국 a[b]는  a[“[object Object]”]를 의미하는 것이다.


</details>


### **30. What's the output?**

```jsx
const foo = () => console.log('First');
const bar = () => setTimeout(() => console.log('Second'));
const baz = () => console.log('Third');

bar();
foo();
baz();
```

- A: `First` `Second` `Third`
- B: `First` `Third` `Second`
- C: `Second` `First` `Third`
- D: `Second` `Third` `First`


<details markdown="1">
<summary>Answer</summary>

B


문제는 비동기 작동의 메커니즘을 물어보고 있다. setTimeout은 인수로 전달된 콜백학수를 지연시간후에 작동시키는 함수이다. 설령 위처럼 지연시간이 정해져 있지 않아도 setTimeout()을 호출함과 동시에 일단 bar함수는 stack에서 pop되기 때문에 다른 함수들이 먼저 실행되고 마지막에 ‘second’를 호출하는 콜백함수를 작동시킨다. 아래 그림을 참조하며 실제로 delay로 주어지는 시간은 정확히 말하면 wep api인 setTimeout()이 테스크큐로 함수를 인풋하기까지 걸리는 delay를 말한다.  출력되기 까지의 시간이 아니라는 것이다. 큐에 등록된 함수는 이벤트루프를 통해서 stack이 비게 된다면 그때 stack으로 push되어 비로소 호출된다.

</details>



### **31. What is the event.target when clicking the button?**

```jsx
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

- A: Outer `div`
- B: Inner `div`
- C: `button`
- D: An array of all nested elements.

<details markdown="1">
<summary>Answer</summary>

C

 event.target은 클릭한 대상 그자체를 의미하며 이 문제에서는 button을 출력하는 button태그를 말한다.
 
</details>




### **32. When you click the paragraph, what's the logged output?**

```jsx
<div onclick="console.log('div')">
  <p onclick="console.log('p')">
    Click here!
  </p>
</div>
```

- A: `p` `div`
- B: `div` `p`
- C: `p`
- D: `div`

<details markdown="1">
<summary>Answer</summary>

A

이벤트는 크게 캡쳐링 타겟 버블링으로 나누어진다. 그 중 addEventlistener를 통해 3번째 인수를 true로 전달하는 경우가 아니라면 기본적으로 캡쳐링은 일어나지 않는다. 즉 클릭한 타겟과 버블링 현상이 나타나고 그로인해 p div가 출력된다.

</details>


### **33. What's the output?**

```jsx
const person = { name: 'Lydia' };

function sayHi(age) {
  return `${this.name} is ${age}`;
}

console.log(sayHi.call(person, 21));
console.log(sayHi.bind(person, 21));
```

- A: `undefined is 21` `Lydia is 21`
- B: `function` `function`
- C: `Lydia is 21` `Lydia is 21`
- D: `Lydia is 21` `function`

<details markdown="1">
<summary>Answer</summary>

D


bind,call,apply는 모두 함수에 this를 고정시켜줄때 사용할수 있는 메소드들이다. bind는 인자로 (this가 될 객체, ...함수에실제인수로 들어갈 표현식)으로 사용할 수 있으며 call, apply와 다르게 함수 실행 결과가 아니라 binding이 끝난 bound 함수를 다시 리턴한다. call과 apply는 첫번째 인자로 this바인딩을 위한 객체를 넣고 두번째부터는 함수에 실제 인수로 들어갈 표현식이 들어간다. 결국 binding과 똑같다. 하지만 call과 apply의 경우 함수를 실행한 값을 리턴한다. 또한 say.apply의 두번째 인수는 모두 배열로 넣어줘야한다는 차이가 있다.(call과 apply의 차이)

- say.apply(obj, [”seoul”])
- say.call(obj, "seoul")
- student1.print.apply(this, ['male', 'A']); - 실행결과 반환
- var print = student1.print.bind(this, 'female','O'); - binding시킨 student1.print함수 반환


</details>


### **34. What's the output?**

```jsx
function sayHi() {
  return (() => 0)();
}

console.log(typeof sayHi());
```

- A: `"object"`
- B: `"number"`
- C: `"function"`
- D: `"undefined"`

<details markdown="1">
<summary>Answer</summary>

B

이 문제는 sayHi()를 통해 함수의 리턴값을 물어보고 있다. 함수의 리턴 값은 화살표함수의 즉시 실행함수로 이루어져있고 그 결과인 0이라는 숫자값이 나오게 되는데 결국 typeof가 가리키는 대상은 숫자형 0이다.

</details>


