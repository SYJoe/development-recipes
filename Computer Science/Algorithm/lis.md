## LIS (Longest Increasing Sequence)

→ 최장 증가 수열

```text
10, 20, 30, 15, 20, 30, 50, 40, 45 ,60
```

> 위와 같은 수열인 경우, `10 - 15 - 20 - 30 - 40 - 45 - 60`이 LIS다. 그렇다면 LIS를 구하는 방법에는 무엇이 있을까?

<br>

## LIS를 구하는 법

> ### 1. Dynamic Programming

| **i** | **dp**                           |
| :---: | -------------------------------- |
|   0   | { 1 }                            |
|   1   | { 1, 2 }                         |
|   2   | { 1, 2, 3 }                      |
|   3   | { 1, 2, 3, 2 }                   |
|   4   | { 1, 2, 3, 2, 3 }                |
|   5   | { 1, 2, 3, 2, 3, 4 }             |
|   6   | { 1, 2, 3, 2, 3, 4, 5 }          |
|   7   | { 1, 2, 3, 2, 3, 4, 5, 5 }       |
|   8   | { 1, 2, 3, 2, 3, 4, 5, 5, 6 }    |
|   9   | { 1, 2, 3, 2, 3, 4, 5, 5, 6, 7 } |

```java
int[] arr = {10, 20, 30, 15, 20, 30, 50, 40, 45 ,60};
int[] dp = new int[arr.length];

for(int i=0 ; i<arr.length ; i++) {
	dp[i] = 1;

	for(int j=0 ; j<i ; j++) {
		if(arr[j] < arr[i]) {
            dp[i] = Math.max(dp[j]+1, dp[i]);
		}
	}  // for_j
}

int max = Integer.MIN_VALUE;
for(int i=0 ; i<arr.length ; i++)
    max = Math.max(dp[i], max);

System.out.println(max);
```

- 대표 문제: [BOJ 11053 가장 긴 증가하는 수열](https://www.acmicpc.net/problem/11053)
- 시간복잡도: `𝑂(𝘕²)`
- 배열의 길이가 길 경우 문제 해결 불가능

<br>

> ### 2. Lower Bound (이분 탐색)

| `LIS`                    | `탐색값` | `추가` / `대치` | `결과`                         |
| ------------------------ | :------: | :-------------: | ------------------------------ |
| {10}                     |    20    |      추가       | {10, `20`}                     |
| {10, 20}                 |    30    |      추가       | {10, 20, `30`}                 |
| {10, 20, 30}             |    15    |      대치       | {10, `15`, 30}                 |
| {10, 15, 30}             |    20    |      대치       | {10, 15, `20`}                 |
| {10, 15, 20}             |    30    |      추가       | {10, 15, 20 ,`30`}             |
| {10, 15, 20, 30}         |    50    |      추가       | {10, 15, 20, 30, `50`}         |
| {10, 15, 20, 30, 50}     |    40    |      대치       | {10, 15, 20, 30, `40`}         |
| {10, 15, 20, 30, 40}     |    45    |      추가       | {10, 15, 20, 30, 40, `45`}     |
| {10, 15, 20, 30, 40, 45} |    60    |      추가       | {10, 15, 20, 30, 40, 45, `60`} |

```java
    . . .

    int[] arr = {10, 20, 30, 15, 20, 30, 50, 40, 45 ,60};
    LIS = new int[N];

    int lisIdx = 1;
    LIS[0] = A[0];

    for(int i=1 ; i<N ; i++) {
		// A[i]가 더 크면 그냥 추가
		if(LIS[lisIdx - 1] < A[i]) {
			lisIdx++;
			LIS[lisIdx - 1] = A[i];
		}
		// 그렇지 않을 경우 Lower Bound 이분탐색 진행
		else {
			int idx = lowerBound(0, lisIdx, A[i]);
			LIS[idx] = A[i];
		}
	}
	System.out.println(lisIdx);
}
public static int lowerBound(int L, int R, int n){
    int mid = 0;

    while(L < R){
        mid = (L + R) / 2;

        if(LIS[mid] < n) L = mid + 1;
        else R = mid;
    }
    return L;
}
```

- 시간복잡도: `𝑂(𝘕 ㏒𝘕)`

<br>

---

### 참고자료

- [@st-lab](https://st-lab.tistory.com/285)
- [@junhok82](https://velog.io/@junhok82/lowerbound-upperbound)
