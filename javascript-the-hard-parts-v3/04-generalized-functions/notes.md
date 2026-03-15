# JavaScript: The Hard Parts v3 - 일반화된 함수와 고차 함수

## 1. 함수가 존재하는 이유

**재사용을 고려하지 않은 함수**를 작성하면, 동일한 연산임에도 값이 다를 때마다 새로운 함수를 별도로 선언해야 한다.

```javascript
function tenSquared() {
  return 10 * 10;
}

function nineSquared() {
  return 9 * 9;
}

function eightSquared() {
  return 8 * 8;
}
```

위 코드는 **DRY(Don't Repeat Yourself)** 원칙을 위반한다. 이 원칙은 프로그래밍의 근본 원칙 중 하나로, 불필요하게 코드를 반복 작성하면 추적과 유지보수가 훨씬 어려워진다.

---

## 2. 매개변수(Parameter)를 통한 함수 일반화

매번 동일한 **제곱(squaring)** 연산을 반복 작성하는 대신, 입력값을 일반화하여 재사용 가능한 함수를 만들 수 있다.

```javascript
function squared(num) {
  return num * num;
}

squared(10); // 100
squared(9);  // 81
squared(8);  // 64
```

**매개변수(parameter)**는 자리 표시자(placeholder)로, 함수를 실행하기 전까지 어떤 데이터를 사용할지 결정하지 않아도 된다. 함수를 실행하는 시점에 값이 동적으로 채워진다.

| 호출 | num에 채워지는 값 | 결과 |
|------|-----------------|------|
| `squared(10)` | 10 | 100 |
| `squared(9)` | 9 | 81 |
| `squared(8)` | 8 | 64 |

핵심: **코드를 한 번 작성하고, 서로 다른 데이터로 반복 재사용한다.**

---

## 3. 고차 함수(Higher Order Functions)

매개변수로 데이터뿐만 아니라 **코드(기능) 자체**도 미정으로 남겨둘 수 있다면 어떨까?

- **데이터를 나중에 결정** → 매개변수(parameter)
- **기능(코드)을 나중에 결정** → 고차 함수(Higher Order Functions)

함수의 일부 기능을 비워두고, 실제 실행 시점에 채워 넣을 수 있다면 함수는 훨씬 더 재사용 가능하고 범용적이 된다.

> 고차 함수란, 함수를 인수(argument)로 받거나 함수를 반환하는 함수이다. 실행하기 전까지 일부 기능을 확정하지 않아도 된다.

---

## Q&A

<details>
<summary>Q: 콜백 함수도 고차 함수인가?</summary>

아니다. 고차 함수와 콜백 함수는 한 쌍이지만 역할이 다르다.

- **고차 함수(Higher Order Function)** — 함수를 인수로 **받는** 함수, 또는 함수를 **반환하는** 함수
- **콜백 함수(Callback Function)** — 고차 함수에 인수로 **전달되는** 함수

```javascript
function higherOrder(callback) { // ← 고차 함수
  return callback(5);
}

function double(num) { // ← 콜백 함수
  return num * 2;
}

higherOrder(double); // 10
```

</details>
