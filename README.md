## 시스템 구조 및 역할
![image](https://user-images.githubusercontent.com/57169754/222325827-8287c63f-7404-4fa4-8fef-ec2fe1d8289a.png)

## 상세 설계( 기능 구현 )
### 🎤사용자 음성인식 및 처리
#### voiceCapture Event: 사용자 음성인식 및 OpenAI GPT3를 통해 원하는 데이터로 처리 후, checkRecognition 이벤트를 호출하여 사용자 데이터를 확인한다.
1. microphone capture 컴포넌트를 사용하여 사용자 음성 캡쳐, google speech to text plugin을 사용하여 사용자 데이터음성을 text화 한다. ①, ②, ③, ④
- 사용자가 녹음 버튼을 누르면 voice capture를 시작하고 다시 누르면 voice capture를 중지. <br> 
- Google STT : 한국어 인식을 위한 South korean language 설정 및 stt실행. <br>
![voiceCapture_level](./images/voicecapture_capture.PNG)

2. Open AI GPT3 모델을 이용한 사용자 text 데이터를 원하는 형식으로 처리한다. ⑤, ⑥ 
- GPT3 settingsscript(=start sequence) : AI가 형식에 맞는 대답을 할 수 있도록 다양한 상황에 맞춘 시나리오 작성
    - stop sequence: 를 설정. <br> 
    - Max Tokens: 한국어 1글자에 1토큰 소요, 적당히 50 설정. <br>
    - Temperature : 0.1로 설정하여 모델의 자율도 낮게 설정. <br>
- Open AICall GPT3 : GPT를 실행하여 사용자 데이터를 처리 및 처리결과 받음. <br>
- GPT3의 한번에 사용 가능한 토큰량의 한계가 있는 이유로, 주문 프로세스(주문 단계)를 나눠서 GPT3 sequence(=scripts)를 작성.
![voiceCapture_level](./images/voicecapture_level.PNG)

3. checkRecognition 이벤트에서 확인된 사용자 주문 데이터의 녹음 성공여부를 전달하고, 화면에 주문 정보(데이터)를 띄우는 함수 호출한다. ⑦, ⑧
- Write STT: 사용자 주문 데이터를 임시저장하고 화면에 띄움.
![voicecapture_wirteSTT](./images/voicecapture_wirteSTT.PNG)
### ✔ 처리된 사용자 데이터(text) 확인
#### checkRecognition Event: 사용자가 주문 프로세스에 따른 정확한 대답을 했는지 성공/실패 반환
1. GPT3에서 처리된 text(=사용자 주문 메뉴)에서 불필요한 값을 정리한다. <br>
- " , "AI:" , "\n" 및 쓰레기 값을 제거.<br>
(ex) AI: Im robot. -> Im robot
![1번 처리 블루프린트](./images/checkrecognition.PNG)

2. 사용자 주문 메뉴가 기존 메뉴에 있고 상황에 맞는 정확한 대답인지, 잘못된 대답인지 확인한다. 
  <br> 잘못된 대답일 경우 실패, 다시 음성인식을 시도를 요청한다. <br> 정확한 대답일 경우 성공, 다음 프로세스(주문단계)로 넘어도록 한다.

## 데모 시연

## 참고 사항
1. 개발자

    |이름|역할|
    |------|---|
    |이민하|일랑일랑홀랑훌롱|
    |임혜진|어저고저저고|

2. 관련 링크
      * [This is Demo Blueprint](http://www.naver.com)
      * 참고영상 : [UE4 GPT3-STT example video](https://www.youtube.com/watch?v=wtv_043sIrg&t=2s)
