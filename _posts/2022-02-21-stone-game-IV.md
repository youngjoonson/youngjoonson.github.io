---
layout: single
title:  "Stone Game IV"
date:   2022-02-21 22:36:07 -0400
categories: algorithms
---
### [link(Leat Code)](https://leetcode.com/problems/stone-game-iv/)
## Decription
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there are `n` stones in a pile. On each player's turn, that player makes a *move* consisting of removing **any** non-zero **square number**of stones in the pile.

Also, if a player cannot make a move, he/she loses the game.

Given a positive integer `n`, return `true` if and only if Alice wins the game otherwise return `false`, assuming both players play optimally.

## Notes
The problem implicit that a winner can be determined by givnen the number `n` of stones. Let's say there is only one stone given (`n==1`). Since only available non-zero **square** number is `1`, Alice will take `1` stone and then Bob will get `0` stone so lose the game. In this case, it's found that `1` is must-win case and `0` is must-lose case. 

When the number of stones is `2`, Alice will take one stone again since it's till the only available non-zero square number. This gives bob a chance to get `1` stone which is must-win case. So Alice will lose in this case. On the other hand, if Alice gets `3` stones, Alice will win this time since Alice can take `1` stone (which is still only non-zero square number), Bob will take `2` stones that is must-lose case.

However, `4` stones give one more choice to Alice. Alice can take `1` or `4` stones and Bob can get `3` or `0` stone. Since `0` is must-lose case, Alice will take `4` stones and win the game.

After examinng above 4 cases, it's determined that Alice must win if Alice can pass must-lose number to Bob after taking arbitrary non-zero square number.

```java
for(int i=1; i*i<=n; i++){
    if(isMustLoseNumber(n-i*i)){
        //must win
    }
}
//must lose
```

To figure out if `n` is must-win number, `n-1` needs to be figure out first. `n-1` also needs `n-2` to be figured out. So we can determine whether `n` is must-win number or not from knowing `1` is which.

`boolean[] checks checks[1] = true (1 is must win)`

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