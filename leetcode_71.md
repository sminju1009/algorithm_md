# 71. Simplify Path

### 풀이 일자

2024년 6월 7일

## 알고리즘 분류

## 내 코드

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        stack = []
        stack = path.split('/')

        # 쓸데 없이 생긴 '' 제거하기
        while '' in stack:
            stack.remove('')

        stack2 = []
        skip = 0
        for i in range(len(stack)-1, -1, -1):
            if stack[i]=='.':
                continue
            elif stack[i]=='..':
                skip += 1
            elif skip>0:
                skip -= 1
            else:
                stack2.append(stack[i])

        stack2.reverse()

        return '/'+"/".join(stack2)
```

## 내 풀이 방향

처음에는 아래와 같은 방향으로 풀이를 진행하기 위해 설계를 했었다.

1. `/`로 split을 진행해서 새로운 배열(stack)에 집어 넣기
2. 만약 `..`이 들어가면 `..`와 그 이전 인덱스의 값을 제거하기
3. stack 배열을 `/`로 join 이후 return하기

그러나 서술하면서 아래와 같은 문제점이 생겨 일부 정정했다.

1. `''`가 쓸데없이 생성되는 문제 => 이 문제는 결국 `stack.remove('')`를 while문을 이용해서 제거해줬다.
2. `..` 값을 제거하는 과정에서 기존에는 stack 배열 안에서 해결하려고 했는데, 이게 잘 되지 않아서 stack2를 만들어서 진행했다.

내 풀이의 경우 메모리는 다른 제출자들에 비해 효율적으로 사용하였으나, 런타임 부분에서 효율적이지 못한 풀이를 진행했다. 이 부분에 있어서는 stack2 배열을 만들면서 진행하느라 여기서 시간이 더 걸린 것으로 추정된다. 다음에 이 문제를 다시 풀이하는 경우에는 stack2를 만들지 않고 기존 stack 배열에서 해결할 수 있도록 설계를 다시 해야 될 것 같다.

### 내 제출 링크

https://leetcode.com/problems/simplify-path/submissions/1280075432?envType=study-plan-v2&envId=top-interview-150
