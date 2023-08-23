# kioskDB
<br><br><br>
# 키오스크 화면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/698f154b-512e-4d3a-9885-026ff68e0c54) <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/aeaa58c0-5199-49bd-9c45-33cd14945394) <br><br>
SceneBuilder에서 디자인한 화면을 띄우는 코드이다.

## 품목 버튼
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/1a0fc4cf-9396-4885-b00c-a57a4d45ab09) <br>
+버튼을 누르면 원하는 품목의 개수가 1개 늘어나고, <br>
-버튼을 누르면 개수가 1개 차감된다. <br>
계산하기 버튼을 누르면 주문한 품목의 총액을 계산하여 금액이 나온다.

## 주문시
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/861e9335-33dd-4bf7-b47a-b96b05677b70) <br>
계산버튼을 누르고 주문버튼을 눌렀을때 합계가 0이라면 실패 안내창이 나오고, <br>
0이 아니라면 성공 안내창이 나오고 DB에 내가 주문한 정보가 저장된다. <br><br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/508a4474-8c34-440b-85d8-d1dc97819b64) <br>
합계가 0원이라면 DB에 정보가 저장이 되지도 않고, <br>
다시 주문해야 한다.

<br><br><br>
# 관리자 로그인 화면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/d11155f5-9b98-41d5-bd45-611edbf431ea) <br>
## 로그인 화면 관련 코드
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/0458a498-35e4-4c70-a285-88375520e9ef) <br>
입력하는 IdTextField와 PwPasswordField가 하나라도 비어있다면, <br>
경고창이 나오고 다시 입력해야한다. <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/73368a3d-345e-476b-b398-6ec7d3836ef9) <br><br>

## 비어있지 않다면
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/438e598b-d018-44b9-86ff-5051edbcab33) <br><br>
DB에 연결하여 select문을 통해 admin_accounts 테이블에 있는 정보와 비교하여 <br>
일치하다면 로그인 성공 안내창이 나오고 다르다면 로그인 실패 안내창이 나온다. <br><br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/299aaa0c-4e7b-4ca1-8300-06f329d92d01) <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/0493f829-55ae-4929-9b6f-2bca9b80d4ae)

<br><br><br>
# 통계 화면
## 전체조회 후 판매수량 그래프
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/ec2b8c22-d13a-45c3-a0ae-c4ae9d5847dd) <br>
## 전체조회 버튼 코드
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/1e81bb51-0739-40f1-a94c-45d75203ed75) <br>
전체조회 버튼을 누르면 DB접속 후 select문으로 orderlist_accounts 테이블에 있는 <br>
주문번호, 주문시간, 주문개수, 합계를 주문번호 순으로 정렬하여 <br>
orderlistTableView인 <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/b3201ba1-172a-4626-87f3-d26cdbde1084) <br>
화면에는 주문번호, 일시, 아메리카노, 카푸치노, 카페라떼의 주문 개수, 합계가 나오고 <br><br>

resultTextArea인 <br>
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/20007359-2e35-4da8-8839-70e6e10341b5) <br>
화면에는 아메리카노, 카푸치노, 카페라떼의 주문 개수와, 금액의 총합이 나온다. <br><br>

## 판매수량 그래프
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/9f003c16-b7b5-4aa8-8d34-9bd7af7b59b7) <br>
판매수량 그래프 버튼을 누르면 세 품목의 주문 개수의 비율을 나타낸 원형그래프가 나온다.<br><br>

## 날짜별 조회
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/ae36a673-f1e1-4409-94ba-a6965fd3afc4) <br>
조회 날짜 입력칸에 날짜를 입력 후 날짜별 조회 버튼을 누르면 당일에 주문한 품목의 정보가 나온다. <br><br>

## 기간별 조회
![image](https://github.com/gkstmdrb/kioskDB/assets/114748816/3d5293b7-8309-48f2-9c8a-2be883ec25f3) <br>
시작 날짜, 종료 날짜 칸에 날짜를 입력하여 범위를 설정 후 기간별 조회 버튼을 누르면 그 범위에 해당하는 날짜의 주문 정보가 나온다. <br><br><br>

# Orderlist.java
``` java
public class Orderlist {
	// 실제 테이블 뷰에 들어가는 각 컬럼들을 나타낼 수 있는 자료구조를 만들어야 함
	String idx=""; 	// 각 컬럼에 해당하는 변수 선언하기
	String date="";
	String count1="";
	String count2="";
	String count3="";
	String sum="";
	
	public Orderlist(String idx, String date, String count1, String count2, String count3, String sum) {
		super();
		this.idx = idx;
		this.date = date;
		this.count1 = count1;
		this.count2 = count2;
		this.count3 = count3;
		this.sum = sum;
	}
	public String getIdx() {	// 정수형, 날짜형, 문자형이 있지만 화면에 보이는건 모두 문자형 --> 모두 String
		return idx;
	}

	public void setIdx(String idx) {
		this.idx = idx;
	}

	public String getDate() {
		return date;
	}

	public void setDate(String date) {
		this.date = date;
	}

	public String getCount1() {
		return count1;
	}

	public void setCount1(String count1) {
		this.count1 = count1;
	}

	public String getCount2() {
		return count2;
	}

	public void setCount2(String count2) {
		this.count2 = count2;
	}

	public String getCount3() {
		return count3;
	}

	public void setCount3(String count3) {
		this.count3 = count3;
	}

	public String getSum() {
		return sum;
	}

	public void setSum(String sum) {
		this.sum = sum;
	}
}
```
