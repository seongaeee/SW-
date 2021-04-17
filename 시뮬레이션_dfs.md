### 시뮬레이션에 DFS 쓰는 경우?

- 어떤 하나의 상황에서 여러 가지 경우의 수가 나오는 경우

### 시뮬레이션에 DFS 쓰는 방법?

- DFS 타고 다음 단계로 넘어갈때, 변화되는 값들을 DFS 매개변수로 넣어준다.
- 하나의 경우의 DFS가 끝나고 다시 이전으로 돌아가기 위해, copy를 미리 해둔다.

### 코드 예시

```
private static void process(int[][] map, int cnt) { //map: 이전 벽돌 모습
		
  if(cnt==N) {
    int sum=calSum(map);
    min=Math.min(sum, min);

    return;
  }

  int[][] newMap=new int[H][W];
  for (int w = 0; w < W; w++) { //증복수열
    //1.map복사
    copyMap(newMap,map);

    //2.구슬 떨어뜨리기
    if(!drop(newMap,w)) process(newMap, cnt+1);
    else {
      //3.빈자리 채우기
      fill(newMap);

      //4.다음으로 넘기기
      process(newMap, cnt+1);
    }
  }

}
```
