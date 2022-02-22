---
layout: post
title:  "Stone Game IV (한국어)"
date:   2022-02-20 17:01:07 -0400
categories: algorithms leetcode
---
### [link(Leat Code)](https://leetcode.com/problems/stone-game-iv/)
## Decription
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there are `n` stones in a pile. On each player's turn, that player makes a *move* consisting of removing **any** non-zero **square number**of stones in the pile.

Also, if a player cannot make a move, he/she loses the game.

Given a positive integer `n`, return `true` if and only if Alice wins the game otherwise return `false`, assuming both players play optimally.

## Notes
문제에 따르면, 두 플레이어 앨리스와 밥이 번갈아 돌을 가져가는데, 그 수는 반드시 제곱수(1, 4, 9, 16, 25, 36,... ) 여야 한다. 처음 주어지는 돌의 개수는 n이고 (n>=1 and n<=10^5>) 돌을 가져가지 못하는 사람 즉 0개의 돌을 받게 되는 사람이 패배한다.
주어진 돌의 개수 n에 따라, 앨리스가 이긴다면 true, 그렇지 않다면 false를 반환해야 한다.

문제는 돌의 개수가 플레이어의 승패를 확정한다고 가정한다. 돌의 개수 1 부터 생각해 보면 (n=1), 앨리스가 1만큼의 돌을 가져갈 경우 밥은 0개의 돌을 받게 되어 패배한다. 즉 0은 반드시 패배()하는 경우의 수, 1은 반드시 승리하는 경우의 수로 볼 수 있다.
돌의 개수 2인 경우 앨리스가 1개의 돌을 가져가 밥이 1개의 돌, 필승의 수, 를 받게되어 앨리스가 패배한다. 반면 돌의 개수 3인 경우 앨리스가 1개의 돌을 가져가서 밥이 2개의 돌, 필패의 수, 를 받게되어 앨리스가 승리한다. 
따라서 돌 2는 필패, 3은 필승의 수라 볼 수 있다.

이제 돌의 개수가 4인 경우를 보자. 앨리스는 자기 차레에 1이나 4만큼의 돌을 가져갈 수 있다. 1개의 돌을 가져가면 3, 필승의 수를 밥에게 주어 패배하게 되지만, 4개의 돌을 가져갈 경우 0, 필패의 수,를 밥에게 넘기게 되어 승리하게 된다. 앨리스는 4개의 돌을 가져갈 경우 반드시 승리하므로 4는 필승의 수라 볼 수 있다.

여기서 우리는 앨리스가 주어진 돌 n개에서 임의의 제곱수를 빼어, 필패의 경우의 수를 만들 수 있으면, n은 필승의 수 인것을 알수있다.

```java
for(int i=1; i*i<=n; i++){
    if(isMustLoseNumber(n-i*i)){
        //must win
    }
}
//must lose
```

또한, n이 필승의 수인지 알기 위해선, n-1의 경우를 알아야 하므로, n=1 부터 점진적으로 계산해 볼 수 있다.

만약 n개의 돌이 필승인 경우, boolean array checks[n] 에 true를 그렇지 않으면 false를 저장한다.

```java
boolean[] checks = new boolean[n+1];
// to use index as number of stone (1 ~ n)
checks[1] = true;
// can omit for 0,2 since its default value is false

for(int i=3; i<=n; i++){
	for(int j=1; j*j<=i; j++){
		if(!checks[i-j*j]){
			//must win
			checks[i]=true;
			break;
		}
	}
	// number i is must-lose case but no need
	// continue leaving checks[i] as false
}

return checks[n];
```
## My Solution

```java
class Solution {
    public boolean winnerSquareGame(int n) {
        boolean[] checks = new boolean[n+1]; //default value is false;
        checks[1] = true;
        // checks[2] = false;
        for(int i = 3; i <= n; i++){
            for(int j = 1; j*j <= i; j++){
                if(!checks[i-j*j]){
                    checks[i]=true;
                    break;
                }
            }
        }
        return checks[n];
    }
}
```