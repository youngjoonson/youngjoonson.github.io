---
layout: single
title:  "Permutation, Combinations and Subsets (한국어)"
date:   2022-03-27 10:45:07 -0400
categories: algorithms
---
### [link(Leat Code)](https://leetcode.com/problems/subsets/)
## Notes
Permutation, Combination, Subset문제는 흔히 볼수 있는 문제 유형인데요. 입력값의 길이나 크기가 큰 경우에는 경우의 수도 늘어나서, Stack overflow 나 Time Limit Exceeded등을 만나게 되기 쉽습니다. 이런 문제들의 해결법은 간결하면서도, 빠뜨리거나 중복되지 않게 계산하는게 중요합니다.

Permutation, Combination은 순열과 조합인데요, 순열과 조합의 경우의수나, 전체 부분집합의 갯수를 찾는 공식등은 여기 적지 않겠습니다. 마크다운에다가 수학 기호들을 적기가 번거롭네요. ^^

리트코드 해설에서 제시한 접근법 3가지는
- Recursion
- Backtracking
- Lexicographic (Binary Sorted) Subsets
인데요. 사실 Backtracking이 Recursion을 포함하고 있다고 생각됩니다. Recursion만으로 해결하려고 하면 보통 Time Limit Exceeded거나, 너무 느리거나, 너무 많은 용량을 쓰더라구요.

Backtracking은 접하실 기회가 많을거라 간단하게 적고 넘어가겠습니다. Backtracking을 통해 해결하는 다른 문제에서 아마 더 자세히 적을 수도 있겠지만. 딱히 많이 적을것도 없습니다.

Lexicographic bit mask 를 이요한 해결법은 사실 Subset관련 문제에 잘 먹히구요. 순열이나.. 조합에.. 도 쓸려면 쓸수 있을지도 모르겠네요. 방법이 조금 생소했는데, 깔끔하고 이해하기 쉬워서 좋아합니다.

### Backtracking
사실 이 접근법은 굉장히 간단합니다. DFS로 모든 Candidates 들을 조회하는 경우를 생각해 봅시다. Power set을 구하는 문제의 경우에는 입력된 배열의 모든 숫자들을 하나씩 보면서, 이 숫자가, ***포함되거나*** 또는 ***포함되지 않는*** 경우로 나뉠겁니다. 수형도를 그리면서 생각하면 쉬운데, 입력된 배열이 [1,2,3,4,5]라 하고, 우선 모든 숫자가 포함되지 않는 부분집합부터 시작했다고 합시다. DFS를 통해 1부터 접근했고 임시 Subset `List<Integer>`에 저장하면서 진행했다고 하면, 처음으로 숫자 5를 처리할때 두가지 부분집합이 생성됩니다. `[]` 그리고 `[5]`. 그리고 다시 숫자 4를 처리하던 메소드로 돌아오게 됩니다. 그런데 이때 임시 Subset인 List가 여전히 숫자 5를 가지고 있으면, 후에 만들어지는 부분집합은 `[4]` `[4,5]`가 아닌, `[4,5]`, `[4,5,5]`가 됩니다. 이러한 문제를 피하기 위해서 Backtracking technique은 매 Recursion 함수내에서 일어난 변화를 리셋하고 돌려주는 방식을 말합니다.

글로쓰려니 길지만 코드로 구현된것을 보면 간단합니다.

### Lexicographic (Binary Sorted) Subsets
부분집합의 경우의 수는, 원 집합 원소의 총 개수를 n이라 할때, 2^n 개 인데요. 이는 각각의 원소가 ***있을수도*** 그리고 ***없을수도*** 있다는 2가지 경우의 수를 갖기 때문입니다. 각각의 원소가.. 2가지의 경우의 수를 갖는다? 각각을 0과 1로 표현한다면 모든 경우의 수는 그 자리수에서 가질수 있는 모든 이진수로 표현이 가능할겁니다.

예를 들어 원 잡합이 3개의 원소 {1,2,3}을 갖는다고 할게요. 부분집합은 모든 원소가 없는 {} 공집합부터, {1,2,3} 까지겠네요. 각각을 이진수로 표현하면 000 ~ 111, 즉 0보다 크고 (inclusive) 2^3, 8보다 작습니다(exclusive).
- 000 :: {}
- 001 :: {3}
- 010 :: {2}
- 011 :: {2,3}
- 100 :: {1}
- 101 :: {1,3}
- 110 :: {1,2}
- 111 :: {1,2,3}

 즉 0부터 7까지의 이진수 bit mask를 구하면 간단하고 확실하게 모든 부분집합들을 찾을 수 있습니다.

 Java에서 이런 bit mask를 만드는 방법을 보면요.

 ```java
int nthBit = 1 << n;
for (int i = 0; i< Math.pow(2,n); i++){
    String bitmask = Integer.toBinaryString(i|nthBit).substring(1);
}
 ```
또다른 방법으로는 

```java
for (int i = Math.pow(2,n); i< Math.pow(2,n+1); i++){
    String bitmask = Integer.toBinaryString(i).substring(1);
}
```

