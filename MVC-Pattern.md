# MVC 패턴 (Model-View-Controller)
프로그래밍 아키텍쳐 중 하나인 MVC 패턴을 활용한 기획서 작성법.

## MVC 패턴 참고 문서
* [공대인들이 직접쓰는 컴퓨터공부방 : MVC, MVP, MVVM 차이점](http://hackersstudy.tistory.com/71)
* [안드로이드의 MVC, MVP, MVVM 종합 안내서](https://news.realm.io/kr/news/eric-maxwell-mvc-mvp-and-mvvm-on-android)

## 개선 가능성
* Text도 하나의 다른 View모델로 분리할 수 있을 것으로 보임. Text의 경우 Success/Failure, Active/Inactive, Correct/Incorrect가 같은 위치에서 나뉘는 경우가 많은데, 이런 경우마다 Controller에서 반복적으로 적기 어려움.  (+setButtonText 메서드 활용도 괜찮을 듯)
* 케이스 혹은 예외처리를 표현할 문법 개발 필요.
* 애니메이션이나 트랜지션의 경우 어떤 식으로 구분해서 적을지 문법 개발 필요.
* 트랙킹을 표현할 문법 개발 필요.


## 구성
### View
View는 어떻게 보일지를 적음

- 실제 디자인 파일의 네이밍과 꼭 연동되어 있어야 함.
- 예외처리
  - Correct/Incorrect/Invalid (Input 컴포넌트 케이스로 다룰 것)
  - Active/Inactive
  - Success/Failure
  - Ideal/Empty/One/Some/Too Many
  
예시:
- FullScreen_more : 검정색 화면에 사진 하나가 정 가운데에 나타나는 형태. 우측 상단에 더보기 버튼이 있음.
- FullScreen_text : 검정색 화면에 사진 하나가 정 가운데에 나타나는 형태. 우측 상단에 텍스트 버튼이 있음.
- FullScreen_selection : 검정색 하면에 사진 하나가 정 가운데에 나타나는 형태. 우측 상단에 선택 버튼이 있음.
- ToastMessage_center
- ToastMessage_under




### Model
Model은 어떤 기능을 하는지 알려줌.

- Gesture_ExpectedAction_(Index_Number)
- Options이 필요한 경우 Views에 표시되어 있어야 함.
- 예외처리
  - Correct/Incorrect/Invalid
  - Active/Inactive (Switch 모델 중 하나로 다룰 것)
  - Success/Failure

예시: 
- Touch_FullScreen(self, view) : {self}를 Touch하면 유저가 이전에 등록한 데이터를 불러옴. 없으면 기본 데이터를 불러옴. 보여주는 방식은 {View}
- Touch_Switch(self, view) : {self}를 Touch하면 해당 기능이 꺼지거나 켜짐. 꺼져있는 기능은 켜지고, 켜져있는 기능은 꺼짐.
- Touch_SwitchCount(self, view) : {self}를 Touch하면 해당 기능이 꺼지거나 켜짐. 꺼질 때는 숫자가 작아지고, 켜질 때는 숫자가 하나 올라감.
- Text(self) : {self}자리에 각 언어 기호를 가진 텍스트를 보여줌. {self}가 텍스트일 경우에만 사용할 수 있음.
- Touch_Download(self, view) : {self}를 기기에 다운로드 함.



### Controller
Controller는 각 컴포넌트들이 어떤 뷰에 어떤 모델을 적용해서 보여주는지 알려줌. 
- ModelName(ViewName, [View Options List])
- 기능별 최소구성 : CRUD
- 예외처리
  - Correct/Incorrect/Invalid
  - Success/Failure
  
예시: 
- Profile Image
  - Touch_FullScreen(FullScreen_more(download, delete))
    - download
      - m.Touch_Download(v.ToastMessage_center(toastmessage_success, toastmessage_failure))
        - toastmessage_success
          - Text
            - EN) Image saved.
            - KO) 이미지가 저장되었습니다.
        - toastmessage_failure
          - Text
            - EN) Failed to save image.
            - KO) 이미지 저장에 실패했습니다.
      - m.Text
        - EN) Save to Gallery
        - KO) 갤러리에 저장하기
    - delete
      - Touch_Delete
        - Text
          - EN) Delete
          - KO) 삭제
