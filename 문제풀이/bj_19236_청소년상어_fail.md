### bj_19236_청소년상어
https://www.acmicpc.net/problem/19236

```
- 시뮬레이션+DFS 문제
- DFS 매개변수로는 map, shark, fish
- 기저 조건 대신 사방탐색으로 걸러줌
```

### 참고 코드
https://bcp0109.tistory.com/entry/%EB%B0%B1%EC%A4%80-19236%EB%B2%88-%EC%B2%AD%EC%86%8C%EB%85%84-%EC%83%81%EC%96%B4-Java

### 나의 코드

```
package SW역량테스트;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class bj_19236_청소년상어 {
	
	static class Fish{
		int no;
		int dir;
		public Fish(int no, int dir) {
			super();
			this.no = no;
			this.dir = dir;
		}
	}
	static class Shark{
		int r;
		int c;
		int sum;
		int dir;
		public Shark(int r, int c, int sum, int dir) {
			super();
			this.r = r;
			this.c = c;
			this.sum = sum;
			this.dir = dir;
		}
	}
	static int max;
	static int[] dr= {0,-1,-1,0,1,1,1,0,-1}; //상 좌상 좌 좌하 하 우하 우 우상
	static int[] dc= {0,0,-1,-1,-1,0,1,1,1};
	static BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
	static StringTokenizer st;
	
	public static void main(String[] args) throws IOException {
		
		Fish[][] map=new Fish[4][4];
		
		for (int i = 0; i < 4; i++) {
			st=new StringTokenizer(br.readLine());
			for (int j = 0; j < 4; j++) {
				int no=Integer.parseInt(st.nextToken());
				int dir=Integer.parseInt(st.nextToken());
				map[i][j]=new Fish(no, dir);
			}
		}
		
		//상어 들어가기
		Shark shark=new Shark(0, 0, 0, 0);
		shark=sharkIn(map);
		
		//이동 시작
		process(map, shark);
		
		System.out.println(max);
	}

	private static void process(Fish[][] map, Shark shark) {
		
		//0.max 비교
		if(max<shark.sum) {
			max=shark.sum;
		}
		
		//1.물고기 이동
		fishMove(map,shark);
			
		//2.상어 이동에 따른 dfs
		int r=shark.r;
		int c=shark.c;
		int dir=shark.dir;
		
		for (int i = 1; i <= 3; i++) {
			int nr=r+dr[dir]*i;
			int nc=c+dc[dir]*i;
			
			if(nr<0 || nc<0 || nr>=4 || nc>=4) continue;
			if(map[nr][nc]==null) continue;

			Fish[][] newMap=copyMap(map);
			Shark newShark=copyShark(shark);
			
			newShark.r=nr;
			newShark.c=nc;
			
			Fish f=newMap[nr][nc];
			newShark.sum+=f.no;
			newMap[nr][nc]=null;

			process(newMap, newShark);
		}
	}

	private static Shark copyShark(Shark shark) {
		Shark tmp=new Shark(0, 0, 0, 0);
		tmp.r=shark.r;
		tmp.c=shark.c;
		tmp.sum=shark.sum;
		tmp.dir=shark.dir;
		
		return tmp;
	}

	private static Fish[][] copyMap(Fish[][] map) {
		Fish[][] tmp=new Fish[4][4];
		
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				tmp[i][j]=map[i][j];
			}
		}
		
		return tmp;
	}

	private static void fishMove(Fish[][] map, Shark shark) {
		
		for (int i = 1; i <= 16; i++) { //번호 순서대로 이동
			for (int r = 0; r < 4; r++) {
				for (int c = 0; c < 4; c++) {
					
					if(map[r][c]==null) continue;
					
					if(map[r][c].no==i) {
						//1.이동가능한 칸 찾기
						int[] go=fishSearch(map,r,c,shark);
						
						//2.이동 가능
						if(go[0]!=-1) fishGo(map,r,c,go);
					}
				}
			}
		}
		
	}


	private static void fishGo(Fish[][] map, int r, int c, int[] go) {
		
		Fish fish=map[r][c];
		
		if(map[go[0]][go[1]]==null) { //빈칸
			map[go[0]][go[1]]=fish;
			map[r][c]=null;
		}else { //다른상어있음
			Fish tmp=map[go[0]][go[1]];
			map[go[0]][go[1]]=fish;
			map[r][c]=tmp;
		}
	}

	private static int[] fishSearch(Fish[][] map, int r, int c, Shark shark) {
		
		Fish fish=map[r][c];
		
		int[] go=new int[2];
		go[0]=-1;
		
		int dir=fish.dir; //현재 상어 방향
		
		//1.이동 가능할 때까지 45도 반시계 회전
		for (int d = 0; d < 7; d++) {
			
			if(dir==9) dir=1;
			
			int nr=r+dr[dir];
			int nc=c+dc[dir];
			
			//이동 불가능한 경우
			if(nr<0 || nc<0 || nr>=4 || nc>=4) {dir++; continue;}
			if(shark.r==nr && shark.c==nc) {dir++; continue;}
			
			//이동 가능한 경우
			go[0]=nr;
			go[0]=nc;
			fish.dir=dir;
			break;
		}
		
		return go;
	}

	private static Shark sharkIn(Fish[][] map) {
		Fish f=map[0][0];
		Shark shark=new Shark(0, 0, f.no, f.dir);
		map[0][0]=null;
		
		return shark;
	}
	
//	private static void print() {
//		for (int i = 0; i < 4; i++) {
//			for (int j = 0; j < 4; j++) {
//				if(map[i][j]==null) continue;
//				System.out.print(map[i][j].no+" ");
//			}
//			System.out.println();
//		}
//	}
}

```
