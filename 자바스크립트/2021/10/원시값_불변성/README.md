## 자바스크립트 원시 값의 불변성(Primitive Type)

자바스크립트는 크게 **원시 타입**(primitive type)과 **객체 타입**(object/reference type)으로 구분할 수 있다.

자바스크립트에서 원시 값은 **변경 불가능한 값**(immutable value)이다. 반면에 객체는 **변경 가능 값**(mutable value)이다.

도대체 이게 무슨 말인지 헷갈려서, 이번 기회에 공부하면서 이해해보려고 한다.

### 원시 값

#### **변경 불가능한 값**

위에서 말했듯이, 원시 타입은 변경이 불가능하다.

처음에 이게 무슨 소리인지 이해가 안됐다. 분명히 나는 **변수**에 원시 값을 할당하고 나서 그 값을 바꿀 수 있는데 말이다.

```javascript
var a = 1; // 숫자 타입은 원시 타입
a = "Beautiful Night"; // 문자열 타입의 값으로 변경

console.log(a); // "Beautiful Night"
```

??? 원시 타입은 분명히 변경이 불가능하다고 했는데 값이 변경됐다.

내가 오해하고 있었다. 변수의 값이 변경된 것이지, 원시 타입 1이 'Beautiful Night' 이라는 문자열로 변경된 것이 아니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo0dIL%2FbtrgBDoOmV0%2FebDNSHXGo0yDMoXTYy5ubk%2Fimg.png)

위의 코드를 그림으로 표현하자면 이렇다. 숫자 타입인 1은 **변경 불가능한 값**이기 때문에, 1은 그대로 두고 **새로운 메모리를 할당**한다. 그리고 새로 할당된 메모리에 재할당한 값이 저장되고, 변수 a는 새로 할당된 메모리를 참조한다.

#### **값에 의한 전달**

원시타입이 값을 전달하면 어떻게 되는지에 대해서도 정리하려고 한다.

```javascript
var number = 10;
var copied = number;

console.log(number); // 10
console.log(copied); // 10

number = 20;

console.log(number); // 20
console.log(copied); // 10
```

변수에 변수를 할당할 때 해당 변수의 값이 원시 타입이라면, 변수의 값이 복사되어 전달된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcbz5Uc%2FbtrgDTRSIv0%2FEP10h6UEGgTNiqcZKEcT51%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPi2Wj%2FbtrgFTxiqwf%2FDedIdxk5GEsoMltOhNwum0%2Fimg.png)

그림으로는 copied 변수에 number 변수를 할당할 때, 각각 독립된 메모리에 저장된 것으로 표현했다.

하지만 실제로 ECMAScript 사양에 변수를 통해 메모리를 어떻게 관리하는지 명확하게 정의되어 있지 않다.

자바스크립트 엔진을 구현하는 제조사마다 구현 방식이 조금씩 다를 수 있기 때문에, 다른 방식으로 동작할 수 있기 때문이다.

예를 들면, copied에 number를 할당했을 때, 기존 number의 메모리를 같이 참조하다가, 다른 값으로 할당되는 시점에  새로운 메모리를 참조하도록 하는 방식이다. 아래 그림 같은 느낌이다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbmTrcd%2FbtrgAUEDSPp%2FrLz1BXJKwhNJNKfB5vVtI1%2Fimg.png)
