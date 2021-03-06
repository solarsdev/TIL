# 알고리즘 문제 해결 전략

## 재귀 호출과 완전 탐색

### 재귀 호출

- 컴퓨터에서 수행하는 작업은 기능별로 분류가 가능한데, 이 분류가 점점 세분화 되면, 상세 내용이 비슷해지는 기능들이 많이 보임
- 예를 들어, 완전히 같은 코드를 실행하는 for문이 그 예가 되는데, 이럴때 재귀 함수를 이용하면 유용하게 코드를 작성해낼 수 있음
- 재귀 함수는 자신이 수행할 작업을 유사한 형태의 여러 조각으로 쪼갠 뒤 그 중 한 조각을 수행하고, 나머지를 자기 자신을 호출해 실행하는 함수를 지칭함

```jsx
// 1부터 n까지의 합을 계산하는 반복 함수와 재귀 함수
// 필수 조건: n >= 1
// 결과: 1부터 n까지의 합을 반환한다.
const sum = (n) => {
  let ret = 0;
  for (let i = 1; i <= n; ++i) {
    ret += i;
  }
  return ret;
};
// 필수 조건: n >= 1
// 결과: 1부터 n까지의 합을 반환한다.
const recursiveSum = (n) => {
  if (n === 1) {
    return 1;
  }
  return n + recursiveSum(n - 1);
};

console.log(sum(10));
console.log(recursiveSum(10));
```

- for문을 이용한 1부터 n까지의 합을 도출하는 함수를 재귀함수 형태로 변환한 코드인데, 여기서 재귀함수에 있는 조건문을 주목해보자
- 해당 조건문이 존재하지 않을때 함수는 제대로 동작하지 않는데, 이렇듯 모든 재귀함수에는 더이상 세분화하지 못할때 답을 곧장 반환하는 조건문이 반드시 필요함
- 더이상 세분화하지 못하는 가장 작은 작업을 가리켜 재귀 호출의 기저 사례라고 부름 (base case)
- 재귀 호출의 기저 사례는 모든 호출이 사용할 수 있도록 신경써야 하는데, 만약 recursiveSum에서 n이 1인 경우를 확인하는게 아니라 2인 경우를 확인한다거나 3인 경우를 확인하면 그보다 작은 값이 들어왔을 경우에 문제가 생길 수 있음
- 샘플 문제 (백준 2775)

```markdown
# 문제

평소 반상회에 참석하는 것을 좋아하는 주희는 이번 기회에 부녀회장이 되고 싶어 각 층의 사람들을 불러 모아 반상회를 주최하려고 한다.

이 아파트에 거주를 하려면 조건이 있는데, “a층의 b호에 살려면 자신의 아래(a-1)층의 1호부터 b호까지 사람들의 수의 합만큼 사람들을 데려와 살아야 한다” 는 계약 조항을 꼭 지키고 들어와야 한다.

아파트에 비어있는 집은 없고 모든 거주민들이 이 계약 조건을 지키고 왔다고 가정했을 때, 주어지는 양의 정수 k와 n에 대해 k층에 n호에는 몇 명이 살고 있는지 출력하라. 단, 아파트에는 0층부터 있고 각층에는 1호부터 있으며, 0층의 i호에는 i명이 산다.

# 입력

첫 번째 줄에 Test case의 수 T가 주어진다. 그리고 각각의 케이스마다 입력으로 첫 번째 줄에 정수 k, 두 번째 줄에 정수 n이 주어진다.

# 출력

각각의 Test case에 대해서 해당 집에 거주민 수를 출력하라.

# 예제 입력 1

2
1
3
2
3

# 예제 출력 1

6
10
```

```jsx
const input = require('fs').readFileSync('input').toString().split('\n');
const t = parseInt(input[0]);

// 1 5 15 35 70
// 1 4 10 20 35
// 1 3  6 10 15
// 1 2  3  4  5

const getResidenceNum = (k, n) => {
  if (k === 0) {
    return n;
  }
  if (n === 1) {
    return 1;
  }
  return getResidenceNum(k - 1, n) + getResidenceNum(k, n - 1);
};

// 케이스 입력 부분
let ret = '';

for (let i = 1; i < t * 2 + 1; i += 2) {
  const k = parseInt(input[i]);
  const n = parseInt(input[i + 1]);

  ret += getResidenceNum(k, n) + '\n';
}

console.log(ret.trim());
```
