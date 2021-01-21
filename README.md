![](coverPage.png)

# 123-JavaScript-Interview-Questions

이 책은 꼼꼼하게 정리한 문제집을 통해 자바스크립트 프론트엔드 개발자들이 기술직 면접을 준비할 수 있도록 돕는 것이 목표입니다.

## 종이책을 구입하고 싶나요? 플래시카드를 원하시나요?

 - 책은 곧 구입이 가능해 질 것 입니다. 만약 이 책의 초기 사본을 원하신다면, [Google Form](https://goo.gl/forms/c8ubV1tWBBdz6fJP2)에 이름과 이메일주소를 등록해 주세요.
 - 기다리는 것이 싫다면, 아름다운 포커사이즈 플래시카드에 인쇄된  [Yuri's JavaScript Flashcards](http://flashcardsjs.com)에서 구입 가능합니다.

## Question 1. 자바스크립트에서 `undefined` 와 `not defined`의 차이

<details><summary><b>정답</b></summary>


만약 당신이 자바스크립트에서 선언되지 않거나 존재하지 않는 변수를 사용하려고 한다면, 자바스크립트는 `var name is not defined` 라는 에러를 던지고 실행을 중지할 것입니다. 그러나 만약 당신이 `typeof undeclared_variable` 를 입력한다면 그것은 `undefined`를 리턴할 것입니다.

더 많은 논의를 시작하기 전에 선언(declaration)과 정의(definition)의 차이를 이해해보도록 하겠습니다.

`var x` 는 선언입니다. 우리는 아직 변수를 정의하지 않았습니다. 하지만 우리는 그것의 존재와 메모리 할당의 필요성을 선언하고 있습니다.

```javascript
var x; // x를 선언
console.log(x); // 출력: undefined
```

`var x = 1` 는 선언과 정의를 동시에 하고 있습니다. 여기서 값의 선언 및 할당은 변수 x에 대해 인라인(?)에서 발생합니다. 우리는 이것을 초기화(initialization)라고 부릅니다. 자바스크립트에서 변수선언과 함수 선언 모두 선언된 범위의 최상단으로 이동한 다음 할당이 발생합니다. 우리는 이것을 보고 호이스팅(hoisting)이라고 부릅니다.

`undefined`는 변수가 선언될 수는 있으나 정의될 수는 없는 경우일때 발생합니다. 우리가 이것에 대해 접근하려고 할 때, 이것은 `undefined`를 내뱉습니다.

```javascript
var x; // 선언
typeof x === 'undefined'; // true가 리턴됩니다.
```

`not defined`는 변수가  선언될 수도 없고 정의될 수도 없는 경우입니다. 우리가 그러한 변수를 참조하려고할 때 결과는 `not defined` 입니다.

```javascript
console.log(y);  // 출력: ReferenceError: y is not defined
```

### 관련 링크:

[http://stackoverflow.com/questions/20822022/javascript-variable-definition-declaration](http://stackoverflow.com/questions/20822022/javascript-variable-definition-declaration)

</details>

## Question 2. 다음 두 조건식의 결과가 일치하지 않는  `x` 값은 어느 것인가요?


```javascript
if( x <= 100 ) {...}
if( !(x > 100) ) {...}
```
<details><summary><b>Answer</b></summary>
`NaN <= 100` 는 `false` 이고 `NaN > 100` 또한 `false` 입니다. 그러므로, 만약  `x` 가 `NaN` 값을 가지면, 두개의 조건식의 결과는 일치하지 않게 됩니다. 

이러한 비교는 숫자(Number) 유형으로 변환되는 모든 x의 값에 대해서도 동일하게 적용되며  `NaN`을 반환합니다. 또한  `undefined`, `[1,2,5]`, `{a:22}`  등의 값도 같은 결과를 보여줍니다. 

 그러므로 우리는 숫자(numeric) 변수를 다룰 때 조심해야 합니다. 왜냐하면,  `NaN` 은 어떠한 숫자(numeric) 값과 같거나 혹은 작거나, 클 수 없기 때문입니다. 

변수 값이 `NaN` 인지 확인하는 유일한 방법은  `isNaN()` 함수를 쓰는 것임을 명심하세요. 

</details>

## Question 3. 자바스크립트 오브젝트를 직접적으로 선언하는것의 단점은 무엇일까요?

<details><summary><b>정답</b></summary>


자바스크립트 오브젝트를 직접적으로 선언하는 것의 단점중 하나는 그것들이 매우 메모리가 비효율적이라는 것입니다. 이렇게 하면 오브젝트의 각 인스턴스에 대해 메서드의 새 복사본이 생성됩니다. 다음은 예시입니다.

```javascript
var Employee = function (name, company, salary) {
  this.name = name || "";       
  this.company = company || "";
  this.salary = salary || 5000;

  // 우리는 이런식으로 메소드를 만들 수 있습니다:
  this.formatSalary = function () {
      return "$ " + this.salary;
  };
};

// 대안으로, Employee의 프로토타입에 메소드를 추가할 수 있습니다:
Employee.prototype.formatSalary2 = function() {
    return "$ " + this.salary;
}

// 객체 생성
var emp1 = new Employee('Yuri Garagin', 'Company 1', 1000000);
var emp2 = new Employee('Dinesh Gupta', 'Company 2', 1039999);
var emp3 = new Employee('Erich Fromm', 'Company 3', 1299483);
```

이 경우에 각각의 변수 `emp1`, `emp2`, `emp3` 는 각각`formatSalary` 메소드를 가집니다. 그러나 `formatSalary2` 는 `Employee.prototype`에 한번만 추가될 것입니다.

</details>

## Question 4. Javascript 에서 “closure” 는 무엇인가요? 예시를 들어줄 수 있나요 ? 

<details><summary><b>정답</b></summary>
클로저는 다른 함수(부모 함수라고 부릅니다.) 안에 정의된 함수입니다. 그래서 그 부모 함수의 scope안에 선언되고 정의된 변수에 접근할 수 있습니다.




Closure는 세가지의 scope 상황에서 변수에 접근 권한을 가집니다. 

- 그것의 scope 안에 선언된 변수
- 그것의 부모의 함수 scope 안에 선언된 변수
- 전역 namespace에 선언된 변수 

```javascript
var globalVar = "abc"; // 전역 변수

// 부모의 자체 호출 함수
(function outerFunction (outerArg) { // 외부 함수의 scope 시작. 

  var outerFuncVar = 'x'; // 외부 함수 scope에 선언된 변수   
  
  // Closure 자체 호출 함수 
  (function innerFunction (innerArg) { // 내부 함수의 scope 시작.

    var innerFuncVar = "y"; // 내부 함수의 scope에 선언된 변수 
    console.log(         
      "outerArg = " + outerArg + "\n" +
      "outerFuncVar = " + outerFuncVar + "\n" +
      "innerArg = " + innerArg + "\n" +
      "innerFuncVar = " + innerFuncVar + "\n" +
      "globalVar = " + globalVar);
  	
  // 내부 함수의 scope 종료. 
  
  })(5); // Clousure에 숫자 5를 매개변수로 넘깁니다. 

// 외부 함수 scope 종료. 

})(7); // 부모 함수에 숫자 7을 매개변수로 넘깁니다.
```

`outerFunction` 안에 정의된 `innerFunction` (내부 함수)는 closure 입니다. 그래서 결과적으로 `outerFunction` scope 와 프로그램의 전역 변수 scope 안에서 선언되고 정의된 모든 변수에 접근이 가능합니다.



위 코드의 출력 결과는 다음과 같습니다:

```javascript
outerArg = 7
outerFuncVar = x
innerArg = 5
innerFuncVar = y
globalVar = abc
```

</details>

## Question 5. 다음 구문을 사용하여 호출할때 제대로 동작하는 곱셈 함수를 작성해주세요.

```javascript
console.log(mul(2)(3)(4)); // 출력 : 24
console.log(mul(4)(3)(4)); // 출력 : 48
```
<details><summary><b>정답</b></summary>


```javascript
function mul (x) {
  return function (y) { // 익명 함수
    return function (z) { // 익명 함수
      return x * y * z;
    };
  };
}
```

여기서 `mul`함수는 첫번째 인자를 받고, 두번째 인자를 취하는 익명함수를 반환하고, 마지막으로 세번째 인자를 취하는 익명함수를 반환하는 형태의 함수입니다.

이때 마지막 함수는 `x`, `y`, `z`를 곱하여 그 연산 결과를 반환합니다.

자바스크립트에서 다른함수 안에서 정의된 함수는 외부 함수의 스코프에 접근할 수 있으며 결과적으로 이것을 캡슐화하는 범위에 속하는 변수인 다른 함수를 반환하거나 리턴할 수 있습니다.

- 함수는 Object 타입의 인스턴스입니다.
- 함수는 속성을 가질 수 있으며 생성자 메서드에 대한 링크를 가질 수 있습니다.
- 함수는 변수로써 저장될 수 있습니다.
- 함수는 다른 함수의 파라미터로 전달될 수 있습니다.
- 함수는 다른 함수에 의해 리턴될 수 있습니다.

</details>

## Question 6. Javascript에서 배열을 비우는 방법은 무엇인가요 ? 
예시:

```javascript
var arrayList =  ['a', 'b', 'c', 'd', 'e', 'f'];
```

위에 보이는 배열을 어떻게 비울 수 있나요?

<details><summary><b>정답</b></summary>

우리가 배열을 비울 수 있는 몇가지 방법이 있습니다. 
그러므로, 가능한 모든 방법들에 대해서 다뤄보겠습니다.

#### 방법 1

```javascript
arrayList = [];
```

위의 코드는 `arrayList` 변수를 새로운 빈 배열로 설정할겁니다. 이러한 방법은 **원래 배열에 대한 참조**가 없을 경우에 추천됩니다. 왜냐하면, 이것은 실제로 새로운 빈 배열을 생성할 것이기 때문입니다. 만약 다른 변수에서 이 배열을 참조한다면 이 배열이 변해도 참조한 변수엔 예전 값이 변하지 않고 남아있습니다. 원래 변수`arrayList`로만 배열을 참조한 경우에 이 방법을 사용하세요. 

예시:

```javascript
var arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; // 배열을 만듧니다.
var anotherArrayList = arrayList;  // 다른 변수가 arrayList를 참조합니다.
arrayList = []; // arrayList 배열을 비웁니다.
console.log(anotherArrayList); // 출력 결과 ['a', 'b', 'c', 'd', 'e', 'f']
```

#### 방법 2

```javascript
arrayList.length = 0;
```

위의 코드는 배열의 길이를 0으로 설정함으로써 기존 배열을 비웁니다. 이렇게 배열을 비우는 방법은 또한 기존 배열을 가리키고 있는 모든 참조 변수를 업데이트 합니다. 

예시:

```javascript
var arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; // 배열을 만듧니다.
var anotherArrayList = arrayList;  // 다른 변수가 arrayList를 참조합니다. 
arrayList.length = 0; // length를 0으로 설정하여 배열을 비웁니다. 
console.log(anotherArrayList); // 출력 결과 []
```

#### 방법 3

```javascript
arrayList.splice(0, arrayList.length);
```

위의 구현은 또한 완벽하게 동작합니다. 이 방법 또한 기존 배열을 참조한 모든 변수들을 업데이트 합니다. 

```javascript
var arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; // 배열을 만듧니다.
var anotherArrayList = arrayList;  // 다른 변수가 arrayList를 참조합니다. 
arrayList.splice(0, arrayList.length); // length를 0으로 설정하여 배열을 비웁니다.
console.log(anotherArrayList); // 출력 결과 []
```

#### Method 4

```javascript
while(arrayList.length) {
  arrayList.pop();
}
```

위의 구현도 배열을 비울 수 있습니다. 그러나 자주 쓰도록 권장하지는 않습니다.


</details>

## Question 7. 객체가 배열인지 어떻게 확인하나요?

<details><summary><b>정답</b></summary>

객체가 특정 클래스의 인스턴스인지 또는 `Object.prototype`에서 `toString`메서드로 사용중인지를 확인할 수 있는 가장 좋은 방법

```javascript
var arrayList = [1 , 2, 3];
```

객체의 타입체크 중 가장 좋은 방법 중 하나는 자바스크립트에서 메서드 오버로딩을 수행하는 경우입니다. 이를 이해하기 위해서 하나의 문자열과 하나의 문자열 목록을 얻을 수 있는 `greet` 이라는 메서드가 있다고 가정해봅시다. `greet` 이라는 메서드가 단일 값을 넘겨주는지 또는 리스트값을 넘겨주는지, 그 두 개를 모두 실행할 수 있게 하기 위해서는 어떤 종류의 매개변수가 들어오는지를 확인해야 합니다.

```javascript
function greet(param) {
  if() {
    // param이 배열인지 아닌지를 체크해야함
  }
  else {
  }
}
```

그러나 위 구현에서는 배열의 유형을 확인할 필요가 없을 수 있는데, 우리가 단일 값 문자열을 확인하고 배열 코드를 다른 블록에 넣을 수 있기 때문입니다. 아래의 코드를 살펴보겠습니다.

```javascript
 function greet(param) {
   if(typeof param === 'string') {
   }
   else {
     // 만약 타입이 array라면 이 코드블록이 실행됩니다.
   }
 }
```

앞의 두 가지 실행은 괜찮은 방법일 수 있지만, `단일 값`, `배열`, `객체`일 경우에는 문제가 될 수 있습니다. 

다시 객체의 타입 체킹으로 돌아와서 우리는 `Object.prototype.toString`을 사용할 수 있다고 언급한 바 있습니다.

```javascript
if(Object.prototype.toString.call(arrayList) === '[object Array]') {
  console.log('Array!');
}
```

만약 당신이 `jQuery`를 사용한다면 당신은 jQuery의 `isArray` 메소드를 사용할 수 있습니다.

```javascript
if($.isArray(arrayList)) {
  console.log('Array');
} else {
  console.log('Not an array');
}
```

jQuery는 객체가 배열인지 아닌지를 확인할때 내부적으로 `Object.prototype.toString.call`을 사용합니다.

모던 브라우저에서는, 다음과 같이 사용할 수 있습니다.

```javascript
Array.isArray(arrayList);
```

`Array.isArray`는 Chrome5, Firefox 4.0, IE 9, Opera 10.5 그리고 Safari 5에서 지원 됩니다.


</details>

## Question 8. 다음 코드의 출력값은 무엇일까요 ? 

```javascript
var output = (function(x) {
  delete x;
  return x;
})(0);

console.log(output);
```
<details><summary><b>정답</b></summary>


위의 코드는 출력값으로 `0`을 출력할 것입니다. `delete` 연산자는 한 객체(object)의 한 속성(property)를 지우기 위해서 쓰입니다. 여기 `x` 는 객체가 아니고, **지역 변수** 입니다. 그래서 `delete` 연산자는 이 지역 변수에 아무런 영향을 주지 못합니다. 


</details>

## Question 9. 다음 코드의 결과는 무엇일까요?

```javascript
var x = 1;
var output = (function() {
  delete x;
  return x;
})();

console.log(output);
```
<details><summary><b>정답</b></summary>
위 코드는  1을 출력합니다.

`delete` 연산자는 객체의 프로퍼티를 삭제하는데 사용됩니다. `x`는 객체가 아니고 `숫자` 타입의 **전역변수** 입니다.

</details>

## Question 10. 다음 코드의 결과 값은 무엇이 될까요 ?

```javascript
var x = { foo : 1};
var output = (function() {
  delete x.foo;
  return x.foo;
})();

console.log(output);
```
<details><summary><b>정답</b></summary>


위 코드의 결과값은 `undefined` 입니다.`delete` 연산자는 객체의 속성을 삭제하는데 쓰입니다. 여기 `x` 는 foo를 속성으로 가지고 있는 객체이고,  자기 호출 함수 (self-invoking function)에서 `foo` 속성을 지우려고 합니다. 그리고 삭제 이후에, 우리는 그 삭제된 `foo` 속성을 참조하려고 하기 때문에 결과는  `undefined` 가 됩니다. 


</details>

## Question 11. 다음 코드의 결과는 무엇일까요?

```javascript
var Employee = {
  company: 'xyz'
}
var emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company);
```

<details><summary><b>정답</b></summary>

이 코드는 `xyz`를 출력합니다. `emp1` 객체는 객체 **프로토타입**으로써 company를 갖고있습니다. delete 연산자는 프로토타입 속성을 삭제하지 않습니다.

 `emp1` 객체는 **company** 속성을 자체 속성으로 가지고 있지않습니다. 우리는 이것을`console.log(emp1.hasOwnProperty('company')); //output : false` 로 테스트 해볼 수 있습니다. 그러나 우리는 `Employee` 객체에서 직접적으로  `delete Employee.company` 메소드를 사용하여 `company` 속성을 삭제할 수 있습니다. 또는 우리는 그것을 `__proto__` 속성인 `delete emp1.__proto__.company` 를 사용하여 `emp1`객체로 부터 삭제할 수 있습니다. 


</details>

## Question 12. What is `undefined x 1` in JavaScript

```javascript
var trees = ["redwood", "bay", "cedar", "oak", "maple"];
delete trees[3];
```

<details><summary><b>Answer</b></summary>
 - When you run the code above and do `console.log(trees);` in chrome developer console then you will get `["redwood", "bay", "cedar", undefined × 1, "maple"]`.
 - In the recent versions of Chrome you will see the word `empty` of `undefined x 1`.
 - When you run the same code in Firefox browser console then you will get `["redwood", "bay", "cedar", undefined, "maple"]`

Clearly we can see that Chrome has its own way of displaying uninitialized index in arrays. However when you check `trees[3] === undefined` in any browser you will get similar output as `true`.

**Note:** Please remember that you need not check for the uninitialized index of the array in  `trees[3] === 'undefined × 1'` it will give an error because `'undefined × 1'` this is just way of displaying an uninitialized index of an array in chrome.



</details>

## Question 13. 다음 코드의 결과는 무엇일까요?

```javascript
var trees = ["xyz", "xxxx", "test", "ryan", "apple"];
delete trees[3];
console.log(trees.length);
```
<details><summary><b>정답</b></summary>

이 코드의 결과는 `5`를 출력합니다. 우리가 배열의 삭제를 위해 `delete` 연산자를 사용했을 때, 배열 길이에는 영향을 끼치지 않습니다. 비록 `delete` 연산자를 사용하여 배열의 원소를 삭제했을 지라도 길이는 그대로 입니다.

delete 연산자를 사용하여 배열 원소를 삭제하면 더이상 배열에는 삭제한 원소가 존재하지 않습니다.  삭제된 값은 `undefined x 1` 로 크롬에서 대체되고 인덱스 자리에 `undefined`가 위치합니다. 만약 당신이 `console.log(trees)`로 출력한다면 크롬에서는   `["xyz", "xxxx", "test", undefined × 1, "apple"]` 를 출력할 것이고 Firefox에서는  `["xyz", "xxxx", "test", undefined, "apple"]` 를 출력할 것입니다.

</details>

## Question 14. What will be the output of the following code?

```javascript
var bar = true;
console.log(bar + 0);   
console.log(bar + "xyz");  
console.log(bar + true);  
console.log(bar + false);
```
<details><summary><b>Answer</b></summary>

The code above will output `1, "truexyz", 2, 1` as output. Here's a general guideline  for the plus operator:
  - Number + Number  -> Addition
  - Boolean + Number -> Addition
  - Boolean + Boolean -> Addition
  - Number + String  -> Concatenation
  - String + Boolean -> Concatenation
  - String + String  -> Concatenation

  

</details>

## Question 15. 다음 코드의 결과는 무엇일까요?

```javascript
var z = 1, y = z = typeof y;
console.log(y);
```
<details><summary><b>정답</b></summary>

이코드의 결과는 `"undefined"`를 출력합니다. 연관 규칙 연산자에 따르면 우선 순위가 동일한 연산자는 연산자의 속성에 따라 처리됩니다. 여기서 할당 연산자는 `오른쪽에서 왼쪽` 으로 연산 순서가 진행되어서 `typeof y`연산이 먼저 진행되고 그 후 `"undefined"`가 z에 할당되고 z의 값이 y에 할당됩니다. 전체적인 순서는 다음과 같을것입니다.

```javascript
var z;
z = 1;
var y;
z = typeof y;
y = z;
```

</details>

## Question 16. What will be the output of the following code?

```javascript
// NFE (Named Function Expression)
var foo = function bar() { return 12; };
typeof bar();
```

<details><summary><b>Answer</b></summary>

The output will be `Reference Error`. To fix the bug we can try to rewrite the code a little bit: 

**Sample 1**

```javascript
var bar = function() { return 12; };
typeof bar();
```

or

**Sample 2**

```javascript
function bar() { return 12; };
typeof bar();
```

The function definition can have only one reference variable as a function name, In **sample 1** `bar` is reference variable which is pointing to `anonymous function` and in **sample 2** we have function statement and `bar` is the function name.

```javascript
var foo = function bar() {
  // foo is visible here
  // bar is visible here
  console.log(typeof bar()); // Works here :)
};
// foo is visible here
// bar is undefined here
```

</details>

## Question 17a. 아래 형식으로 함수를 선언하는 방법들의 차이는 무엇일까요?

```javascript
var foo = function() {
  // Some code
}
```

```javascript
function bar () {
  // Some code
}
```
<details><summary><b>정답</b></summary>

주요한 차이는 함수 `foo`는 `run-time`에 정의됩니다. 그리고 이것을 function expression이라고 부릅니다. 반면에 함수 `bar`는 `parse time`에 실행됩니다. 또한 이것은 function statement라고 불립니다. 이것을 좀더 잘 이해하기 위해서 아래 코드를 살펴봅시다.

```javascript
// Run-Time 함수 선언
  foo(); // foo 함수를 여기서 호출하면 에러가 날 것 입니다.
  var foo = function() {
    console.log("Hi I am inside Foo");
  };
```

```javascript
// Parse-Time 함수 선언
bar(); // bar함수를 여기에 호출하면 이것은 에러가 나지 않을 것입니다.
function bar() {
  console.log("Hi I am inside Foo");
}
```
</details>

## Question 17b. 다음 코드의 결과는 무엇일까요?

```javascript
bar();
(function abc(){console.log('something')})();
function bar(){console.log('bar got called')};
```
<details><summary><b>정답</b></summary>


이것의 정답은 다음과 같이 될 것입니다.
``` 
bar got called
something
```
구문분석 시간동안 함수가 먼저 호출되고  정의되기때문에 JS 엔진은 가능한 구문분석 시간 정의를 찾고 실행 루프를 시작하려고 합니다. 이는 정의가 다른 함수를 게시하더라도  함수를 먼저 호출한다는 의미입니다. 

</details>

## Question 18. In which case the function definition is not hoisted in JavaScript?

<details><summary><b>Answer</b></summary>

Let's take the following **function expression**

```javascript
 var foo = function foo() {
     return 12;
 }
```

In JavaScript `var`-declared variables and functions are `hoisted`. Let's take function `hoisting` first. Basically, the JavaScript interpreter looks ahead to find all the variable declaration and hoists them to the top of the function where it's declared. For example:

```javascript
foo(); // Here foo is still undefined
var foo = function foo() {
  return 12;
};
```

The code above behind the scene look something like this:

```javascript
var foo = undefined;
foo(); // Here foo is undefined
foo = function foo() {
  // Some code stuff
}
```

```javascript
var foo = undefined;
foo = function foo() {
  // Some code stuff
}
foo(); // Now foo is defined here
```

</details>

## Question 19. 다음 코드의 결과는 무엇일까요?

```javascript
var salary = "1000$";

(function () {
  console.log("Original salary was " + salary);

  var salary = "5000$";

  console.log("My New Salary " + salary);
})();
```
<details><summary><b>정답</b></summary>

다음 코드의 정답은 다음과 같을 것입니다 : 호이스팅 문제 때문에 `undefined, 5000$`을 출력합니다. 위 코드에서, `salary`가 내부 범위에서 다시 선언될 때까지 `salary`가 외부범위로부터 값을 유지할 것으로 예상할 수 있습니다. 그러나 `호이스팅` 때문에 salary 값은 `undefined` 입니다. 다음의 코드를 더 잘 이해하기 위해, 여기 `salary` 변수는 함수 범위의 맨 위에 호이스팅되고 선언됩니다. 우리가 그 값을 `console.log`를 사용하여 출력할 때 그 값은 `undefined`가 됩니다. 이후에 변수는 다시 선언되고 새로운 값은 `"5000$"`로 할당됩니다.

</details>

## Question 20. What’s the difference between `typeof` and `instanceof`?

<details><summary><b>Answer</b></summary>

`typeof` is an operator that returns a string with the type of whatever you pass.

The `typeof` operator checks if a value belongs to one of the seven basic types: `number`, `string`, `boolean`, `object`, `function`, `undefined` or `Symbol`.

`typeof(null)` will return `object`.

`instanceof` is much more intelligent: it works on the level of prototypes. In particular, it tests to see if the right operand appears anywhere in the prototype chain of the left. `instanceof` doesn’t work with primitive types. The `instanceof` operator checks the current object and returns true if the object is of the specified type, for example:

```javascript
var dog = new Animal();
dog instanceof Animal; // Output : true
```

Here `dog instanceof Animal` is true since `dog` inherits from `Animal.prototype`

```javascript
var name = new String("xyz");
name instanceof String; // Output : true
```


Ref Link: [http://stackoverflow.com/questions/2449254/what-is-the-instanceof-operator-in-javascript](http://stackoverflow.com/questions/2449254/what-is-the-instanceof-operator-in-javascript)

</details>

## Question 21. 연관 배열의 길이를 계산하기

```javascript
var counterArray = {
  A : 3,
  B : 4
};
counterArray["C"] = 1;
```
<details><summary><b>정답</b></summary>

먼저 자바스크립트에서 연관배열은 객체와 같습니다. 두번째로 객체의 길이/크기를 계산하는데 사용할 수 있는 내장함수나 속성이 없더라도, 이러한 함수를 직접 작성할 수 있습니다.

#### 방법 1

`Object`는 `keys` 메소드를 가지고 있고 오브젝트의 길이를 계산하기 위해 사용됩니다.

```javascript
Object.keys(counterArray).length; // 3 출력
```

#### 방법 2

우리는 또한 객체를 반복하면서 객체의 프로퍼티의 개수를 세면서 객체의 길이를 계산할 수 있습니다. 이 방법은 객체의 프로토타입 체인에서 가져온 속성을 무시할 수 있습니다. 

```javascript
function getLength(object) {
  var count = 0;
  for(key in object) {
    // hasOwnProperty method check own property of object
    if(object.hasOwnProperty(key)) count++;
  }
  return count;
}
```

#### 방법 3 

모든 모던 브라우저들 (IE9 포함) 은 [`getOwnPropertyNames`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames) 메소드를 지원합니다. 그래서 우리는 다음과 같은 코드로 길이를 계산할 수 있습니다.

```javascript
Object.getOwnPropertyNames(counterArray).length; // 3 출력
```

#### 방법 4

[Underscore](https://underscorejs.org/#size) 와 [lodash](https://lodash.com/docs/4.17.10#size) 라이브러리들은 객체 길이를 계산하기 위해 `size` 메소드를 갖고 있습니다. 우리는  단지 이 메소드를 사용하기 위해 라이브러리를 포함 하는 것은 추천하지 않지만 만약 당신이 이미 프로젝트에 이 라이브러리를 사용하고 있다면 사용하는 것도 괜찮습니다.

```javascript
_.size({one: 1, two: 2, three: 3});
=> 3
```

</details>

## Question 22. Difference between `Function`, `Method` and `Constructor` calls in JavaScript.

<details><summary><b>Answer</b></summary>

If your are familiar with Object-oriented programming, More likely familiar to thinking of functions, methods, and class constructors as three separate things. But In JavaScript, these are just three different usage patterns of one single construct.

functions : The simplest usages of function call:

```javascript
function helloWorld(name) {
  return "hello world, " + name;
}

helloWorld("JS Geeks"); // "hello world JS Geeks"
```

Methods in JavaScript are nothing more than object properties that are functions.

```javascript
var obj = {
  helloWorld : function() {
    return "hello world, " + this.name;
  },
  name: 'John Carter'
}
obj.helloWorld(); // // "hello world John Carter"
```

Notice how `helloWorld` refer to `this` properties of obj. Here it's clear or you might have already understood that `this` gets bound to `obj`. But the interesting point that we can copy a reference to the same function `helloWorld` in another object and get a difference answer. Let see:

```javascript
var obj2 = {
  helloWorld : obj.helloWorld,
  name: 'John Doe'
}
obj2.helloWorld(); // "hello world John Doe"
```

You might be wonder what exactly happens in a method call here. Here we call the expression itself determine the binding of this `this`, The expression `obj2.helloWorld()` looks up the `helloWorld` property of obj and calls it with receiver object `obj2`.

The third use of functions is as constructors. Like function and method, `constructors` are defined with function.

```javascript
function Employee(name, age) {
  this.name = name;
  this.age = age;
}

var emp1 = new Employee('John Doe', 28);
emp1.name; // "John Doe"
emp1.age; // 28
```

Unlike function calls and method calls, a constructor call `new Employee('John Doe', 28)` creates a brand new object and passes it as the value of `this`, and implicitly returns the new object as its result.

The primary role of the constructor function is to initialize the object.


</details>

## Question 23. 다음 코드의 결과는 무엇일까요?

```javascript
function User(name) {
  this.name = name || "JsGeeks";
}

var person = new User("xyz")["location"] = "USA";
console.log(person);
```

<details><summary><b>정답</b></summary>

위 코드의 결과는 `"USA"`입니다. `new User("xyz")`는 새로운 객체를 생성하고 `location`속성을 생성해 `"USA"` 를 location 속성에 할당합니다. 그리고 그 객체는 person에 의해 참조됩니다.

이제 `new User("xyz")`를 `foo`라고 해보겠습니다. `"USA"`값은 `foo["location"]`에 할당될 것입니다. 그러나  [ECMAScript Specification](http://www.ecma-international.org/ecma-262/6.0/#sec-assignment-operators-runtime-semantics-evaluation) 에 따르면  할당은 가장 오른쪽 값을 반환하게 될 것이라고 되어있습니다. : 여기의 경우엔 `"USA"`가 되겠네요. 그럼 person에 "USA"가 할당될 것입니다.

이것이 어떻게 돌아가는지 좀더 잘 이해하기 위해서, 여기 아래 콘솔에 있는 코드를 한줄한줄씩 실행해봅시다. 

 ```javascript
function User(name) {
  this.name = name || "JsGeeks";
}

var person;
var foo = new User("xyz");
foo["location"] = "USA";
// foo는 다음과 같은 객체로 변화해있습니다. User {name: "xyz", location: "USA"}
 ```


</details>

## Question 24. What are Service Workers and when can you use them?

<details><summary><b>Answer</b></summary>

It’s a technology that allows your web application to use cached resources first, and provide default experience offline, before getting more data from the network later. This principle is commonly known as Offline First.

Service Workers actively use promises. A Service Worker has to be installed,activated and then it can react on fetch, push and sync events.

As of 2017, Service Workers are not supported in IE and Safari.

</details>

## Question 25. 자바스크립트에서 function과 method의 차이는 무엇인가요?

<details><summary><b>정답</b></summary>

js에서 그차이는 미묘합니다. function은 어떤 object와도 연결되지 않고 어떤 object안에도 정의되지 않은 이름 및 함수로 호출되는 코드조각입니다.

이것은 데이터를 전달하여 작동시킬 수 있으며(즉, 파라미터) 선택적으로 데이터를 반환할 수 있습니다(반환 값).

```javascript
// 함수 명세
function myFunc() {
  // 무엇인가 일을 한다;
}

// 함수 호출
myFunc();
```

여기서 myFunc() 함수 호출이 개체와 연결되어 있지 않으므로 개체를 통해 호출되지 않습니다.

함수는 즉시 호출되는 함수 식(IIFE)의 형태를 취할 수 있습니다.

```javascript

// 자신을 호출
(function() {
  // 무엇인가 일을 한다;
})();
```

마지막으로 화살표 함수도 있습니다.

```javascript
const myFunc = arg => {
    console.log("hello", arg)
} 
```

method는 코드이름으로 호출되고 object와 연결된 코드 조각입니다. method는 함수입니다. `obj1.myMethod()`같은 방식으로 method를 호출할때 `obj1`에 대한 참조가 `this` 변수에 할당됩니다. 다른말로 `this`의 값은 `obj1`안의 `myMethod`입니다. 

여기 method의 예제가 있습니다.

##### 예제 1
```javascript
var obj1 = {
  attribute: "xyz",
  myMethod: function () {  // Method
    console.log(this.attribute);
  }
};

// method 호출
obj1.myMethod();
```

`obj1`은 object이고 `myMethod`는 `obj1`과 연관된 method 입니다.

##### 예제 2
ES6에서 우리는 class를 갖고 있습니다. method는 아래 처럼 보일 것 입니다.

```javascript
class MyAwesomeClass {
  myMethod() {
    console.log("hi there");
  }
}

const obj1 = new MyAwesomeClass();
obj1.myMethod();
```

이해하기 : method는 함수의 특별한 타입의 함수가 아니며 함수를 선언하는 방법에 대한 것도 아닙니다. 이것은 단지 우리가 함수를 **호출** 하는 방법입니다. 아래를 보시죠.

```javascript
var obj1 = {
  prop1: "buddy"
}; 
var myFunc = function () {
  console.log("Hi there", this);
};
// function으로써 myFunc를 호출하기
myFunc(); // "Hi there undefined" 또는 "Hi there Window"를 출력할 것입니다.
 
obj1.myMethod = myFunc;
// 이제 우리는 obj1의 method로써 myFunc를 호출하겠습니다. 이렇게 obj1을 가리켜줍니다.
obj1.myMethod(); //  "Hi there"을 출력할 것입니다. this는 {prop1: "buddy", myMethod: ƒ} 이렇게 나옵니다.

```

</details>

## Question 26. What is IIFE (Immediately Invoked Function Expression) and how it can be useful?
<details><summary><b>Answer</b></summary>

#### Definition
IIFE a function that runs as soon as it's defined. Usually it's anonymous (doesn't have a function name), but it also can be named. Here's an example of IIFE:

```javascript
(function() {
  console.log("Hi, I'm IIFE!");
})();
// outputs "Hi, I'm IIFE!"
```
#### Explanation

So, here's how it works. Remember the difference between function statements (`function a () {}`) and function expressions (`var a = function() {}`)? So, IIFE is a function expression. To make it an expression we surround our function declaration into the parens. We do it to explicitly tell the parser that it's an expression, not a statement (JS doesn't allow statements in parens).

After the function you can see the two `()` braces, this is how we run the function we just declared. 

That's it. The rest is details.  
- The function inside IIFE doesn't have to be anonymous. This one will work perfectly fine and will help to detect your function in a stacktrace during debugging: 
  ```javascript
  (function myIIFEFunc() {
    console.log("Hi, I'm IIFE!");
  })();
  // outputs "Hi, I'm IIFE!"
  ```
- It can take some parameters:
  ```javascript
  (function myIIFEFunc(param1) {
    console.log("Hi, I'm IIFE, " + param1);
  })("Yuri");
  // outputs "Hi, I'm IIFE, Yuri!"
  ```
  Here there value `"Yuri"` is passed to the `param1` of the function.
- It can return a value: 
  ```javascript
  var result = (function myIIFEFunc(param1) {
    console.log("Hi, I'm IIFE, " + param1);
    return 1;
  })("Yuri");
  // outputs "Hi, I'm IIFE, Yuri!"
  // result variable will contain 1
  ```
- You don't have to surround the function declaration into parens, although it's the most common way to define IIFE. Instead you can use any of the following forms: 
  - `~function(){console.log("hi I'm IIFE")}()`
  - `!function(){console.log("hi I'm IIFE")}()`
  - `+function(){console.log("hi I'm IIFE")}()`
  - `-function(){console.log("hi I'm IIFE")}()`
  - `(function(){console.log("hi I'm IIFE")}());`
  - `var i = function(){console.log("hi I'm IIFE")}();`
  - `true && function(){ console.log("hi I'm IIFE") }();`
  - `0, function(){ console.log("hi I'm IIFE") }();`
  - `new function(){ console.log("hi I'm IIFE") }`
  - `new function(){ console.log("hi I'm IIFE") }()`
  
  Please don't use all these forms to impress colleagues, but be prepared that you can encounter them in someone's code. 

#### Applications and usefulness

Variables and functions that you declare inside an IIFE are not visible to the outside world, so you can:
 - Use the IIFE for isolating parts of the code to hide details of implementation.
 - Specify the input interface of your code by passing commonly used global objects (window, document, jQuery, etc.) IIFE’s parameters, and then reference these global objects within the IIFE via a local scope.
 - Use it in closures, when you use closures in loops.
 - IIFE is the basis of in the module pattern in ES5
code, it helps to prevent polluting the global scope and provide the module interface to the outside.


</details>

## Question 27. 자바스크립트에서의 싱글톤 디자인 패턴
<details><summary><b>정답</b></summary>

싱글톤 패턴은 종종 자바스크립트 디자인 패턴으로 사용됩니다. 이 방법은 한개의 변수를 통해 액세스할 수 있는 논리 단위로 코드를 감싸는 방법입니다. 싱글톤 디자인 패턴은 애플리케이션의 수명 동안 하나의 객체 인스턴스만 필요할 때 사용됩니다. 자바스크립트에서 싱글톤 패턴은 여러 용도로 사용될 수 있으며, 네임스페이싱을 위해서도 사용될 수 있습니다.이 패턴은 페이지의 전역 변수의 숫자를 줄이고(전역 공간을 오염시키지 않도록), 코드를 일관된 방식으로 구성함으로써 페이지의 가독성과 유지보수를 증가시킵니다.

여기 싱글톤 패턴의 전통적인 정의의 두가지 중요한 점이 있습니다:

- 한 클래스에 대해 하나의 인스턴스만 허용되어야 합니다.
- 단일 인스턴스에 대한 글로벌 액세스 지점을 허용해야 합니다.

자바스크립트 문맥으로 싱글톤 패턴을 이해해 봅시다:

> 이 개체는 네임스페이스를 만들고 관련 메서드 및 속성 집합(캡슐화)을 그룹화하는 데 사용되는 개체이며, 시작을 허용할 경우 한 번만 시작할 수 있습니다.

자바스크립트에서, 우리는 객체 리터럴을 통해 싱글톤을 만들 수 있습니다. 여기 몇몇 다른 방법이 있지만 다음 포스트때 그 방법을 다룰것입니다.

싱글톤 객체는 두 부분으로 구성됩니다. 개체 자체로서, 개체 내의 구성원(메소드와 속성)과 개체에 액세스하는 데 사용되는 글로벌 변수를 포함합니다. 변수는 전역적이어서 페이지의 모든 위치에서 객체에 액세스할 수 있습니다. 이 변수는 싱글톤 패턴의 주요 기능입니다.

**자바스크립트: 네임스페이스로써의 싱글톤**

위에서 말한 바와 같이, 자바스크립트에서 네임스페이스를 선언하는 데 싱글톤을 사용할 수 있습니다. NameSpacing은 JavaScript에서 중요한 프로그래밍의 부분입니다. 왜냐하면 실수하여 모든 것을 덮어쓸 수 있고,  변수나 함수, 심지어 클래스도 모르는 사이에 삭제하기가 매우 쉽기 때문입니다. 다른 팀원과 병렬로 작업할 때 자주 발생하는 일반적인 예입니다.

```javascript
function findUserName(id) {

}

/* 후에 다른 프로그래머가 이부분을 추가할 것입니다. */
var findUserName = $('#user_list');

/* 당신은 이부분을 호출하길 원합니다. :( */
console.log(findUserName())
```

실수로 변수를 덮어쓰지 않도록 하는 가장 좋은 방법 중 하나는 싱글톤 개체 내에서 코드 네임스페이스를 지정하는 것입니다.

```javascript
/*  Using Namespace */

var MyNameSpace = {
  findUserName : function(id) {},
  // 다른 메소드와 함수 역시 여기에 선언합니다.
}

/*  후에 다른 프로그래머가 이부분을 추가할 것입니다.  */
var findUserName = $('#user_list');

/* 당신은 이부분을 호출하길 원합니다. */
console.log(MyNameSpace.findUserName());
```

### 싱글톤 디자인 패턴 구현

```javascript
/* 싱글 톤 패턴에 대한 지연 인스턴스화 스켈레톤 */

var MyNameSpace = {};
MyNameSpace.Singleton = (function() {

  // 단일 인스턴스를 보유하는 private 속성
  var singletonInstance;  

  // 모든 일반 코드가 여기에 있습니다.
  function constructor() {
    // Private 멤버들
    var privateVar1 = "Nishant";
    var privateVar2 = [1,2,3,4,5];

    function privateMethod1() {
      // 코드
    }

    function privateMethod1() {
      // 코드
    }

    return {
      attribute1 : "Nishant",
      publicMethod: function() {
        alert("Nishant");// 코드
      }
    }
  }

  return {
    // public 메서드 (Singleton 개체에 대한 전역 액세스 지점)
    getInstance: function() {
      // 인스턴스가 이미 존재하고 return
      if(!singletonInstance) {
        singletonInstance = constructor();
      }
      return singletonInstance;           
    }           
  }

})();   

// publicMethod에 액세스하기
console.log(MyNamespace.Singleton.getInstance().publicMethod());
```

위에서 구현한 싱글톤은 이해하기 쉽습니다. 싱글톤 클래스는 싱글톤 인스턴스에 대한 정적 참조를 유지하고 정적 getInstance() 메서드에서 해당 참조를 반환합니다.

</details>

## Question 28. What are the ways of creating objects in JavaScript ?

<details><summary><b>Answer</b></summary>

#### Method 1: Function based

This method is useful if we want to create several similar objects. In the code sample below, we wrote the function `Employee` and used it as a constructor by calling it with the `new` operator. 

```javascript

  function Employee(fName, lName, age, salary){
  	this.firstName = fName;
  	this.lastName = lName;
  	this.age = age;
  	this.salary = salary;
  }

  // Creating multiple object which have similar property but diff value assigned to object property.
  var employee1 = new Employee('John', 'Moto', 24, '5000$');
  var employee2 = new Employee('Ryan', 'Jor', 26, '3000$');
  var employee3 = new Employee('Andre', 'Salt', 26, '4000$');
```

#### Method 2: Object Literal

Object Literal is best way to create an object and this is used frequently. Below is code sample for create employee object which contains property as well as method.

```javascript
var employee = {
	name : 'Nishant',
	salary : 245678,
	getName : function(){
		return this.name;
	}
}
```
The code sample below is Nested Object Literal, Here address is an object inside employee object.

```javascript
var employee = {
	name : 'Nishant',
	salary : 245678,
	address : {
		addressLine1 : 'BITS Pilani',
		addressLine2 : 'Vidya Vihar'.
		phoneNumber: {
		  workPhone: 7098889765,
		  homePhone: 1234567898
		}
	}
}
```
#### Method 3: From `Object` using `new` keyword

In the code below, a sample object has been created using `Object`'s constructor function.

```javascript
var employee = new Object(); // Created employee object using new keywords and Object()
employee.name = 'Nishant';
employee.getName = function(){
	return this.name;
}
```

#### Method 4:** Using `Object.create`

`Object.create(obj)` will create a new object and set the `obj` as its prototype. It’s a modern way to create objects that inherit properties from other objects. `Object.create` function doesn’t run the constructor. You can use `Object.create(null)` when you don’t want your object to inherit the properties of `Object`.

</details>

## Question 29. 객체를 가져와서 객체의 복사본을 만드는 deepClone 함수를 작성해봅시다.

``` javascript
var newObject = deepClone(obj);
```
<details><summary><b>정답</b></summary>


```javascript
function deepClone(object){
	var newObject = {};
	for(var key in object){
		if(typeof object[key] === 'object'  && object[key] !== null ){
		 newObject[key] = deepClone(object[key]);
		}else{
		 newObject[key] = object[key];
		}
	}
	return newObject;
}
```

**설명:** 우리는 먼저 객체의 deep copy의 의미가 무엇인지 부터 알아야 합니다. 주어진 `personalDetail`객체는 `address`객체와 그 일부인 `phoneNumber`객체를 살펴보도록 합시다. 간단한 `personalDetail` 은 중첩된 객체(객체안에 객체)입니다. 여기 deep copy는 중첩된 객체를 포함하여 `personalDetail`객체의 전부를 복사해야 하는 것을 의미합니다.

```javascript
var personalDetail = {
	name : 'Nishant',
	address : {
	  location: 'xyz',
	  zip : '123456',
	  phoneNumber : {
	    homePhone: 8797912345,
	    workPhone : 1234509876
	  }
	}
}
```
그래서 우리가 deep clone을 할 때 우리는 모든 속성을 복사해야 합니다.(중첩된 객체를 포함하여)

</details>

## Question 30. Best way to detect `undefined` object property in JavaScript.

<details><summary><b>Answer</b></summary>

> Suppose we have given an object `person`

```javascript
var person = {
	name: 'Nishant',
	age : 24
}
```
Here the `person` object has a `name` and `age` property. Now we are trying to access the **salary** property which we haven't declared on the person object so while accessing it will return undefined. So how we will ensure whether property is undefined or not before performing some operation over it?

**Explanation:**

We can use `typeof` operator to check undefined

```javascript
if(typeof someProperty === 'undefined'){
	console.log('something is undefined here');
}
```
Now we are trying to access salary property of person object.

```javascript
if(typeof person.salary === 'undefined'){
	console.log("salary is undefined here because we haven't declared");
}
```
</details>

## Question 31. 오브젝트를 가져와서 오브젝트의 복사본을 만들지만 깊은 속성은 복사하지 않는 `Clone` 함수를 작성해 보세요.

```javascript
   var objectLit = {foo : 'Bar'}; 
	var cloneObj = Clone(obj); // Clone은 당신이 작성해야하는 함수입니다.
	console.log(cloneObj === Clone(objectLit)); // 이것은 false를 리턴해야합니다.
	console.log(cloneObj == Clone(objectLit)); // 이것은 true를 리턴해야합니다.
```
<details><summary><b>정답</b></summary>


```javascript
function Clone(object){
  var newObject = {};
  for(var key in object){
  	newObject[key] = object[key];
  }
  return newObject;
}
```

</details>

## Question 32. What are promises and how they are useful?

<details><summary><b>Answer</b></summary>

We use promises for handling asynchronous interactions in a sequential manner. They are especially useful when we need to do an async operation and THEN do another async operation based on the results of the first one. For example, if you want to request the list of all flights and then for each flight you want to request some details about it. The promise represents the future value. It has an internal state (`pending`, `fulfilled` and `rejected`) and works like a state machine.

A promise object has `then` method, where you can specify what to do when the promise is fulfilled or rejected.

You can chain `then()` blocks, thus avoiding the callback hell. You can handle errors in the `catch()` block.  After a promise is set to fulfilled or rejected state, it becomes immutable.

Also mention that you know about more sophisticated concepts: 
 - `async/await` which makes the code appear even more linear
 - RxJS observables can be viewed as the recyclable promises

Be sure that you can implement the promise, read [one of the articles on a topic](https://opensourceconnections.com/blog/2014/02/16/a-simple-promise-implementation-in-about-20-lines-of-javascript/), and learn the source code of the [simplest promise implementation](https://gist.github.com/softwaredoug/9044640). 


</details>

## Question 33. 어떻게 자바스크립트 객체에 키가 존재하는지 체크해야 하나요?

<details><summary><b>정답</b></summary>

 **name** 과 **age** 속성을 갖는 `person` 객체가 있다고 해봅시다.

```javascript
var person = {
	name: 'Nishant',
	age: 24
}
```
이제 `person` 객체에 `name`속성이 있는지 어떻게 확인해야 할까요?

자바스크립트에서 객체는 자신의 프로퍼티를 가질 수 있습니다. 위의 예에서 name 과 age는 person 객체의 속성입니다. 또한 객체는 toString 과 같은 기본 개체의 상속된 속성이 있기도 합니다.

이제 어떻게 우리가 속성이 자신의 속성인지 상속된 속성인지 체크하는 방법에 대해 알아보도록 하겠습니다.

방법 1: 우리는 `in`연산자를 사용하여 객체가 자신의 속성인지 상속된 속성인지 체크할 수 있습니다.

```javascript
console.log('name' in person); // 자신의 속성이면 true를 출력합니다.
console.log('salary' in person); // undefined를 출력합니다.
```
`in` 연산자는 또한 자신의 속성으로써 정의된 속성을 찾지 못한다면 상속된 속성을 찾아봅니다. 예를들어 만약 toString 프로퍼티의 존재를 확인하려 한다면 먼저 객체의 기본 속성안을 들여다 본다음 기본 속성안에 선언되지 않았으므로 그 다음으로 상속된 속성을 살펴봅니다.

```javascript
console.log('toString' in person); // true를 출력합니다.
```
만약 우리가 상속되지 않은 속성만을 검사하고 싶다면 `hasOwnProperty`를 쓰면 됩니다.

```javascript
console.log(person.hasOwnProperty('toString')); // false 를 출력합니다.
console.log(person.hasOwnProperty('name')); // true를 출력합니다
console.log(person.hasOwnProperty('salary')); // false를 출력합니다.
```

</details>

## Question 34. What is NaN, why do we need it, and when can it break the page?

<details><summary><b>Answer</b></summary>

`NaN` stands for “not a number.” and it can break your table of numbers when it has an arithmetic operation that is not allowed. Here are some examples of how you can get `NaN`:

```javascript
Math.sqrt(-5);
Math.log(-1);
parseFloat("foo"); /* this is common: you get JSON from the server, convert some strings from JSON to a number and end up with NaN in your UI. */
```

`NaN` is not equal to any number, it’s not less or more than any number, also it's not equal to itself: 

```javascript
NaN !== NaN
NaN < 2 // false
NaN > 2 // false
NaN === 2 // false
```

To check if the current value of the variable is NaN, you have to use the `isNaN` function. This is why we can often see NaN in the webpages: it requires special check which a lot of developers forget to do. 

Further reading: [great blogpost on ariya.io](https://ariya.io/2014/05/the-curious-case-of-javascript-nan)

</details>

## Question 35. ES5 만 사용하여 버그를 수정하세요.

```javascript
var arr = [10, 32, 65, 2];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function() {
    console.log('The index of this number is: ' + i);
  }, 3000);
}
```
<details><summary><b>정답</b></summary>

ES6에서는 `var i` 를 `let i` 로 대체하면 버그를 해결할 수 있습니다..

ES5에서는 함수 스코프를 아래와 같이 생성해야 합니다.

```javascript 
var arr = [10, 32, 65, 2];
for (var i = 0; i < arr.length; i++) {
  setTimeout(function(j) {
    return function () {
      console.log('The index of this number is: ' + j)
    };
  }(i), 3000);
}
```

이것은 forEach로도 가능합니다.(forEach의 스코프 안에서도 변수를 유지하도록 허용할 수 있습니다.)

```javascript 
var arr = [10, 32, 65, 2];
arr.forEach(function(ele, i) {
  setTimeout(function() {
    console.log('The index of this number is: ' + i);
  }, 3000);
})
```

</details>

## Question 36. How to check if the value of a variable in an array?

<details><summary><b>Answer</b></summary>

We always encounter in such situation where we need to know whether value is type of array or not.

For instance : the code below perform some operation based value type

```javascript
function(value){
	if("value is an array"){
		// Then perform some operation
	}else{
		// otherwise
	}
}
```

Let's discuss some way to detect an array in JavaScript.

**Method 1:**

Juriy Zaytsev (Also known as kangax) proposed an elegant solution to this.

```javascript
	function isArray(value){
		return Object.prototype.toString.call(value) === '[object Array]';
	}
```
This approach is most popular way to detecting a value of type array in JavaScript and recommended to use. This approach relies on the fact that, native toString() method on a given value produce a standard string in all browser. 


**Method 2:** 

Duck typing test for array type detection

```javascript
 // Duck typing arrays
 function isArray(value){
 	return typeof value.sort === 'function';
 }
```
As we can see above isArray method will return true if value object have `sort` method of type `function`. Now assume you have created a object with sort method

```javascript
	var bar = {
		sort: function(){
			// Some code 
		}
	}
```
Now when you check `isArray(bar)` then it will return true because bar object has sort method, But the fact is bar is not an array.

So this method is not a best way to detect an array as you can see it's not handle the case when some object has sort method.

**Method 3:** 

ECMAScript 5 has introduced **Array.isArray()** method to detect an array type value. The sole purpose of this method is accurately detecting whether a value is an array or not.

In many JavaScript libraries you may see the code below for detecting an value of type array.

```javascript
function(value){
   // ECMAScript 5 feature
	if(typeof Array.isArray === 'function'){
		return Array.isArray(value);
	}else{
	   return Object.prototype.toString.call(value) === '[object Array]';
	}
}
```

**Method 4:**

You can query the constructor name:

```javascript
function isArray(value) {
	return value.constructor.name === "Array";
}

```

**Method 5:**

You check  if a given value is an `instanceof Array`:

```javascript
function isArray(value) {
	return value instanceof Array;
}
```

</details>

## Question 37. 자바스크립트에서 모든 유형의 참조 값을 검색하는 가장 좋은 방법은 무엇인가요?

<details><summary><b>정답</b></summary>

자바스크립트에서 객체는 참조타입으로써 호출됩니다. 원시값이 아닌 다른값들은 분명히 참조값입니다. **Object**, **Array**, **Funciton**, **Date**, **null**, **Error** 와 같은 기본 제공 참조 유형이 있습니다.

`typeof` 연산자를 사용하여 객체를 감지하는 방법

```javascript
console.log(typeof {});           // object
console.log(typeof []);           // object
console.log(typeof new Array());  // object
console.log(typeof null);         // object 
console.log(typeof new RegExp()); // object
console.log(typeof new Date());   // object
```
그러나 typeof 연산자를 사용하여 객체를 감지하는 단점은 `null`이 `object`라고 리턴한다는 점입니다. (이것은 자바스크립트에서 null이 객체라는 사실입니다.)

참조 타입을 검사하는 가장 좋은 방법은 `instanceof` 연산자를 사용하는 것입니다.

>문법 : **value** instanceof **constructor**   

```javascript
//array 감지하기
if(value instanceof Array){
	console.log("value is type of array");
}
```
```javascript
// Employee 생성자 함수
function Employee(name){
	this.name = name; // Public property
}

var emp1 = new Employee('John');

console.log(emp1 instanceof Employee); // true
```
아래의 예를 참조하면 `instanceof` 는 객체에 사용되는 생성자를 확인할 뿐만 아니라 프로토타입 체인을  확인할 수 있습니다.

```javascript
console.log(emp1 instanceof Object); // true
```

</details>

## Question 38. How does Object.create method works JavaScript?

<details><summary><b>Answer</b></summary>

The ECMAScript 5 **Object.create()** method is the easiest way for one object to inherit from another, without invoking a constructor function. 

**For instance:** 

```javascript
var employee = {
  name: 'Nishant',
  displayName: function () {
    console.log(this.name);
  }
};

var emp1 = Object.create(employee);
console.log(emp1.displayName());  // output "Nishant"
```

In the example above, we create a new object `emp1` that inherits from `employee`. In other words `emp1`'s prototype is set to `employee`. After this emp1 is able to access the same properties and method on employee until new properties or method with the same name are defined.

**For instance:** Defining `displayName()` method on `emp1` will not automatically override the employee `displayName`.

```javascript
emp1.displayName = function() {
	console.log('xyz-Anonymous');
};

employee.displayName(); //Nishant
emp1.displayName();//xyz-Anonymous
```

In addition to this **`Object.create(`)** method also allows to specify a second argument which is an object containing additional properties and methods to add to the new object.

**For example**

```javascript
var emp1 = Object.create(employee, {
	name: {
		value: "John"
	}
});

emp1.displayName(); // "John"
employee.displayName(); // "Nishant"
```
In the example above, `emp1` is created with it's own value for name, so calling **displayName()** method will display `"John"` instead of `"Nishant"`.

Object created in this manner give you full control over newly created object. You are free to add, remove any properties and method you want.

</details>

## Question 39. 자바스크립트에서 상속을 사용하려면 생성자 함수를 어떻게 사용해야 하나요?

<details><summary><b>정답</b></summary>

name, age, salary 속성을 갖고 있고 **incrementSalary** 메소드를 갖고있는 `Person` 함수를 만들어봅시다.

```javascript
function Person(name, age, salary) {
  this.name = name;
  this.age = age;
  this.salary = salary;
  this.incrementSalary = function (byValue) {
    this.salary = this.salary + byValue;
  };
}
```

이제 우리는 Person class의 속성을 모두 포함하는 Employee class를 만들고 싶다고 해보죠. 그리고 Employee 속성에 추가적으로 몇몇 속성들을 좀더 추가하고 싶습니다.

```javascript
function Employee(company){
	this.company = company;
}

//Prototypal Inheritance 
Employee.prototype = new Person("Nishant", 24,5000);
```
위의 예에서 **Employee**타입은 **Person** 으로부터 상속받았습니다. 이것은 `Employee` 프로토타입에 `Person`의 새로운 인스턴스를 할당함으로써 수행됩니다. 그후 모든 `Employee` 인스턴스는 `Person`으로부터 속성과 메소드를 상속받습니다. 

```javascript
//Prototypal Inheritance 
Employee.prototype = new Person("Nishant", 24,5000);

var emp1 = new Employee("Google");

console.log(emp1 instanceof Person); // true
console.log(emp1 instanceof Employee); // true
```

이제 Constructor inheritance를 이해해보도록 하겠습니다.

```javascript
//Person class 정의
function Person(name){
	this.name = name || "Nishant";
}

var obj = {};

// obj는 Person class 속성과 메소드를 상속받습니다.
Person.call(obj); // constructor inheritance

console.log(obj); // Object {name: "Nishant"}
```
여기 우리가 **Person.call(obj)**를 호출하는 것을 볼 수 있습니다. 이것은 `Person`으로부터 `obj`에게 이름 속성을 정의합니다. 

```javascript
console.log(name in obj); // true
```
Type-based 상속은 javascript에서 기본적으로 사용되는 것이 아니라 개발자가 정의한 생성자 함수와 같이 사용하는 것이 좋습니다. 이와 더불어 유사한 유형의 객체를 생성하는 방법도 유연하게 허용됩니다.

</details>

## Question 40. How we can prevent modification of object in JavaScript ?.

<details><summary><b>Answer</b></summary>

 ECMAScript 5 introduce several methods to prevent modification of object which lock down object to ensure that no one, accidentally or otherwise, change functionality of Object.

There are three levels of preventing modification: 

**1: Prevent extensions :** 

No new properties or methods can be added to the object, but one can change the existing properties and method.

For example: 

```javascript
var employee = {
	name: "Nishant"
};

// lock the object 
Object.preventExtensions(employee);

// Now try to change the employee object property name
employee.name = "John"; // work fine 

//Now try to add some new property to the object
employee.age = 24; // fails silently unless it's inside the strict mode
```
**2: Seal :**

It is same as prevent extension, in addition to this also prevent existing properties and methods from being deleted.

To seal an object, we use **Object.seal()** method. you can check whether an object is sealed or not using **Object.isSealed();**

```javascript
var employee = {
	name: "Nishant"
};

// Seal the object 
Object.seal(employee);

console.log(Object.isExtensible(employee)); // false
console.log(Object.isSealed(employee)); // true

delete employee.name // fails silently unless it's in strict mode

// Trying to add new property will give an error
employee.age = 30; // fails silently unless in strict mode
```

when an object is sealed, its existing properties and methods can't be removed. Sealed object are also non-extensible.

**3: Freeze :**

Same as seal, In addition to this prevent existing properties methods from being modified (All properties and methods are read only).

To freeze an object, use Object.freeze() method. We can also determine whether an object is frozen using Object.isFrozen();

```javascript
var employee = {
	name: "Nishant"
};

//Freeze the object
Object.freeze(employee); 

// Seal the object 
Object.seal(employee);

console.log(Object.isExtensible(employee)); // false
console.log(Object.isSealed(employee));     // true
console.log(Object.isFrozen(employee));     // true


employee.name = "xyz"; // fails silently unless in strict mode
employee.age = 30;     // fails silently unless in strict mode
delete employee.name   // fails silently unless it's in strict mode
```

Frozen objects are considered both non-extensible and sealed.

**Recommended:**

If you are decided to prevent modification, sealed, freeze the object then use in strict mode so that you can catch the error.

For example: 

```javascript
"use strict";

var employee = {
	name: "Nishant"
};

//Freeze the object
Object.freeze(employee); 

// Seal the object 
Object.seal(employee);

console.log(Object.isExtensible(employee)); // false
console.log(Object.isSealed(employee));     // true
console.log(Object.isFrozen(employee));     // true


employee.name = "xyz"; // fails silently unless in strict mode
employee.age = 30;     // fails silently unless in strict mode
delete employee.name;  // fails silently unless it's in strict mode
```


</details>

## Question 41. console.log에 접두사로 `(당신이 쓰고 싶은 메시지)`를 출력하기 위해서는 어떻게 로그함수를 써야할까요? 
예를 들어, 만약 당신이 `console.log("Some message")`를 로그로 찍으려고 한다면 그때 출력은 **(당신이 쓰고 싶은 메시지) Some message**가 되어야만 합니다.

 <details><summary><b>정답</b></summary>


에러메시지를 로그로 찍거나 몇몇 정보 메시지들은 항상 당신이 console.log를 사용하면서 자바스크립트에서 다룰때 필요합니다. 경우에 따라 프로그램에서 생성된 메시지 로그를 식별하기 위해 console.log에  접두사를 추가하고 싶을때가 있을 것입니다.

이렇게 하는 일반적인 방법은 보통 다음과 같이 모든 console.log 에 다음과 같이 할 것입니다.

```javascript
console.log('your app name' + 'some error message');
```
그러나 이러한 방식은 모든 로그에 적어줘야 하므로 비효율적입니다.

여기 몇몇 성공적인 방법들이 있습니다.

```javascript
function appLog() {
  var args = Array.prototype.slice.call(arguments);
  args.unshift('your app name');
  console.log.apply(console, args);
}

appLog("Some error message"); 
//콘솔에 다음과 같이 찍힙니다: 'your app name Some error message'
```

</details>

## Question 42 . Write a function which will test string as a literal and as an object ?

For example: We can create string using string literal and using String constructor function. 

```javascript
 // using string literal
 var ltrlStr = "Hi I am string literal";
 // using String constructor function 
 var objStr = new String("Hi I am string object");
```

<details><summary><b>Answer</b></summary>

 We can use typeof operator to test string literal and instanceof operator to test String object.

```javascript
 function isString(str) {
 	return typeof(str) == 'string' || str instanceof String;
 }
 
 var ltrlStr = "Hi I am string literal";
 var objStr = new String("Hi I am string object");
 console.log(isString(ltrlStr)); // true
 console.log(isString(objStr)); // true
```
</details>

## Question 43 . 자바스크립트에서 익명함수의 사용 사례는 무엇이 있을까요?

<details><summary><b>정답</b></summary>


익명함수는 기본적으로 다음과 같은 시나리오에 사용됩니다.

1. 함수가 한곳에서만 사용되는 경우에는 이름이 필요없으므로, 함수에 이름을 추가할 필요가 없음

  setTimeout 함수의 예를 살펴봅시다.

  ```javascript
  setTimeout(function(){
  	alert("Hello");
  },1000);
  ```
  여기서는 `hello`알림 기능이 어플리케이션에서 한번만 사용되는 것을 확신한다면 이름을 사용할 필요가 없습니다.

2. 익명함수는 인라인으로 선언되고 인라인 함수는 부모의 범위에서 변수를 엑세스 할 수 있다는 장점이 있음

  이벤트 핸들러의 예제를 살펴봅시다. 주어진 오브젝트에 대해 특정 유형(예: 클릭) 이벤트를 알립니다. 

  클릭 이벤트를 추가하고자 하는 HTML 요소(버튼)이 있다고 가정하고, 사용자가 버튼을 클릭할 때 로직을 실행하고자 합니다.

  ```html
  <button id="myBtn"></button>
  ```
  이벤트 리스너를 추가합니다. 

  ```javascript
  var btn = document.getElementById('myBtn');
  btn.addEventListener('click', function () {
    alert('button clicked');
  });
  ```

  위의 예는 이벤트 핸들러에서 콜백함수로써 익명함수가 사용되는 것을 보여줍니다.

3. 함수를 호출하기 위해 파라미터로 익명함수를 넘겨주는 것

  예: 

  ```javascript
  // 콜백함수를 실행하는 함수
  function processCallback(callback){
  	if(typeof callback === 'function'){
  		callback();
  	}
  }
  
  // 콜백으로써 익명함수를 넘겨주고 호출함
  processCallback(function(){
  	alert("Hi I am anonymous callback function");
  });
  ```
  익명함수를 사용할지 말지를 결정하는 가장 좋은 방법은 아래와 같은 질문에 답해보는 것 입니다:

정의할 기능들이 다른 곳에서도 쓰이나요?

만약 이 답이 yes 라면 익명함수를 만들기 보다는 함수의 이름을 만들어야 합니다.

**익명함수를 사용하는 장점**

1. 코드의 양을 줄일 수 있습니다. 특히 콜백함수나 재귀함수 같은경우에.
2.  필요없는 전역변수공간 할당을 피할 수 있습니다.

</details>

## Question 44 . How to set a default parameter value ?

<details><summary><b>Answer</b></summary>

 If you are coming from python/c# you might be using default value for function parameter incase value(formal parameter) has not been passed. For instance : 

```python
// Define sentEmail function 
// configuration : Configuration object
// provider : Email Service provider, Default would be gmail
def sentEmail(configuration, provider = 'Gmail'):
	# Your code logic
```
**In Pre ES6/ES2015**

There are a lot of ways by which you can achieve this in pre ES2015.

Let's understand the code below by which we achieved setting default parameter value.

**Method 1: Setting default parameter value** 

```javascript
function sentEmail(configuration, provider) {
  // Set default value if user has not passed value for provider
  provider = typeof provider !== 'undefined' ? provider : 'Gmail'  
  // Your code logic
;
}
// In this call we are not passing provider parameter value
sentEmail({
  from: 'xyz@gmail.com',
  subject: 'Test Email'
});
// Here we are passing Yahoo Mail as a provider value
sentEmail({
  from: 'xyz@gmail.com',
  subject: 'Test Email'
}, 'Yahoo Mail');
```

**Method 2: Setting default parameter value** 

```javascript
function sentEmail(configuration, provider) {
  // Set default value if user has not passed value for provider
  provider = provider || 'Gmail'  
  // Your code logic
;
}
// In this call we are not passing provider parameter value
sentEmail({
  from: 'xyz@gmail.com',
  subject: 'Test Email'
});
// Here we are passing Yahoo Mail as a provider value
sentEmail({
  from: 'xyz@gmail.com',
  subject: 'Test Email'
}, 'Yahoo Mail');
```

**Method 3: Setting default parameter value in ES6**
```javascript
function sendEmail(configuration, provider = "Gmail") {
  // Set default value if user has not passed value for provider
  
  // Value of provider can be accessed directly
  console.log(`Provider: ${provider}`);
}

// In this call we are not passing provider parameter value
sentEmail({
  from: 'xyz@gmail.com',
  subject: 'Test Email'
});
// Here we are passing Yahoo Mail as a provider value
sentEmail({
  from: 'xyz@gmail.com',
  subject: 'Test Email'
}, 'Yahoo Mail');
```

</details>

## Question 45. 두개의 자바스크립트 객체를 동적으로 합치는 코드를 작성해보세요.
아래 두 개의 오브젝트를 살펴봅시다. 

```javascript
var person = {
	name : 'John',
	age  : 24
}

var address = {
	addressLine1 : 'Some Location x',
	addressLine2 : 'Some Location y',
	city : 'NewYork'
} 
```
첫번째 객체 안에 두번째 객체의 속성을 모두 추가하고 두개의 오브젝트를 가지는 함수를 작성해봅시다.

<details><summary><b>정답</b></summary>


```javascript
merge(person , address); 
 
/* Now person should have 5 properties 
name , age , addressLine1 , addressLine2 , city */
```
**방법 1: ES6문법인 Object.assign을 사용하는 것**

```javascript
const merge = (toObj, fromObj) => Object.assign(toObj, fromObj);
```

**방법 2: 미리 만들어진 함수 없이 쓰는 것**

```javascript
function merge(toObj, fromObj) {
  // 두개의 파라미터가 오브젝트인지 먼저 확인
  if (typeof toObj === 'object' && typeof fromObj === 'object') {
    for (var pro in fromObj) {
      // 상속되지 않은 고유 속성을 할당합니다.
      if (fromObj.hasOwnProperty(pro)) {
        // 속성과 값을 할당합니다.
        toObj[pro] = fromObj[pro];
      }
    }
  }else{
  	throw "Merge function can apply only on object";
  }
}
```
</details>

## Question 46. What is non-enumerable property in JavaScript and how you can create one?

<details><summary><b>Answer</b></summary>

Object can have properties that don't show up when you iterate through object using for...in loop or using Object.keys() to get an array of property names. This properties is know as non-enumerable properties.

Let say we have following object

```javascript
var person = {
	name: 'John'
};
person.salary = '10000$';
person['country'] = 'USA';

console.log(Object.keys(person)); // ['name', 'salary', 'country']
```
As we know that person object properties `name`, `salary` ,`country` are enumerable hence it's shown up when we called Object.keys(person).

To create a non-enumerable property we have to use **Object.defineProperty()**. This is a special method for creating non-enumerable property in JavaScript.

```javascript
var person = {
	name: 'John'
};
person.salary = '10000$';
person['country'] = 'USA';

// Create non-enumerable property
Object.defineProperty(person, 'phoneNo',{
	value : '8888888888',
	enumerable: false
})

Object.keys(person); // ['name', 'salary', 'country']
```
In the example above `phoneNo` property didn't show up because we made it non-enumerable by setting **enumerable:false**

**Bonus**

Now let's try to change value of `phoneNo`

```javascript
person.phoneNo = '7777777777'; 
```

**Object.defineProperty()** also lets you create read-only properties as we saw above, we are not able to modify phoneNo value of a person object. This is because descriptor has **writable** property, which is `false` by default. Changing non-writable property value will return error in strict mode. In non-strict mode it won't through any error but it won't change the value of phoneNo.

</details>

## Question 47. 함수 바인딩이란 무엇인가요?

<details><summary><b>정답</b></summary>

함수 바인딩은 미리 자바스크립트 범주로 지정되며, 함수를 매개 변수로 전달하면서 코드 실행 컨텍스트를 보존하기 위해 이벤트 핸들러와 콜백함수와 같이 사용하는 매우 인기있는 기술입니다.

다음 예제를 살펴봅시다.

```javascript
var clickHandler = {
	message: 'click event handler',
	handleClick: function(event) {
		console.log(this.message);
	}
};

var btn = document.getElementById('myBtn');
// 버튼에 click 이벤트 추가
btn.addEventListener('click', clickHandler.handleClick);
```

이 예에서는 message 속성과 handleClick 메소드를 포함하느 Handler 객체가 있습니다.

handleClick 메소드를 DOM 버튼에 할당하였고, 클릭에 대응하여 실행될 것입니다. 버튼이 클릭될 때, handleClick 메소드는 콘솔 메시지를 호출할 것입니다. console.log는 `click event handler`를 출력해야하지만 실제로는 `undefined`를 출력합니다.

`undefined`를 표시하는 이유는 clickHandler의 실행 컨텍스트 때문입니다. handleClick 메소드는 저장되지 않아 `this`는 버튼 `btn`객체를 가리키고 있습니다. 우리는 바인드 메소드를 사용하여 이것을 고칠 수 있습니다.

```javascript
var clickHandler = {
	message: 'click event handler',
	handleClick: function(event) {
		console.log(this.message);
	}
};

var btn = document.getElementById('myBtn');
// 이벤트를 버튼에 추가하고 clickhHandler를 객체 클릭에 바인딩
btn.addEventListener('click', clickHandler.handleClick.bind(clickHandler));
```

`bind` method is available to all the function similar to call and apply method which take argument value of `this`.

`bind` 메소드는 이와 모든 비슷한 함수에 적용이 가능하고 `this`를 가지는 메소드를 적용할 수 있습니다.

</details>

# 코딩 질문들

## 참조로 값을 넘겨주기 vs 값으로 값을 넘겨주기
Js 개발자들에게 참조로 값을 넘겨주는 것과 값으로 값을 넘겨주는 것을 이해하는 것은 매우 중요합니다. 배열을 포함해 객체들은 참조로서 값을 넘겨주고 문자열, 불리언, 숫자형은 값으로 넘겨준다는 것을 기억합시다.

### 1. 다음 코드의 결과는 무엇일까요?

```javascript
var strA = "hi there";
var strB = strA;
strB="bye there!";
console.log (strA)
```

<details><summary><b>정답</b></summary>

결과는 `'hi there'` 이 될 것입니다. 왜냐하면 문자열은 값으로 넘겨지고 복사되기 때문입니다. 

</details>

###  2. 다음 코드의 결과는 무엇일까요?
```javascript
var objA = {prop1: 42};
var objB = objA; 
objB.prop1 = 90;
console.log(objA) 
```

<details><summary><b>정답</b></summary>

이 결과는 `{prop1: 90}`입니다. 왜냐하면 객체는 참조로써 넘겨지고 즉, 이말은 `objA`와 `objB`가 메모리에서 동일한 객체를 가리키고 있다는 뜻이 됩니다. 

</details>

###  3. 다음 코드의 결과는 무엇일까요?

```javascript
var objA = {prop1: 42};
var objB = objA;
objB = {};
console.log(objA)
```

<details><summary><b>정답</b></summary>

이 결과는 `{prop1: 42}`입니다.

우리가 `objB`에 `objA`를 할당할 때, `objB`변수는 `objB`변수로써 동일한 객체를 가리킬 것입니다.

그러나 우리가 다시 `objB` 에 빈 객체를 재할당 할 때, 우리는 단순하게 `objB` 변수가 참조하는 곳을 바꾼다고 생각할 수 있습니다.

하지만 이것은 `objA`에게 영향을 끼치지 않습니다.

</details>

###  4. 다음 코드의 결과는 무엇일까요?

```javascript
var arrA = [0,1,2,3,4,5];
var arrB = arrA;
arrB[0]=42;
console.log(arrA)
```

<details><summary><b>정답</b></summary>


이것의 결과는 `[42,1,2,3,4,5]` 입니다. 

배열은 자바스크립트 안의 객체이고 그것들은 참조에의해 값이 할당되고 넘겨집니다. 이것이 `arrA`와 `arrB`가 동일한 배열 `[0,1,2,3,4,5]` 를 가리키고 있는 이유이기도 합니다. 따라서 `arrB` 의 첫번째 원소가 바뀌면 `arrA`또한 바뀌게 됩니다. 이것이 메모리의 동일한 배열이기 떄문입니다.

</details>

###  5. What would be the output of following code?
```javascript
var arrA = [0,1,2,3,4,5];
var arrB = arrA.slice();
arrB[0]=42;
console.log(arrA)
```


<details><summary><b>Answer</b></summary>

The output will be `[0,1,2,3,4,5]`. 

The `slice` function copies all the elements of the array returning the new array. That's why
`arrA` and `arrB` reference two completely different arrays. 

</details>

###  6. What would be the output of following code?

```javascript
var arrA = [{prop1: "value of array A!!"},  {someProp: "also value of array A!"}, 3,4,5];
var arrB = arrA;
arrB[0].prop1=42;
console.log(arrA);
```

<details><summary><b>Answer</b></summary>

The output will be `[{prop1: 42},  {someProp: "also value of array A!"}, 3,4,5]`. 

Arrays are object in JS, so both varaibles arrA and arrB point to the same array. Changing
`arrB[0]` is the same as changing `arrA[0]`

</details>

### 7. What would be the output of following code?

```javascript
var arrA = [{prop1: "value of array A!!"}, {someProp: "also value of array A!"},3,4,5];
var arrB = arrA.slice();
arrB[0].prop1=42;
arrB[3] = 20;
console.log(arrA);
```

<details><summary><b>Answer</b></summary>

The output will be `[{prop1: 42},  {someProp: "also value of array A!"}, 3,4,5]`. 

The `slice` function copies all the elements of the array returning the new array. However,
it doesn't do deep copying. Instead it does shallow copying. You can imagine slice implemented like this: 

 ```javascript
function slice(arr) {
    var result = [];
    for (i = 0; i< arr.length; i++) {
        result.push(arr[i]);
    }
    return result; 
}
 ```

Look at the line with `result.push(arr[i])`. If `arr[i]` happens to be a number or string, 
it will be passed by value, in other words, copied. If `arr[i]` is an object, it will be passed by reference. 

In case of our array `arr[0]` is an object `{prop1: "value of array A!!"}`. Only the reference
to this object will be copied. This effectively means that arrays arrA and arrB share first
two elements. 

This is why changing the property of `arrB[0]` in `arrB` will also change the `arrA[0]`.

</details>

## Hoisting 

### 1. console.log(employeeId);

1.  Some Value
2.  Undefined 
3.  Type Error
4.  ReferenceError: employeeId is not defined 

<details><summary><b>Answer</b></summary>

 4) ReferenceError: employeeId is not defined 

</details>

###  2. What would be the output of following code?

```javascript
console.log(employeeId);
var employeeId = '19000';
```

1.  Some Value
2.  undefined 
3.  Type Error
4.  ReferenceError: employeeId is not defined 

<details><summary><b>Answer</b></summary>

 2) undefined 

</details>

### 3. What would be the output of following code?

```javascript
var employeeId = '1234abe';
(function(){
	console.log(employeeId);
	var employeeId = '122345';
})();
```

1.  '122345'
2.  undefined 
3.  Type Error
4.  ReferenceError: employeeId is not defined 

<details><summary><b>Answer</b></summary>

 2) undefined 

</details>

### 4. What would be the output of following code?

```javascript
var employeeId = '1234abe';
(function() {
	console.log(employeeId);
	var employeeId = '122345';
	(function() {
		var employeeId = 'abc1234';
	}());
}());
```

1.  '122345'
2.  undefined 
3.  '1234abe'
4.  ReferenceError: employeeId is not defined 

<details><summary><b>Answer</b></summary>

 2) undefined 

</details>

### 5. What would be the output of following code?

```javascript
(function() {
	console.log(typeof displayFunc);
	var displayFunc = function(){
		console.log("Hi I am inside displayFunc");
	}
}());
```

1.  undefined
2.  function 
3.  'Hi I am inside displayFunc'
4.  ReferenceError: displayFunc is not defined 

<details><summary><b>Answer</b></summary>

 1) undefined 

</details>

### 6. What would be the output of following code?

```javascript
var employeeId = 'abc123';
function foo(){
	employeeId = '123bcd';
	return;
}
foo();
console.log(employeeId);
```

1.  undefined
2.  '123bcd' 
3.  'abc123'
4.  ReferenceError: employeeId is not defined 

<details><summary><b>Answer</b></summary>

 2) '123bcd' 

</details>

### 7. What would be the output of following code?

```javascript
var employeeId = 'abc123';

function foo() {
	employeeId = '123bcd';
	return;

	function employeeId() {}
}
foo();
console.log(employeeId);
```

1.  undefined
2.  '123bcd' 
3.  'abc123'
4.  ReferenceError: employeeId is not defined 

<details><summary><b>Answer</b></summary>

 3) 'abc123' 

</details>

### 8. What would be the output of following code?

```javascript
var employeeId = 'abc123';

function foo() {
	employeeId();
	return;

	function employeeId() {
		console.log(typeof employeeId);
	}
}
foo();
```

1.  undefined
2.  function 
3.  string
4.  ReferenceError: employeeId is not defined 

<details><summary><b>Answer</b></summary>

 2) 'function'

</details>

### 9. What would be the output of following code?

```javascript
function foo() {
	employeeId();
	var product = 'Car'; 
	return;

	function employeeId() {
		console.log(product);
	}
}
foo();
```

1.  undefined
2.  Type Error 
3.  'Car'
4.  ReferenceError: product is not defined 

<details><summary><b>Answer</b></summary>

 1) undefined

</details>

### 10. What would be the output of following code?

```javascript
(function foo() {
	bar();

	function bar() {
		abc();
		console.log(typeof abc);
	}

	function abc() {
		console.log(typeof bar);
	}
}());
```

1.  undefined undefined
2.  Type Error 
3.  function function
4.  ReferenceError: bar is not defined 

<details><summary><b>Answer</b></summary>

 3) function function

</details>

## Objects

### 1. What would be the output of following code ?

```javascript
(function() {
	'use strict';

	var person = {
		name: 'John'
	};
	person.salary = '10000$';
	person['country'] = 'USA';

	Object.defineProperty(person, 'phoneNo', {
		value: '8888888888',
		enumerable: true
	})

	console.log(Object.keys(person)); 
})();
```
1.  Type Error
2.  undefined 
3.  ["name", "salary", "country", "phoneNo"]
4.  ["name", "salary", "country"]
	
<details><summary><b>Answer</b></summary>

 3) ["name", "salary", "country", "phoneNo"]

</details>

### 2. What would be the output of following code ?

```javascript
(function() {
	'use strict';

	var person = {
		name: 'John'
	};
	person.salary = '10000$';
	person['country'] = 'USA';

	Object.defineProperty(person, 'phoneNo', {
		value: '8888888888',
		enumerable: false
	})

	console.log(Object.keys(person)); 
})();
```
1.  Type Error
2.  undefined 
3.  ["name", "salary", "country", "phoneNo"]
4.  ["name", "salary", "country"]
	
<details><summary><b>Answer</b></summary>

 4) ["name", "salary", "country"]

</details>

### 3. What would be the output of following code ?

```javascript
(function() {
	var objA = {
		foo: 'foo',
		bar: 'bar'
	};
	var objB = {
		foo: 'foo',
		bar: 'bar'
	};
	console.log(objA == objB);
	console.log(objA === objB);
}());
```
1.  false true
2.  false false 
3.  true false
4.  true true
	
<details><summary><b>Answer</b></summary>

 2) false false

</details>

### 4. What would be the output of following code ?

```javascript
(function() {
	var objA = new Object({foo: "foo"});
	var objB = new Object({foo: "foo"});
	console.log(objA == objB);
	console.log(objA === objB);
}());
```
1.  false true
2.  false false 
3.  true false
4.  true true
	
<details><summary><b>Answer</b></summary>

 2) false false

</details>

### 5. What would be the output of following code ?

```javascript
(function() {
	var objA = Object.create({
		foo: 'foo'
	});
	var objB = Object.create({
		foo: 'foo'
	});
	console.log(objA == objB);
	console.log(objA === objB);
}());
```
1.  false true
2.  false false 
3.  true false
4.  true true
	
<details><summary><b>Answer</b></summary>

 2) false false

</details>

### 6. What would be the output of following code ?

```javascript
(function() {
	var objA = Object.create({
		foo: 'foo'
	});
	var objB = Object.create(objA);
	console.log(objA == objB);
	console.log(objA === objB);
}());
```
1.  false true
2.  false false 
3.  true false
4.  true true
	
<details><summary><b>Answer</b></summary>

 2) false false

</details>

### 7. What would be the output of following code ?

```javascript
(function() {
	var objA = Object.create({
		foo: 'foo'
	});
	var objB = Object.create(objA);
	console.log(objA.toString() == objB.toString());
	console.log(objA.toString() === objB.toString());
}());
```
1.  false true
2.  false false 
3.  true false
4.  true true
	
<details><summary><b>Answer</b></summary>

 4) true true

</details>

### 8. What would be the output of following code ?

```javascript
(function() {
	var objA = Object.create({
		foo: 'foo'
	});
	var objB = objA;
	console.log(objA == objB);
	console.log(objA === objB);
	console.log(objA.toString() == objB.toString());
	console.log(objA.toString() === objB.toString());
}());
```
1.  true true true false
2.  true false true true 
3.  true true true true
4.  true true false false
	
<details><summary><b>Answer</b></summary>

 3) true true true true

</details>

### 9. What would be the output of following code ?

```javascript
(function() {
	var objA = Object.create({
		foo: 'foo'
	});
	var objB = objA;
	objB.foo = 'bar';
	console.log(objA.foo);
	console.log(objB.foo);
}());
```
1.  foo bar
2.  bar bar 
3.  foo foo
4.  bar foo
	
<details><summary><b>Answer</b></summary>

 2) bar bar

</details>

### 10. What would be the output of following code ?

```javascript
(function() {
	var objA = Object.create({
		foo: 'foo'
	});
	var objB = objA;
	objB.foo = 'bar';

	delete objA.foo;
	console.log(objA.foo);
	console.log(objB.foo);
}());
```
1.  foo bar
2.  bar bar 
3.  foo foo
4.  bar foo
	
<details><summary><b>Answer</b></summary>

 3) foo foo

</details>

### 11. What would be the output of following code ?

```javascript
(function() {
	var objA = {
		foo: 'foo'
	};
	var objB = objA;
	objB.foo = 'bar';

	delete objA.foo;
	console.log(objA.foo);
	console.log(objB.foo);
}());
```
1.  foo bar
2.  undefined undefined 
3.  foo foo
4.  undefined bar
	
<details><summary><b>Answer</b></summary>

 2) undefined undefined

</details>

## Arrays

### 1. What would be the output of following code?

```javascript
(function() {
	var array = new Array('100');
	console.log(array);
	console.log(array.length);
}());
```

1.  undefined undefined
2.  [undefined × 100] 100 
3.  ["100"] 1
4.  ReferenceError: array is not defined 

<details><summary><b>Answer</b></summary>

 3) ["100"] 1

</details>

### 2. What would be the output of following code?

```javascript
(function() {
	var array1 = [];
	var array2 = new Array(100);
	var array3 = new Array(['1',2,'3',4,5.6]);
	console.log(array1);
	console.log(array2);
	console.log(array3);
	console.log(array3.length);
}());
```

1.  [] [] [Array[5]] 1
2.  [] [undefined × 100] Array[5] 1
3.  [] [] ['1',2,'3',4,5.6] 5
4.  [] [] [Array[5]] 5 

<details><summary><b>Answer</b></summary>

 1) [] [] [Array[5]] 1

</details>

### 3. What would be the output of following code?

```javascript
(function () {
  var array = new Array('a', 'b', 'c', 'd', 'e');
  array[10] = 'f';
  delete array[10];
  console.log(array.length);
}());
```

1.  11
2.  5
3.  6
4.  undefined

<details><summary><b>Answer</b></summary>

 1) 11

</details>

### 4. What would be the output of following code?

```javascript
(function(){
	var animal = ['cow','horse'];
		animal.push('cat');
		animal.push('dog','rat','goat');
		console.log(animal.length);
})();
```

1.  4
2.  5
3.  6
4.  undefined

<details><summary><b>Answer</b></summary>

 3) 6

</details>

### 5. What would be the output of following code?

```javascript
(function(){
	var animal = ['cow','horse'];
		animal.push('cat');
		animal.unshift('dog','rat','goat');
		console.log(animal);
})();
```

1.  [ 'dog', 'rat', 'goat', 'cow', 'horse', 'cat' ]
2.  [ 'cow', 'horse', 'cat', 'dog', 'rat', 'goat' ]
3.  Type Error
4.  undefined

<details><summary><b>Answer</b></summary>

 1) [ 'dog', 'rat', 'goat', 'cow', 'horse', 'cat' ]

</details>

### 6. What would be the output of following code?

```javascript
(function(){
	var array = [1,2,3,4,5];
	console.log(array.indexOf(2));
	console.log([{name: 'John'},{name : 'John'}].indexOf({name:'John'}));
	console.log([[1],[2],[3],[4]].indexOf([3]));
	console.log("abcdefgh".indexOf('e'));
})();
```

1.  1 -1 -1 4
2.  1 0 -1 4
3.  1 -1 -1 -1
4.  1 undefined -1 4

<details><summary><b>Answer</b></summary>

 1) 1 -1 -1 4

</details>

### 7. What would be the output of following code?

```javascript
(function(){
	var array = [1,2,3,4,5,1,2,3,4,5,6];
	console.log(array.indexOf(2));
	console.log(array.indexOf(2,3));
	console.log(array.indexOf(2,10));
})();
```

1.  1 -1 -1
2.  1 6 -1
3.  1 1 -1 
4.  1 undefined undefined

<details><summary><b>Answer</b></summary>

 2) 1 6 -1

</details>

### 8. What would be the output of following code?

```javascript
(function(){
	var numbers = [2,3,4,8,9,11,13,12,16];
	var even = numbers.filter(function(element, index){
		return element % 2 === 0; 
	});
	console.log(even);

	var containsDivisibleby3 = numbers.some(function(element, index){
		return element % 3 === 0;
	});

	console.log(containsDivisibleby3);	
})();
```

1.  [ 2, 4, 8, 12, 16 ] [ 0, 3, 0, 0, 9, 0, 12]
2.  [ 2, 4, 8, 12, 16 ] [ 3, 9, 12]
3.  [ 2, 4, 8, 12, 16 ] true 
4.  [ 2, 4, 8, 12, 16 ] false

<details><summary><b>Answer</b></summary>

 3) [ 2, 4, 8, 12, 16 ] true 

</details>

### 9. What would be the output of following code?

```javascript
(function(){
	var containers = [2,0,false,"", '12', true];
	var containers = containers.filter(Boolean);
	console.log(containers);
	var containers = containers.filter(Number);
	console.log(containers);
	var containers = containers.filter(String);
	console.log(containers);
	var containers = containers.filter(Object);
	console.log(containers);		
})();
```

1.	[ 2, '12', true ]
	[ 2, '12', true ]
	[ 2, '12', true ]
	[ 2, '12', true ]
2.	[false, true]
	[ 2 ]
	['12']
	[ ]
3.	[2,0,false,"", '12', true]
	[2,0,false,"", '12', true]
	[2,0,false,"", '12', true]
	[2,0,false,"", '12', true]
4. [ 2, '12', true ]
	[ 2, '12', true, false ]
	[ 2, '12', true,false ]
	[ 2, '12', true,false]


<details><summary><b>Answer</b></summary>

 1) [ 2, '12', true ]
			 [ 2, '12', true ]
			 [ 2, '12', true ]
			 [ 2, '12', true ]
			 
</details>

### 10. What would be the output of following code?

```javascript
(function(){
	var list = ['foo','bar','john','ritz'];
	    console.log(list.slice(1));	
	    console.log(list.slice(1,3));
	    console.log(list.slice());
	    console.log(list.slice(2,2));
	    console.log(list);				
})();
```

1. [ 'bar', 'john', 'ritz' ]
   [ 'bar', 'john' ]
   [ 'foo', 'bar', 'john', 'ritz' ]
   []
   [ 'foo', 'bar', 'john', 'ritz' ]
2. [ 'bar', 'john', 'ritz' ]
   [ 'bar', 'john','ritz ]
   [ 'foo', 'bar', 'john', 'ritz' ]
   []
   [ 'foo', 'bar', 'john', 'ritz' ]
3. [ 'john', 'ritz' ]
   [ 'bar', 'john' ]
   [ 'foo', 'bar', 'john', 'ritz' ]
   []
   [ 'foo', 'bar', 'john', 'ritz' ]
4. [ 'foo' ]
   [ 'bar', 'john' ]
   [ 'foo', 'bar', 'john', 'ritz' ]
   []
   [ 'foo', 'bar', 'john', 'ritz' ]

<details><summary><b>Answer</b></summary>

 1) [ 'bar', 'john', 'ritz' ]
		 	 [ 'bar', 'john' ]
           [ 'foo', 'bar', 'john', 'ritz' ]
           []
           [ 'foo', 'bar', 'john', 'ritz' ]		

</details>

### 11. What would be the output of following code?

```javascript
(function(){
	var list = ['foo','bar','john'];
	    console.log(list.splice(1));		
	    console.log(list.splice(1,2));
	    console.log(list);			
})();
```

1.  [ 'bar', 'john' ] [] [ 'foo' ]
2.  [ 'bar', 'john' ] [] [ 'bar', 'john' ]
3.  [ 'bar', 'john' ] [ 'bar', 'john' ] [ 'bar', 'john' ]
4.  [ 'bar', 'john' ] [] []

<details><summary><b>Answer</b></summary>

 1.  [ 'bar', 'john' ] [] [ 'foo' ] 

</details>

### 12. What would be the output of following code?

```javascript
(function(){
	var arrayNumb = [2, 8, 15, 16, 23, 42];
	arrayNumb.sort();
	console.log(arrayNumb);
})();
```

1.  [2, 8, 15, 16, 23, 42]
2.  [42, 23, 26, 15, 8, 2]
3.  [ 15, 16, 2, 23, 42, 8 ]
4.  [ 2, 8, 15, 16, 23, 42 ]

<details><summary><b>Answer</b></summary>

 3.  [ 15, 16, 2, 23, 42, 8 ]

</details>

## Functions

### 1. What would be the output of following code ?

```javascript
function funcA(){
	console.log("funcA ", this);
	(function innerFuncA1(){
		console.log("innerFunc1", this);
		(function innerFunA11(){
			console.log("innerFunA11", this);
		})();
	})();
}
	
console.log(funcA());
```

1.  funcA  Window {...} 
    innerFunc1 Window {...} 
    innerFunA11 Window {...}
2.  undefined 
3.  Type Error
4.  ReferenceError: this is not defined 
	
<details><summary><b>Answer</b></summary>

 1) 

</details>

### 2. What would be the output of following code ?

```javascript
var obj = {
	message: "Hello",
	innerMessage: !(function() {
		console.log(this.message);
	})()
};
	
console.log(obj.innerMessage);
```

1.  ReferenceError: this.message is not defined 
2.  undefined 
3.  Type Error
4.  undefined true
	
<details><summary><b>Answer</b></summary>

 4) undefined true

</details>

### 3. What would be the output of following code ?

```javascript
var obj = {
	message: "Hello",
	innerMessage: function() {
		return this.message;
	}
};
	
console.log(obj.innerMessage());
```

1.  Hello 
2.  undefined 
3.  Type Error
4.  ReferenceError: this.message is not defined
	
<details><summary><b>Answer</b></summary>

 1) Hello

</details>

### 4. What would be the output of following code ?

```javascript
var obj = {
  message: 'Hello',
  innerMessage: function () {
    (function () {
      console.log(this.message);
    }());
  }
};
console.log(obj.innerMessage());
```

1.  Type Error 
2.  Hello 
3.  undefined
4.  ReferenceError: this.message is not defined
	
<details><summary><b>Answer</b></summary>

 3) undefined
	
</details>

### 5. What would be the output of following code ?

```javascript
var obj = {
  message: 'Hello',
  innerMessage: function () {
  	var self = this;
    (function () {
      console.log(self.message);
    }());
  }
};
console.log(obj.innerMessage());
```

1.  Type Error 
2.  'Hello' 
3.  undefined
4.  ReferenceError: self.message is not defined
	
<details><summary><b>Answer</b></summary>

 2) 'Hello'

</details>

### 6. What would be the output of following code ?

```javascript
function myFunc(){
	console.log(this.message);
}
myFunc.message = "Hi John";
	
console.log(myFunc());
```

1.  Type Error 
2.  'Hi John' 
3.  undefined
4.  ReferenceError: this.message is not defined
	
<details><summary><b>Answer</b></summary>

 3) undefined

</details>

### 7. What would be the output of following code ?

```javascript
function myFunc(){
	console.log(myFunc.message);
}
myFunc.message = "Hi John";
	
console.log(myFunc());
```

1.  Type Error 
2.  'Hi John' 
3.  undefined
4.  ReferenceError: this.message is not defined
	
<details><summary><b>Answer</b></summary>

 2) 'Hi John'

</details>

### 8. What would be the output of following code ?

```javascript
function myFunc() {
  myFunc.message = 'Hi John';
  console.log(myFunc.message);
}
console.log(myFunc());
```

1.  Type Error 
2.  'Hi John' 
3.  undefined
4.  ReferenceError: this.message is not defined
	
<details><summary><b>Answer</b></summary>

 2) 'Hi John'

</details>

### 9. What would be the output of following code ?

```javascript
function myFunc(param1,param2) {
  console.log(myFunc.length);
}
console.log(myFunc());
console.log(myFunc("a","b"));
console.log(myFunc("a","b","c","d"));
```

1.  2 2 2 
2.  0 2 4
3.  undefined
4.  ReferenceError
	
<details><summary><b>Answer</b></summary>

 a) 2 2 2 

</details>

### 10. What would be the output of following code ?

```javascript
function myFunc() {
  console.log(arguments.length);
}
console.log(myFunc());
console.log(myFunc("a","b"));
console.log(myFunc("a","b","c","d"));
```

1.  2 2 2 
2.  0 2 4
3.  undefined
4.  ReferenceError
	
<details><summary><b>Answer</b></summary>

 2) 0 2 4 

</details>

## Object Oriented

### 1. What would be the output of following code ?

```javascript
function Person(name, age){
	this.name = name || "John";
	this.age = age || 24;
	this.displayName = function(){
		console.log(this.name);
	}
}

Person.name = "John";
Person.displayName = function(){
	console.log(this.name);
}

var person1 = new Person('John');
	person1.displayName();
	Person.displayName();
```

1.  John Person
2.  John John
3.  John undefined
4.  John John
	
<details><summary><b>Answer</b></summary>

 1) John Person 

</details>

## Scopes

### 1. What would be the output of following code ?

```javascript
function passWordMngr() {
	var password = '12345678';
	this.userName = 'John';
	return {
		pwd: password
	};
}
// Block End
var userInfo = passWordMngr();
console.log(userInfo.pwd);
console.log(userInfo.userName);
```

1.  12345678 Window
2.  12345678 John
3.  12345678 undefined
4.  undefined undefined
	
<details><summary><b>Answer</b></summary>

 3) 12345678 undefined 

</details>

### 2. What would be the output of following code ?

```javascript
var employeeId = 'aq123';
function Employee() {
  this.employeeId = 'bq1uy';
}
console.log(Employee.employeeId);
```

1.  Reference Error
2.  aq123
3.  bq1uy
4.  undefined
	
<details><summary><b>Answer</b></summary>

 4) undefined 

</details>

### 3. What would be the output of following code ?

```javascript
var employeeId = 'aq123';

function Employee() {
	this.employeeId = 'bq1uy';
}
console.log(new Employee().employeeId);
Employee.prototype.employeeId = 'kj182';
Employee.prototype.JobId = '1BJKSJ';
console.log(new Employee().JobId);
console.log(new Employee().employeeId);
```

1.  bq1uy 1BJKSJ bq1uy undefined
2.  bq1uy 1BJKSJ bq1uy
3.  bq1uy 1BJKSJ kj182
4.  undefined 1BJKSJ kj182
	
<details><summary><b>Answer</b></summary>

 2) bq1uy 1BJKSJ bq1uy 

</details>

### 4. What would be the output of following code ?

```javascript
var employeeId = 'aq123';
(function Employee() {
	try {
		throw 'foo123';
	} catch (employeeId) {
		console.log(employeeId);
	}
	console.log(employeeId);
}());
```

1.  foo123 aq123
2.  foo123 foo123
3.  aq123 aq123
4.  foo123 undefined 
	
<details><summary><b>Answer</b></summary>

 1) foo123 aq123 

</details>

## Call, Apply, Bind

### 1. What would be the output of following code ?

```javascript
(function() {
	var greet = 'Hello World';
	var toGreet = [].filter.call(greet, function(element, index) {
		return index > 5;
	});
	console.log(toGreet);
}());
```

1.  Hello World
2.  undefined
3.  World
4.  [ 'W', 'o', 'r', 'l', 'd' ] 
	
<details><summary><b>Answer</b></summary>

 4) [ 'W', 'o', 'r', 'l', 'd' ]  

</details>

### 2. What would be the output of following code ?

```javascript
(function() {
	var fooAccount = {
		name: 'John',
		amount: 4000,
		deductAmount: function(amount) {
			this.amount -= amount;
			return 'Total amount left in account: ' + this.amount;
		}
	};
	var barAccount = {
		name: 'John',
		amount: 6000
	};
	var withdrawAmountBy = function(totalAmount) {
		return fooAccount.deductAmount.bind(barAccount, totalAmount);
	};
	console.log(withdrawAmountBy(400)());
	console.log(withdrawAmountBy(300)());
}());
```

1. Total amount left in account: 5600 Total amount left in account: 5300
2.  undefined undefined
3.  Total amount left in account: 3600 Total amount left in account: 3300
4.  Total amount left in account: 5600 Total amount left in account: 5600
	
<details><summary><b>Answer</b></summary>

 1) Total amount left in account: 5600 Total amount left in account: 5300 

</details>

### 3. What would be the output of following code ?

```javascript
(function() {
	var fooAccount = {
		name: 'John',
		amount: 4000,
		deductAmount: function(amount) {
			this.amount -= amount;
			return this.amount;
		}
	};
	var barAccount = {
		name: 'John',
		amount: 6000
	};
	var withdrawAmountBy = function(totalAmount) {
		return fooAccount.deductAmount.apply(barAccount, [totalAmount]);
	};
	console.log(withdrawAmountBy(400));
	console.log(withdrawAmountBy(300));
	console.log(withdrawAmountBy(200));
}());
```

1. 5600 5300 5100
2. 3600 3300 3100
3. 5600 3300 5100
4. undefined undefined undefined
	
<details><summary><b>Answer</b></summary>

 1) 5600 5300 5100

</details>

### 4. What would be the output of following code ?

```javascript
(function() {
	var fooAccount = {
		name: 'John',
		amount: 6000,
		deductAmount: function(amount) {
			this.amount -= amount;
			return this.amount;
		}
	};
	var barAccount = {
		name: 'John',
		amount: 4000
	};
	var withdrawAmountBy = function(totalAmount) {
		return fooAccount.deductAmount.call(barAccount, totalAmount);
	};
	console.log(withdrawAmountBy(400));
	console.log(withdrawAmountBy(300));
	console.log(withdrawAmountBy(200));
}());
```

1. 5600 5300 5100
2. 3600 3300 3100
3. 5600 3300 5100
4. undefined undefined undefined
	
<details><summary><b>Answer</b></summary>

 2) 3600 3300 3100

</details>

### 5. What would be the output of following code ?

```javascript
(function greetNewCustomer() {
	console.log('Hello ' + this.name);
}.bind({
	name: 'John'
})());
```

1. Hello John
2. Reference Error
3. Window
4. undefined
	
<details><summary><b>Answer</b></summary>

 1) Hello John

</details>

### 6. Suggest your question!


## Callback Functions

### 1. What would be the output of following code ?

```javascript
function getDataFromServer(apiUrl){
    var name = "John";
	return {
		then : function(fn){
			fn(name);
		}
	}
}

getDataFromServer('www.google.com').then(function(name){
	console.log(name);
});

```

1. John
2. undefined
3. Reference Error
4. fn is not defined
	
<details><summary><b>Answer</b></summary>

 1) John

</details>

### 2. What would be the output of following code ?

```javascript
(function(){
	var arrayNumb = [2, 8, 15, 16, 23, 42];
	Array.prototype.sort = function(a,b){
		return a - b;
	};
	arrayNumb.sort();
	console.log(arrayNumb);
})();

(function(){
	var numberArray = [2, 8, 15, 16, 23, 42];
	numberArray.sort(function(a,b){
		if(a == b){
			return 0;
		}else{
			return a < b ? -1 : 1;
		}
	});
	console.log(numberArray);
})();

(function(){
	var numberArray = [2, 8, 15, 16, 23, 42];
	numberArray.sort(function(a,b){
		return a-b;
	});
	console.log(numberArray);
})();
```

1. [ 2, 8, 15, 16, 23, 42 ]
   [ 2, 8, 15, 16, 23, 42 ]
   [ 2, 8, 15, 16, 23, 42 ]
2. undefined undefined undefined
3. [42, 23, 16, 15, 8, 2]
   [42, 23, 16, 15, 8, 2]
   [42, 23, 16, 15, 8, 2]
4. Reference Error
	
<details><summary><b>Answer</b></summary>

 1) [ 2, 8, 15, 16, 23, 42 ]
			 [ 2, 8, 15, 16, 23, 42 ]
			 [ 2, 8, 15, 16, 23, 42 ]

</details>
			
## Return Statement

### 1. What would be the output of following code ?

```javascript
(function(){
	function sayHello(){
		var name = "Hi John";
		return 
		{
			fullName: name
		}
	}
	console.log(sayHello().fullName);
})();
```

1. Hi John
2. undefined
3. Reference Error
4. Uncaught TypeError: Cannot read property 'fullName' of undefined
	
<details><summary><b>Answer</b></summary>

 4) Uncaught TypeError: Cannot read property 'fullName' of undefined

</details>

### 2. What would be the output of following code ?

```javascript
function getNumber(){
	return (2,4,5);
}

var numb = getNumber();
console.log(numb);
```

1. 5
2. undefined
3. 2
4. (2,4,5)
	
<details><summary><b>Answer</b></summary>

 1) 5

</details>

### 3. What would be the output of following code ?

```javascript
function getNumber(){
	return;
}

var numb = getNumber();
console.log(numb);
```

1. null
2. undefined
3. ""
4. 0
	
<details><summary><b>Answer</b></summary>

 2) undefined

</details>

### 4**. What would be the output of following code ?

```javascript
function mul(x){
	return function(y){
		return [x*y, function(z){
			return x*y + z;
		}];
	}
}

console.log(mul(2)(3)[0]);
console.log(mul(2)(3)[1](4));
```

1. 6, 10
2. undefined undefined
3. Reference Error
4. 10, 6
	
<details><summary><b>Answer</b></summary>

 1) 6, 10

</details>

### 5**. What would be the output of following code ?

```javascript
function mul(x) {
	return function(y) {
		return {
			result: x * y,
			sum: function(z) {
				return x * y + z;
			}
		};
	};
}
console.log(mul(2)(3).result);
console.log(mul(2)(3).sum(4));
```

1. 6, 10
2. undefined undefined
3. Reference Error
4. 10, 6
	
<details><summary><b>Answer</b></summary>

 1) 6, 10

</details>

### 6. What would be the output of following code ?

```javascript
function mul(x) {
	return function(y) {
		return function(z) {
			return function(w) {
				return function(p) {
					return x * y * z * w * p;
				};
			};
		};
	};
}
console.log(mul(2)(3)(4)(5)(6));
```

1. 720
2. undefined
3. Reference Error
4. Type Error
	
<details><summary><b>Answer</b></summary>

 1) 720

</details>

### 7. What would be the output of following code ?

```javascript
function getName1(){
	console.log(this.name);
}

Object.prototype.getName2 = () =>{
	console.log(this.name)
}

let personObj = {
	name:"Tony",
	print:getName1
}

personObj.print();
personObj.getName2();
```

1. undefined undefined
2. Tony undefined
3. undefined Tony
4. Tony Tony

<details><summary><b>Answer</b></summary>

 2) Tony undefined

Explaination: **getName1()** function works fine because it's being called from ***personObj***, so it has access to *this.name* property. But when while calling **getnName2** which is defined under *Object.prototype* doesn't have any proprty named *this.name*. There should be *name* property under prototype. Following is the code:

```javascript
function getName1(){
	console.log(this.name);
}

Object.prototype.getName2 = () =>{
  console.log(Object.getPrototypeOf(this).name);
}

let personObj = {
	name:"Tony",
	print:getName1
}

personObj.print();
Object.prototype.name="Steve";
personObj.getName2();
```

</details>

## Contributing

We always appreciate your feedback on how the book can be improved, and more questions can be added. If you think you have some question then please add that and open a pull request. 


## License

This book is released under a Creative Commons Attribution-Noncommercial- No Derivative Works 3.0 United States License.

What this means it that the project is free to read and use, but the license does not permit commercial use of the material (i.e you can freely print out the questions for your own use, but you can't sell it). I'm trying to best to publish all of my books in a free + purchased (if you would like to support these projects) form so I would greatly appreciate it if you would respect these terms.

Copyright Iurii Katkov and Nishant Kumar, 2017. 
