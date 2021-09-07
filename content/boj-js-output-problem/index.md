---
emoji: 🐌
title: js로 알고리즘 풀다가 시간초과 나올 때
date: '2021-09-07 17:51:24'
author: 이준영
tags: algorithm
categories: 알고리즘
---

# js로 알고리즘 풀다가 시간초과 나올 때(console.log 관련)

## 왜 시간초과가 나오니....
백준에서 알고리즘 문제를 푸는데 시간초과로 정답 통과가 안되는 상황이 발생했다.
로직적인 문제인건지 계속 생각해보며 코드 수정을 했는데 시간이 좀 오래 걸릴만한 테스트 케이스로 테스트 해보니
뭔가 터미널에 로그가 늦게찍히는 느낌이 들었다.

전에 python으로 백준에서 문제를 풀 때 `input`을 사용할 때 너무 느려서 `sys.stdin.readline`
을 사용해서 해결한 적이 있었기에 설마 js에서도 비슷한 문제인건지 찾아봤는데 역시 `console.log`가 너무 느려서 시간초과가 일어난거였다!

```js
let fs = require('fs')
const PATH = process.platform === 'linux' ? '/dev/stdin' : 'testcase.txt'
const N = +(fs.readFileSync(PATH).toString().trim())

function hanoi(N, start, end, via) {
  if (N === 1) {
    console.log(`${start} ${end}`)
  } else {
    hanoi(N-1, start, via, end)
    console.log(`${start} ${end}`)
    hanoi(N-1, via, end, start)
  }
}

console.log(2 ** N - 1)
console.log(hanoi(N, 1, 3, 2))
```

이렇게 하고 `N`값을 4, 5정도만 해보고 괜찮은 것 같아서 제출했었는데 20으로 테스트하게 되면 `console.log`가 백만번 이상 호출되었기 때문에 시간초과가 일어나게 된 것이었다.

---
## 해결방법 #1
```js
let fs = require('fs')
const PATH = process.platform === 'linux' ? '/dev/stdin' : 'testcase.txt'
const N = +(fs.readFileSync(PATH).toString().trim())

const answer = [];
function hanoi(N, start, end, via) {
  if (N === 1) {
    answer.push(`${start} ${end}`);
  } else {
    hanoi(N-1, start, via, end);
    answer.push(`${start} ${end}`);
    hanoi(N-1, via, end, start);
  };
};

console.log(2 ** N - 1);
hanoi(N, 1, 3, 2);
console.log(answer.join("\n"));
```
`console.log`때문에 속도가 느려졌으니 `console.log`로 한 번에 모두 출력하기 위해서 array를 따로 만들고 거기에 출력들을 전부다 집어넣어서 join메서드를 활용해서 출력했다!

이렇게 해서 제출해보니 일단 통과는 되었는데 array에 결과값을 하나씩 다 집어넣고 나중에 합치다보니 메모리가 너무 비효율적으로 사용되게 되어서 다른 방법을 찾아보게 됐다🥲

---
## 해결방법 #2
```js
let fs = require('fs')
const PATH = process.platform === 'linux' ? '/dev/stdin' : 'testcase.txt'
const N = +(fs.readFileSync(PATH).toString().trim())

function hanoi(N, start, end, via) {
  if (N === 1) return `${start} ${end}\n`

  let answer = ''
  answer += hanoi(N - 1, start, via, end)
  answer += `${start} ${end}\n`
  answer += hanoi(N - 1, via, end, start)
  return answer
}

console.log(2 ** N - 1)
console.log(hanoi(N, 1, 3, 2))
```
이번엔 array를 따로 사용하지 않고 아예 string에 계속 더해서 return시켰다.

array에 담고 꺼내는 동작이 없어지니 메모리 사용량이 151600kb에서 74848kb로 반 정도 줄어들고 처리 속도도 array에 담을 때 보다 빨라지게 됐다!

---

## 느낀점

확실히 js는 아직 익숙하지 않은데다 알고리즘을 js로 풀게된 건 일주일도 안됐기 때문에 python으로 풀던 시절이 그리울 때가 있다....😢

일단 js로 백준에서 알고리즘 문제를 풀려면 input부터 이게 뭐야?라는 말이 나오기 때문에 시작하는 것 자체가 살짝 힘든 부분이 있다.. 

그래도 한 번 어떻게 하는지 정립을 해놓고 하니까 점점 적응도 되고 js 자체도 알고리즘을 풀어가면서 여러 방식을 사용해서 문제를 해결하기 때문에 새로운 메서드를 하나씩 알아가는게 너무 재밌는 것 같다!

앞으로도 알고리즘을 지속적으로 열심히 풀어봐야겠다....ㅋㅋㅋㅋ 개인적인 생각이지만 언어에 익숙해지는데는 알고리즘 문제 푸는것이 효과가 좋은듯!😊

```toc
```