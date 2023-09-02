# TeleSquare TLR-2005KSH Reverse Engineering
2CPU 장터에 나온 텔레스퀘어 LTE 모뎀 하드웨어 역공학  
<img src="https://user-images.githubusercontent.com/27724108/189268814-a558de9a-d8ed-4d26-b40a-c868591d8ba5.png" width="200" />  
"프로그래머 아님" + "해커 아님" + "정보보안 알못" 의 라우터 뜯어보기 "유사" 라이트업

## 하드웨어 뜯어보기
### 하드웨어 분해 방법
1. 나사 4개 (고무 피트 아래에 있음) 및 유심홀 개방 (나사 1개)  
   <img src="https://github.com/Alex4386/telesquare-lte-re/assets/27724108/dbffb436-a508-4284-a8af-ee6c7007befc" width="400" />
2. 윗판이 열리므로 윗판을 기준으로 위로 개방  
   <img src="https://github.com/Alex4386/telesquare-lte-re/assets/27724108/fb802121-e657-44bf-ae5a-9af828045116" width="400" />

### UART 물리기
이 부분은 [2CPU 게시물](https://www.2cpu.co.kr/freeboard_2011/1530219?&sfl=wr_subject&stx=LTE&sop=and) 을 참고하여 작성되었습니다.  

본래대로면 멀티미터로 그라운드 핀 찾아서 물려야 하는데, 멀티미터기가 고장이 난 관계로.... 사전 자료를 바탕으로 추정 (통상 쉴드랑 Continuity 체크해서 GND 핀 찾음)  
![image](https://github.com/Alex4386/telesquare-lte-re/assets/27724108/bde56556-8611-4178-a97c-08d8e43fea39)  

내 UART의 경우 컬러코딩 방식이라 (VCC-빨간색, GND-검정색, TX-흰색, RX-초록색) 다음과 같이 꽂음.  
(임베디드 해킹 팁: 전원을 따로 인가할때는 그라운드만 연결할 것, 역전류 먹여서 기계 죽일꺼 아니면 VCC 는 꽂지 말것~)  

### BaudRate 찾기
Arduino IDE 의 시리얼모니터는 시리얼을 연결한 상태에서 baud rate 를 변경할 수 있음.  
putty 로 매번 baud rate 변경하고 다시 연결하기 귀찮으니 Arduino IDE에게 이 라우터가 아두이노라고 훼이크를 치고 Arduino IDE 를 이용해 baud rate 를 찾자.  

![image](https://github.com/Alex4386/telesquare-lte-re/assets/27724108/6f7431e2-7965-4e64-a925-fde68196dc02)

일단 제일 흔해 빠졌다는 9600, 115200 부터 먼저 해보자.  
  
![image](https://github.com/Alex4386/telesquare-lte-re/assets/27724108/b3e19cc0-4e9b-48ab-b15a-400f712a04ab)  
9600: 깨진다.

![image](https://github.com/Alex4386/telesquare-lte-re/assets/27724108/44c79eac-4c04-4fea-ba0b-632c83770e36)
115200: 깨진다.  

일단 흔해 빠진 친구는 아니다. 다른 걸로 해보자.  
![image](https://github.com/Alex4386/telesquare-lte-re/assets/27724108/9931677c-0d32-421e-8224-3df85b6a3a2f)  

노가다 끝에 baudrate가 57600 이라는 것을 알아냈다.  


