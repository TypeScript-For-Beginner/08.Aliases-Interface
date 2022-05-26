# 8. 타입 별칭 & 인터페이스(Type Aliases&Interface)

## 8.1 타입 별칭 (Type Aliases)

### 8.1.1 타입 별칭이란?

타입에 대한 별칭을 제공하며, 재사용 할 수 있다.
주의해야 할 부분은 타입 별칭은 정의한 타입을 참고할 수 있게 이름을 지어 주는 것이지, 새로운 타입을 생성하는 것이 아니라는 점이다.

### 8.1.2 타입 별칭 사용

`type 별칭 = 타입;`으로 정의한다.

> 예제 8-1

```tsx
// 타입만 사용
const food: string = "banana";
const food: string = 1; // Error!

// 타입 별칭을 사용
type Food = string;
const myFood: Food = "banana";
const myFood2: Food = 1; // Error!
```

> 예제 8-2

```tsx
// 문자열 타입
type Name = string;
const userName: Name = "Jade";

// 문자열 리터럴
type MyName = "Jade";
const userName: MyName = "jade"; // Error!

// 숫자 타입
type MyNumber = number;
const myNumber: MyNumber = 1;

// 유니온 타입
type MyText = string | number;

// 문자열 유니온
type YourText = "hello" | "bye";

// 숫자 리터럴 유니온
type MyNumber = 1 | 2 | 3 | 4 | 5 | 6;

// 객체 리터럴 유니온
type MyObject = { first: 1 } | { second: 2 };

// 함수 유니온
type MyFunction = (() => string) | (() => void);

// 인터페이스 유니온
interface A {
    a: number;
}
interface B {
    b: number;
}
type MyInterface = A | B;

// 튜플 타입
type MyTuple = [string, number];

// 제네릭 타입
type MyGenerics<T> = {
    name: T;
};
```

타입의 자리에는 원시값 이외에도 유니온, 튜플, 함수, 객체 등 인터페이스 레벨의 복잡한 타입이 올 수 있다.

> 예제 8-3

```tsx
// 객체 타입
type User = {
    name: string;
    age: number;
};

// 함수의 파라미터
function getUser(user: User) {}
let newUser: User = {
    name: "철수",
    age: 20,
};

getUser(newUser);

// 함수 타입
type AddNumber = (x: number, y: number) => number; // return 해주는 값이 없을땐 void
let addNumber: AddNumber = (x, y) => {
    return x + y;
};
```

타입 별칭은 `string`, `number`와 같은 원시 타입보다는 객체, 함수 등에 유용하게 사용된다.

## 8.2. 인터페이스 (Interface)

### 8.2.1 인터페이스란?

타입 별칭과 \*\*\*\*비슷하게 새로운 타입을 정의하는 또 다른 방법이다. 객체, 함수, 함수의 파라미터, 클래스 등에 사용할 수 있으며 상호 간에 정의한 약속 혹은 규칙을 의미한다.

`interface 인터페이스이름 { 속성: 타입; }` 으로 정의한다.

### 8.2.2 인터페이스 속성

**8.2.2.1 객체의 스펙**

> 예제 8-4

```tsx
// 객체의 스펙(속성과 속성의 타입)
interface User {
    name: string;
    age: number;
}

let user1: User = {
    name: "영희",
    age: 20,
};

let user2 = {} as User;
user1.name = "철수";
user1.age = 20;
```

**8.2.2.2 함수 타입**

> 예제 8-5

```tsx
// 함수의 스펙(파라미터, 반환타입 등)
interface AddNumber {
    (x: number, y: number): number; // return 해주는 값이 없을 땐 void
}

let addNumber: AddNumber = (x, y) => {
    return x + y;
};

addNumber(10, 10); // result: 20
```

> 예제 8-6

```tsx
// 함수의 파라미터
interface User {
    name: string;
    age: number;
}

function getAge(obj: User) {
    console.log(obj.age);
}

let person = {
    name: "철수",
    age: 20,
    gender: "male",
};

getAge(person); // result: 20
```

인터페이스를 인자로 사용할 때, 정의한 프로퍼티와 타입의 조건을 만족한다면, 인자로 받는 객체의 프로퍼티 개수나 순서는 같지 않아도 된다.

**8.2.2.3 옵션 속성**

인터페이스를 정의할 때 옵션 속성을 사용하면 그 속성을 사용하지 않아도 된다.

`속성?: 타입;` 과같이 속성 뒤에 `?`를 붙여 사용한다.

> 예제 8-7

```tsx
// 옵션 속성을 사용하지 않은 경우
interface User {
    name: string;
    age: number;
}

function getAge(obj: User) {
    console.log(obj.name);
    console.log(obj.age);
}

let person = {
    name: "철수",
};

getAge(person); // Error!
```

> 예제 8-8

```tsx
// 옵션 속성을 사용한 경우
interface User {
    name: string;
    age: number;
    gender?: "male" | "female";
}

function getAge(obj: User) {
    console.log(obj.name);
    console.log(obj.age);
}

let person = {
    name: "철수",
    age: 20,
};

getAge(person);
```

`getAge` 함수의 인자로 받는 `person`의 프로퍼티에는 `gender`가 없지만, 인터페이스에 이를 옵션 속성으로 정의했기 때문에 오류가 나지 않는다.

**8.2.2.4 readonly 읽기 전용**

인터페이스를 사용하여 객체를 생성할 때, 값을 할당한 후 변경할 수 없는 속성을 의미한다.

`readonly 속성: 타입;` 과같이 속성 앞에 `readonly` 를 붙여 사용한다.

> 예제 8-10

```tsx
// 읽기 전용 속성
interface User {
    readonly name: string;
}

let person: User = {
    name: "철수",
};

person.name = "영희"; // Error!
```

`ReadonlyArray<T>` 타입을 사용하여 읽기 전용 배열을 생성할 수 있다.

> 예제 8-11

```tsx
// 읽기 전용 배열
let Users: ReadonlyArray<string> = ["철수", "영희"];

Users.splice(0, 1); // Error!
Users.push("현지"); // Error!
Users[0] = "현지"; // Error!
```

**8.2.2.5 클래스 타입**

`implements` 로 미리 정의된 인터페이스를 채택하여 클래스를 정의할 수 있다.

인터페이스로 정의한 메서드나 프로퍼티들은 해당 클래스에 필수적으로 들어가야 한다.

> 예제 8-12

```tsx
interface User {
    name: string;
    age: number;
    getAge(age: number): void;
}

class newUser implements User {
    name: string = "철수";
    age: number = 20;
    getAge(age: number) {
        console.log(this.age);
    }
    constructor() {}
}

let user1 = new newUser();
user1; // result: {name: "철수", age: 20}
```

> 예제 8-13

```tsx
interface User {
    name: string;
    age: number;
    getAge(age: number): void;
}

class newUser implements User {
    constructor(public name: string, public age: number) {}
    getAge(age: number) {
        console.log(this.age);
    }
}

let user1 = new newUser("영희", 20);
user1; // result: {name: "영희", age: 20}
```

## 8.3 인터페이스와 타입 별칭의 차이

인터페이스와 타입 별칭의 가장 큰 차이는 타입의 확장 가능, 불가능 여부이다.

### 8.3.1 선언적 확장

> 예제 8-14

```tsx
interface User {
    name: string;
}

interface User {
    age: number;
} // 같은 이름으로 정의하면 자동으로 확장된다.
```

인터페이스는 새로운 속성을 추가하기 위해 같은 이름으로 재 정의하여 확장할 수 있다. 이를 선언적 확장이라고 한다.

> 예제 8-15

```tsx
type User = {
    name: string;
};

type User = {
    age: number;
}; // Error!
```

타입은 같은 이름으로 재 정의할 수 없다.

### 8.3.2 인터페이스의 확장

`extends` 를 사용하여 인터페이스 간 확장이 가능하다.

> 예제 8-16

```tsx
interface User {
    name: string;
}

interface UserInfo extends User {
    age: number;
}

let person: UserInfo = {
    name: "철수",
    age: 20,
};
```

> 예제 8-17

```tsx
interface User {
    name: string;
}

interface Age {
    age: number;
}

interface UserInfo extends User, Age {
    gender: string;
}

let newUser: UserInfo = {
    name: "영희",
    age: 20,
    gender: "female",
};
```

위 예제처럼 인터페이스를 여러 개 상속받을 수 있다.

> 예제 8-18

```tsx
// 인터페이스 -> 타입 확장
type User = {
    name: string;
    age: number;
};

interface UserInfo extends User {
    gender: string;
}

let newUser: UserInfo = {
    name: "영희",
    age: 20,
    gender: "female",
};

interface UserInfo2 extends string {} // Error!
```

인터페이스는 타입을 상속받을 수 있지만 리터럴 타입은 불가능하다. 또한 유니온 연산자를 사용한 타입은 `extends`, `implements` 할 수 없다.

### 8.3.3 타입의 확장

타입 별칭은 `extends` 대신 인터섹션(`&`)을 사용하여 확장할 수 있다. 인터페이스처럼 타입을 상속받아 확장하는 개념이 아닌, 새로운 타입을 정의하는 것이다.

> 예제 8-19

```tsx
// 타입의 확장
type User = {
    name: string;
};

type UserInfo = User & {
    age: number;
};

let newUser: UserInfo = {
    name: "영희",
    age: 20,
};
```

> 예제 8-20

```tsx
// 타입 -> 인터페이스 확장
interface User {
    name: string;
    age: number;
}

type UserInfo = User & {
    gender: string;
};

let newUser: UserInfo = {
    name: "영희",
    age: 20,
    gender: "female",
};
```

타입뿐 아니라 인터페이스도 확장이 가능하다.

### 8.3.4 VScode preview 속성 정보

```tsx
// 타입 선언
type User = {
    name: string;
    age: number;
};
```

```tsx
// 인터페이스 선언
interface User {
    name: string;
    age: number;
}
```

![해당 타입의 전체 속성 정보 조회 가능 ](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/574a22c8-ee0c-4c81-9728-ed4f91813153/스크린샷_2022-05-19_오전_2.29.42.png)

해당 타입의 전체 속성 정보 조회 가능

![인터페이스 명만 조회 가능](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/134d8852-1d57-4a09-a812-f29b1ab39139/스크린샷_2022-05-19_오전_2.30.00.png)

인터페이스 명만 조회 가능

VScode의 프리뷰 기능을 통해 해당 타입의 속성 정보를 조회할 수 있다. 인터페이스는 명을, 타입 별칭은 객체 리터럴 타입을 보여준다.

### 8.3.5 인터페이스 vs 타입 별칭

Typescript의 공식 문서에서는 가능하다면 인터페이스를 사용하고, 인터페이스로 표현할 수 없거나 유니온, 튜플을 사용해야 하는 상황이라면 타입 별칭을 사용하도록 권장하고 있다.
