### 문제
https://programmers.co.kr/learn/courses/30/lessons/72410

<br>

### 솔루션

- 단순하게 string에 대한 함수를 이용해서 조건을 만족해주는 것라고 생각해서
- `replace`, `charAt`, `substring`,`length`, `isempty`함수를 이용하여 문제를 풀었다.
- 하지만 다른 사람 코드를 보니 `정규표현식(regex)`를 이용하면 코드를 짧게 짤 수 있었다.

<br>

### 정규 표현식(regex)
```
참고 블로그
https://nesoy.github.io/articles/2018-06/Java-RegExp
https://yulsfamily.tistory.com/232
https://pridiot.tistory.com/61
```

<br>

### 나의 코드

```java
package study13;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class pro_l1_신규아이디추천 {
	
	static String id;
	static BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
	
	public static void main(String[] args) throws IOException {
		id=br.readLine();
		
		//1단계: 모든 대문자를 대응되는 소문자로 치환
		id=id.toLowerCase();
		
		//2단계: 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 모든 문자를 제거
		String tmp="";
		
		for (int i = 0; i < id.length(); i++) {
			char x=id.charAt(i);
			if((x>='a' && x<='z') || (x>='0' && x<='9') || x=='-' || x=='_' || x=='.') {
				tmp+=x;
			}
		}
		id=tmp;
		
		//3단계: 마침표(.)가 2번 이상 연속된 부분을 하나의 마침표(.)로 치환
		do {
			id=tmp;
			tmp=tmp.replace("..", ".");
		}while(!id.equals(tmp));
		
		//4단계: 마침표(.)가 처음이나 끝에 위치한다면 제거
		if(!id.isEmpty() && id.charAt(0)=='.') 
			id=id.substring(1, id.length());
		if(!id.isEmpty() && id.charAt(id.length()-1)=='.') 
			id=id.substring(0, id.length()-1);
		
		//5단계: 빈 문자열이라면, new_id에 "a"를 대입
		if(id.isEmpty()) 
			id="a";
		
		//6-1단계: 길이가 16자 이상이면, new_id의 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거
		if(id.length()>=16) {
			id=id.substring(0, 15);
			//6-2단계: 만약 제거 후 마침표(.)가 new_id의 끝에 위치한다면 끝에 위치한 마침표(.) 문자를 제거
			if(id.charAt(id.length()-1)=='.') 
				id=id.substring(0, id.length()-1);
		}
		
		//7단계: new_id의 길이가 2자 이하라면, new_id의 마지막 문자를 new_id의 길이가 3이 될 때까지 반복해서 끝에 붙임
		while(id.length()<=2) 
			id+=id.charAt(id.length()-1);
		
		//출력
		System.out.println(id);
		
	}
}

```
