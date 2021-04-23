### 문제

https://www.acmicpc.net/problem/17825

```
[첫번째 도전]
와... 거의 다 완성했는데 마지막에 배열 잘못 설계한걸 알아서 결국 실패했다...  
나중에 다시 도전...
```
```
[두번째 도전]
윷놀이판의 노드들을 임의로 인덱스를 정하여, 고유의 자리를 만들어야한다.
그래서 인덱스를 따로 정의해두고, 인덱스에 따라 점수와 가는 길을 정의해주었다.
하지만 인덱스를 잘못 지정해준건지 한번에 가도록 만든것 때문인지, 코드가 매우 복잡해졌고 테케가 잘 돌아가지 않았다.
하아.. 또 나중에 도전해야겠다..
시간이 충분하다면 하나씩 앞으로 전진하는 코드를 짜자..!
```
참고 블로그: https://haejun0317.tistory.com/163


### 첫번째 코드
```
package SW역량테스트;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class bj_17825_주사위윷놀이 {
	
	static class Horse{
		int road;	 //현재 길
		int position;//현재 길에서의 위치
		boolean finish; //도착 여부
		public Horse(int road, int position, boolean finish) {
			super();
			this.road = road;
			this.position = position;
			this.finish = finish;
		}
	}
	static int maxScore, go[];
	static int[][] roadArr= { //길 정보
			{0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32,34,36,38,40}, //그대로
			{0,2,4,6,8,10,13,16,19,25,30,35,40}, //blue 10
			{0,2,4,6,8,10,12,14,16,18,20,22,24,25,30,35,40}, //blue 20
			{0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,28,27,26,25,30,35,40} //blue 30
	};
	static ArrayList<Horse> horse;
	static BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
	static StringTokenizer st;
	
	public static void main(String[] args) throws IOException {
		
		horse=new ArrayList<>();//4개의 말
		go=new int[10];			//주사위
		
		for (int i = 0; i < 4; i++) {
			horse.add(new Horse(0,0,false)); 
		}
		st=new StringTokenizer(br.readLine());
		for (int i = 0; i < 10; i++) {
			go[i]=Integer.parseInt(st.nextToken());
		}
		
		dfs(0, horse, 0, s);
		
		System.out.println(maxScore);
	}
	static String s;
	private static void dfs(int cnt, ArrayList<Horse> beforehorse, int sumScore, String string) {
		
		if(cnt==10) {
			maxScore=Math.max(maxScore, sumScore);
			if(sumScore>217)
				System.out.println(string);
			return;
		}
		
		int goNum=go[cnt]; //현재 주사위 수
		for (int i = 0; i < 4; i++) { //이동할 말
			//이미 도착한 말
			if(beforehorse.get(i).finish) continue;
			
			//1.복사
			ArrayList<Horse> newHorse=copyHorse(beforehorse);
			
			//2.말 움직이기
			int score=horseMove(goNum, i, newHorse);
			if(score==-1) continue; //이동할 수 없는 말
			
			//3.dfs
			dfs(cnt+1, newHorse, sumScore+score, string+" "+i);
		}
	}
	
	
	private static int horseMove(int goNum, int horse, ArrayList<Horse> newHorse) {
		
		int position=newHorse.get(horse).position; //현재 위치
		int road=newHorse.get(horse).road; //현재 길
		int goPosition=position+goNum; //다음 위치
		
		if(goPosition >= roadArr[road].length) { //도착
			newHorse.get(horse).finish=true;
			return 0;
		}
		
		int goRoad=road; //다음 길
		if(goRoad==0) goRoad=checkBlue(goPosition); 
		
		//다른 말이 있는지 체크
		if(checkOtherHorse(goRoad, goPosition, newHorse)) return -1;
		
		//이동
		newHorse.get(horse).position=goPosition;
		newHorse.get(horse).road=goRoad;
		int sum=roadArr[goRoad][goPosition];
		
		return sum;
	}
	
	private static int checkBlue(int goPosition) {
		
		switch (roadArr[0][goPosition]) {
		case 10: return 1;
		case 20: return 2;
		case 30: return 3;
		}
		return 0;
			
	}

	private static boolean checkOtherHorse(int goRoad, int goPosition, ArrayList<Horse> newHorse) {
		
		int num=roadArr[goRoad][goPosition];
		for (int i = 0; i < newHorse.size(); i++) {
			Horse tmp=newHorse.get(i);
			if(tmp.finish) continue;
			
			//같은 road에서 비교
			if(goRoad==tmp.road && goPosition==tmp.position) return true;
			
			//다른 road에서 비교
			if(num==25 || num==30 || num==35) {
			}
			if(num==40) {
			}
		}
		return false;
	}

	private static ArrayList<Horse> copyHorse(ArrayList<Horse> beforehorse) {
		ArrayList<Horse> newHorse=new ArrayList<>();
		beforehorse.forEach(e->newHorse.add(new Horse(e.road, e.position, e.finish)));
		return newHorse;
	}
	
}

```
