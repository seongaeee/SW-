### 문제

https://www.acmicpc.net/problem/17822

```
구조가 단순하여 쉬운 문제인줄 알았더니
여러 자잘한 조건이 많다보니, 테케마다 걸리고 수정하느라 시간이 엄청 지났다
모든 게시판 테케를 해보았지만 결국 어떤곳이 틀렸는지 모르겠다ㅠㅠ
```

- 작은 조건들을 적어놓고 코딩하자!
- 범위 체크 필수!

### 코드
```
package SW역량테스트;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class bj_17822_원판돌리기 {
	static int N, M, T;
	static int[] dr= {-1,1,0,0};
	static int[] dc= {0,0,-1,1};
	static int[][] board, commad;
	static BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
	static StringTokenizer st;
	
	public static void main(String[] args) throws IOException {
		
		
		st=new StringTokenizer(br.readLine());
		N=Integer.parseInt(st.nextToken());
		M=Integer.parseInt(st.nextToken());
		T=Integer.parseInt(st.nextToken());
		board=new int[N+1][M];
		commad=new int[T][3];
		
		for (int i = 1; i <= N; i++) {
			st=new StringTokenizer(br.readLine());
			for (int j = 0; j < M; j++) {
				board[i][j]=Integer.parseInt(st.nextToken());
			}
		}
		
		for (int i = 0; i < T; i++) {
			st=new StringTokenizer(br.readLine());
			commad[i][0]=Integer.parseInt(st.nextToken()); //몇배수
			commad[i][1]=Integer.parseInt(st.nextToken()); //방향 0:시계 1:반시계
			commad[i][2]=Integer.parseInt(st.nextToken()); //k칸 회전
		}
		
		process();
		
		int sum=0;
		for (int r = 1; r < N+1; r++) {
			for (int c = 0; c < M; c++) {
				sum+=board[r][c];
			}
		}
		System.out.println(sum);
	}

	private static void process() {
		
		for (int i = 0; i < T; i++) {
			int x=commad[i][0];
			int d=commad[i][1];
			int k=commad[i][2];
			
			//1.회전
			for (int xCnt = x; xCnt < N+1; xCnt+=x) {
				turn(xCnt, d, k);
			}

			//2.인접한 수 중 같은 수
			ArrayList<int[]> sameNum=nearNum();
			
			//3.재조정
			remake(sameNum);
			
		}
	}

	private static void remake(ArrayList<int[]> sameNum) {
		//3-1.있으면? 삭제
		if(!sameNum.isEmpty()) {
			for (int j = 0; j < sameNum.size(); j++) {
				int r=sameNum.get(j)[0];
				int c=sameNum.get(j)[1];
				board[r][c]=0;
			}
		}
		//3-2.없으면? 평균
		else {
			int sum=0;
			int cnt=0;
			for (int r = 1; r < N+1; r++) {
				for (int c = 0; c < M; c++) {
					if(board[r][c]==0) continue;
					sum+=board[r][c];
					cnt++;
				}
			}
			if(sum==0) return;
			
			double avg=sum/cnt;
			for (int r = 1; r < N+1; r++) {
				for (int c = 0; c < M; c++) {
					if(board[r][c]==0) continue;
					if(board[r][c]==avg) continue;
					if(board[r][c]>avg) board[r][c]--;
					if(board[r][c]<avg) board[r][c]++;
				}
			}
		}
		
	}

	private static ArrayList<int[]> nearNum() {
		
		ArrayList<int[]> sameNum=new ArrayList<int[]>();
		boolean[][] visited=new boolean[N+1][M];
		
		for (int i = 1; i < N+1; i++) {
			for (int j = 0; j < M; j++) {
				
				if(visited[i][j]) continue;
				if(board[i][j]==0) continue;
				
				Queue<int[]> queue=new LinkedList<int[]>();
				queue.add(new int[] {i,j});
				visited[i][j]=true;
				
				boolean flag=false;
				while(!queue.isEmpty()) {
					int[] tmp=queue.poll();
					int r=tmp[0]; int c=tmp[1];
					
					for (int d = 0; d < 4; d++) {
						int nr=r+dr[d];
						int nc=c+dc[d]; if(nc<0) nc=M-1; if(nc>=M) nc=0;
						
						if(nr>=0 && nc>=0 && nr<N+1 & nc<M && !visited[nr][nc] && board[nr][nc]==board[i][j]) {
							queue.add(new int[] {nr,nc});
							visited[nr][nc]=true;
							
							sameNum.add(new int[] {nr,nc});
							flag=true;
						}
					}
				}
				if(flag) sameNum.add(new int[] {i,j});
			
			}
		}
		
		return sameNum;
	}

	private static void turn(int boardNo, int d, int k) { //d 방향, k 횟수
		
		for (int kcnt = 0; kcnt < k; kcnt++) {
			if(d==0) { //시계
				int tmp=board[boardNo][M-1];
				for (int i = M-2; i >= 0; i--) {
					board[boardNo][i+1]=board[boardNo][i];
				}
				board[boardNo][0]=tmp;
			}
			else { //반시계
				int tmp=board[boardNo][0];
				for (int i = 1; i <= M-1; i++) {
					board[boardNo][i-1]=board[boardNo][i];
				}
				board[boardNo][M-1]=tmp;
			}
		}
	}
}

```
