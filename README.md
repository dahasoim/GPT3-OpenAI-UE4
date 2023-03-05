## 시스템 구조 및 역할
![image](https://user-images.githubusercontent.com/57169754/222955397-ab6ad589-d5be-418e-beef-b113eda9d4ef.png)
![image](https://user-images.githubusercontent.com/57169754/222482578-c6782dc3-95c1-42c2-a282-39eb1b6f3a82.png)

<br>

## 상세 설계( 기능 구현 )
### 🎤사용자 음성인식 및 처리
#### voiceCapture Event: 사용자 음성인식 및 OpenAI GPT3를 통해 원하는 데이터로 처리 후, checkRecognition 이벤트를 호출하여 사용자 데이터를 확인한다.
1. microphone capture 컴포넌트를 사용하여 사용자 음성 캡쳐, google speech to text plugin을 사용하여 사용자 데이터음성을 text화 한다. ①, ②, ③, ④
- 사용자가 녹음 버튼을 누르면 voice capture를 시작하고 다시 누르면 voice capture를 중지. <br> 
- Google STT : 한국어 인식을 위한 South korean language 설정 및 stt실행. <br>
![voiceCapture_level](./images/voicecapture_capture.PNG)

2. Open AI GPT3 모델을 이용한 사용자 text 데이터를 원하는 형식으로 처리한다. ⑤, ⑥ 
- GPT3 settings
    - script(=start sequence) : AI가 형식에 맞는 대답을 할 수 있도록 다양한 상황에 맞춘 시나리오 작성. <strong> [GPTsettings/startsequence/version2.txt](https://github.com/dahasoim/GPT3-OpenAI-UE4/tree/main/GPT3settings/startsequence)</strong> <br>
    - stop sequence: 스크립트에서 인식 끝 범위 설정. <strong> [GPTsettings/stopsequence/stopsequence.PNG](https://github.com/dahasoim/GPT3-OpenAI-UE4/tree/main/GPT3settings/stopsequence)</strong> <br> 
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

<br>

## 데모 시연
 - Youtube링크 : https://youtu.be/10gEs9kuPbo
 - 데이터 형식 : 빼는 재료 @ 넣는 재료.
 
### 1. 양파 빼주세요.
 ![양파빼주세요](https://user-images.githubusercontent.com/57169754/222956677-dfedf04a-f622-4340-aedd-d6c3ffbffc52.gif)

### 2. 오이 빼주세요.
![오이빼주세요](https://user-images.githubusercontent.com/57169754/222956854-9987c46e-fbbf-4653-8e43-18eb43062043.gif)

### 3. 양상추 토마토 넣어주세요.
![양상추토마토넣어주세요](https://user-images.githubusercontent.com/57169754/222956863-56423a19-3e46-4434-9c63-59bd2b37601d.gif)


<br>

## 참고 사항
1. 개발자

    |이름|역할|
    |------|---|
    |이민하| STT/GPT3 설계 및 데이터 처리, GPT3 AI Script 작성|
    |임혜진| STT/GPT3 데모 설계|
    |정다은| GPT3 AI Script 데모 작성|

2. 관련 링크
      * [This Demo Blueprint](http://www.naver.com)
      * [This Demo Video](https://youtu.be/10gEs9kuPbo)
      * 참고자료 영상 : [UE4 GPT3-STT example video](https://www.youtube.com/watch?v=wtv_043sIrg&t=2s)
