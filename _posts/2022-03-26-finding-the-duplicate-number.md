---
layout: single
title:  "Finding the duplicate number (한국어)"
date:   2022-03-26 21:37:07 -0400
categories: algorithms
---
### [link(Leat Code)](https://leetcode.com/problems/find-the-duplicate-number/)
## Decription
Give an array of integers `nums` containing `n+1` integers where each integer is in the range `[1,n]` inclusive.
There is only **one repeated number** in `nums`, return this repeated number.
You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

## Notes
입력된 배열에서 중복된 숫자를 찾는 것은 쉽다. 숫자들을 하나씩 `HashSet` 등의 중복을 허용하지 않는 DS에 집어넣다 보면 linear time으로 해결되는 문제다. 그러나, 입력된 데이터를 조작하면 안된다는 조건과 함께, Constant extra space를 써야 한다는 고약한 조건이 붙어있다. 위에 언급한 방법은 **모든** 숫자를 **하나씩** 넣어보기 때문에, 최악의 경우 모든 숫자를 다 넣어봐야 중복된 수가 나온다. 즉 `O(n)`의 Space complexity를 갖는다.

Leetcode 에는 고맙게도 이에 관한 7가지 접근법을 소개하는데, 처음 4가지는 위에 조건을 만족하지는 않지만, 자주 쓰이는 테크닉이라 알아두면 좋을것 같다고 한다. 마지막 Two pointer 방식은 LinkedList 관련 문제에도 많이 쓰이고, 그림이 없으면 설명하기 힘들어서 나중에 포스팅 해야지.

### 1. Sort
간단하게, 입력된 배열을 크기순으로 정렬을 하고, 앞에서 부터 하나씨 찾아보는 방식. 직관적이고 간단하게 생각할 수 있지만 위에 조건을 만족하지도 않고, 대체로 최선의 해법과는 항상 거리가 좀 있더라

#### Analysis
- Time complexity: `O(n log n)` 많이 쓰이는 정렬 알고리즘이 merge랑 quick이고 둘 모두 `O(n log n)`의 Time complexity를 갖는다. 물론 정렬 후에 `O(n)` 의 시간동안 처리를 더 하지만, 무시된다.
- Space complexity: `O(log n)` 또는 `O(n)` . 정렬 알고리즘을 직접 짜서 쓰지 않는이상, 언어가 같고있는 라이브러리르 쓰는데, 각 언어가 갖고있는 Implementation에 따라 이 space complexity가 달라진다. Java의 경우는 Quick sort의 변형을 쓴다. 그래서 `O(log n)`. C++ 는 Quick sort, heap sort, insertion sort를 잘 섞어서 쓴다고 한다. `O(log n)`. Python 의 경우 Timsort 를 쓰는데 그래서 `O(n)` 의 Space complexity 를 갖는다. 
  - 적어는 봤지만 굳이 알아야 할까 하는 생각이다.
 
---

### 2. Set
간단하게, 위에서 말한것 처럼, 중복이 허용되지 않는 Data Structure를 이용해서 중복된 숫자를 찾는 방법이다.... 그 밖에 굳이 더 할 말이.. 없는거 같다.

#### Analysis
- Time complexity: `O(n)` , 모든 숫자를 한번씩 다 넣다보면 나온다.
- Space complexity: `O(n)`, 최악의 경우에도, 입력된 배열이 갖는 숫자의 개수만큼의 공간이 필요

---

### 3. Negative Marking
개인적으로 리트코드 하면서 은근히 많이 본 테크닉이다. 이건 입력된 배열을 좀 조작해야하고, 또 모든 숫자가 0보다 크다는 보장이 있어야 한다.

#### Algorithm
입력된 배열, `nums`의 임의의 수를 `n` 이라 할때 그 범위는 `[1,n]` 이고, `nums`의 길이는 `n+1`이다. 여기서 포인트는 `nums`의 인덱스의 범위가 `[0,n]`이라는 점. 이는 각각의 숫자가 가질 수 있는 범위를 포함한다. 그 말은 입력된 배열안의 숫자들을 ***인덱스***로 사용할 수 있다는 뜻. 그래서 만약 `i == j` 이면, `nums[i] == nums[j]` 라는 점을 이용해서 문제를 풀 수 있다.

1. 배열에서 숫자 하나를 가져온다.
2. 위 숫자의 절대값을 구한다.
3. 그 절대값, `i`, 의 위치의 수를 조회해 본다. `nums[i]`
4. 조회한 숫자가 0보다 크면 -1을 곱해준다.
5. 조회한 숫자가 0보다 작다? 이전에 조회를 한적이 있다? 인덱스로 사용한 숫자 `i`가 전에 또 나왔었다? => `i`가 중복된 수

#### Analysis
- Time Complexity: `O(n)` 한 번씩 쭉 돌다보면 나오기 때문에
- Space Complexity: `O(1)` 특별히 추가로 사용된 DS가 없다.

---

### 4. Array as HashMap (Recursive)
위의 방식과 비슷한 접근법이나, 본격적으로 입력된 배열을 조작한다. 포인트는 위에 비슷하다. 

#### Algorithm
입력된 배열 `nums`, 각각의 숫자의 범위는 `[1.n]`, 그리고 `nums`의 `index`의 범위는 `[0,n]`이다. 이번 접근법을 간단히 설명하자만, 각각의 숫자를, 제자리에 (그 숫자를 인덱스로 했을 때의 위치)에 가져다 놓다보면, 중복된 숫자를 발견할 수 있다는 것.

1. 0은 각 숫자의 범위를 벗어난다. 이 위치를 임시 저장소로 사용한다.
2. `nums[0]`의 숫자를 가져온다. `n1`
3. 이 숫자 `n1`을 알맞은 자리 `nums[n]`에 가져다 놓기 위해 이미 이자리에 있는 숫자와 교환을 해야 한다. 이미 이자리에 있는 숫자를 `n2`라 하면,
4. `n1 != n2`, 숫자 `n1`은 `nums[n]`에 넣고, `n2`는 `nums[0]`에 넣어둔다. 그리고 2번 단계로 돌아가 반복한다 (recursively).
5. `n1 == n2`, 중복된 수를 발견했다.

#### Analysis
- Time Complexity O(n). 3번 Negative Marking이랑 같은 이유
- Space Complexity O(n), Recursive 방식이므로, 메소드 또는 함수의 호출이 쌓이다 보면 `O(n)`까지 갈 수 있다.

---

### 5. Array as HashMap (Iterative)
알고리즘 자체는 위와 정확히 같다. 다만 단계 4번 반복을 Recursive가 아닌 Iteraton으로 하는것. 훨씬 간단하고 보기 좋다. 이정도 반복을 Recursive로 하는게 사실 말이 안됨.

#### Analysis
- Time Complexity O(n). 3번 Negative Marking이랑 같은 이유
- Space Complexity O(n), Recursive 방식이므로, 메소드 또는 함수의 호출이 쌓이다 보면 `O(n)`까지 갈 수 있다.

