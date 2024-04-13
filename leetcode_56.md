# leetcode 56. Merge Intervals

### 풀이 일자

2024년 4월 13일

## 문제 링크

https://leetcode.com/problems/merge-intervals

## 최종 제출 링크

https://leetcode.com/problems/merge-intervals/submissions/1230783476

## 처음 내 문제풀이

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:

        intervals.sort(key=lambda x: x[0])

        n = len(intervals)
        for i in range(n-1):
            for j in range(i+1, n):
                if intervals[i][0]<=intervals[j][0]<=intervals[i][1]<=intervals[j][1]:
                    intervals[i] = [intervals[i][0], intervals[j][1]]
                    intervals[j] = [intervals[i][0], intervals[j][1]]

                if intervals[i][0]>=intervals[j][0] and intervals[i][1]<=intervals[j][1]:
                    intervals[i] = [intervals[j][0], intervals[j][1]]
                    intervals[j] = [intervals[j][0], intervals[j][1]]

                if intervals[i][0]<=intervals[j][0]<=intervals[j][1]<=intervals[j][0]:
                    intervals[j] = [intervals[i][0], intervals[i][1]]

        ans = []
        for i in range(n):
            if intervals[i] not in ans:
                ans.append(intervals[i])

        return ans

```

처음 풀이할 당시에는 제시되어 있는 테스트 케이스 2가지(하단 제시)에 있는 경우들만 고려해서 문제를 풀이함.

```
intervals = [[1,3],[2,6],[8,10],[15,18]]
```

```
intervals = [[1,4],[4,5]]
```

그러다 보니 자꾸 예외를 고려하지 못하고 문제를 풀이하게 되었고, 그러면서 점점 상단에 보이는 것처럼 `if intervals[i][0]` 으로 시작하는 조건들이 자꾸 늘어나게 되고 불필요한 조건문이 증가함. 그 과정에서 여러 조건문이 엉키면서 기존에 되던 테스트 케이스들도 자꾸 오답이 뜨는 문제가 발생함.

## 수정된 문제풀이

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0])

        ans = []
        for interval in intervals:
            if not ans or ans[-1][1] < interval[0]:
                ans.append(interval)
            else:
                ans[-1][1] = max(ans[-1][1], interval[1])

        return ans
```

기존에는 불필요하게 구간을 순회하는 일이 있었는데, 코드를 전체적으로 고치면서 각 구간을 한 번만 순회하여 필요한 경우에만 병합을 수행하게끔 변경함.
그렇게 하다 보니 조건문이 엉키는 문제도 발생하지 않음.
