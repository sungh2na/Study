# 명언 생성기 앱 만들기
<image src = "Resource/명언생성기.png" >

- 기능 상세
    - 버튼을 누를 때마다 랜덤한 명언이 생성된다
- 활용 기술
    - Storyboard
    - AutoLayout
    - UILabel
    - UIButton
- 기능을 구현하기 앞서
    - UIKit
    - UIViewController
    - AutoLayout
    - IBOutlet & IBAction

## UIKit 알아보기

### Cocoa touch Framework

<image src = "Resource/cocoatouch.png" >

- ios 개발환경을 구축하기 위한 최상위 프레임워크
- 일반적으로 ios 개발을 위해 Object-c 혹은 Swift에서 상속해서 사용하는 UIkit Foundation 포함한 대부분의 클래스 객체들이 코코아더치프레임워크에 속한다.
### Foundation
    - 가장 기본적인 데이터타입, 자료구조, 각족 구조체, 네트워크 통신, 파일관리 등 기본적인 프로그램의 중심 담당
### UIKit

<image src = "Resource/UIKit.png" >

- UI라는 이름에서 알 수 있듯이 사용자의 인터페이스를 관리하고 이벤트를 처리하는 게 주 목적인 프레임워크
- UIKit에서 주로 처리하는 사용자 이벤트
    - 제스쳐 처리
    - 애니메이션
    - 그림그리기
    - 이미지 처리
    - 텍스트 처리
    
- 어플리케이션 화면 관리하는 요소
    - 테이블뷰
    - 슬라이더
    - 버튼
    - 텍스트필드
    - 스위치
    - 알럿창

- 자주 사용하는 UIViewController나 UIView등 UI가 붙는 Class 등을 사용하려면 UIKit을 import 해줘야 함
- Code Structure of a UIKit App
     UIKit 앱의 구조는 기본적으로 MVC디자인패턴을 사용

<image src = "Resource/MVC.png" >   

    - Model은 앱의 데이터와 비즈니스 로직
    - View는 사용자에게 데이터를 보여주는 UI 
    - Controller는 Model과 View의 중간다리 역할로 View로부터 사용자의 액션을 전달받아 Model에게 어떤 작업을 해야하는지 알려주거나 Model의 데이터변화를 View에 전달하여 어떻게 업데이트할지 알려줌
    - Model이 무엇을 Controller가 어떻게 View가 보여줄 것인가

    - View와 Model의 상호 의존성을 없애고 Controller가 중간다리 역할을해주는게 MVC패넡의 이상적인 형태
    - 현실의 MVC패턴 형태

<image src = "Resource/MVC1.png" > 

    - View와 ViewController가 너무 친함
    - ViewController가 거의 모든 일을 담당 ViewController가 View의 라이프사이클에 관여하기 때문에 View와 Controller를 분리하기가 어렵다
    - 프로젝트의 규모가 커질수록 Controller가 비대해지고 내부 구조가 복잡해져 유지보수가 어려워진다
    - MVVM이나 바이퍼패턴등 단점을 해결하기위한 디자인패턴이 나옴
    
### UIViewController 알아보기
- UIView?
    - 화면의 직사각형 영역에 대한 내용을 관리하는 개체
    - 화면을 구성하는 요소의 기본 클래스
    - 위치와 크기를 갖는 사각형으로 배경색을 가지고 있고 문자나 이미지등의 컨텐츠를 갖는 것이 가능
    - 여러 UIComponent들을 가지고 있어서 이것을 보여주는 용도로 사용

- UIViewController?
    - 앱의 근간을 이루는 객체로 모든 앱은 최소한 하나 이상의 뷰 컨트롤러를 가지고 있다.
    - 사용자가 화면을 보는 것에 대한 관리
    - 화면을 만든 후에 시뮬레이터로 실행시켜 보면 뷰컨트롤러가 나옴
    - ViewController의 주요 역할
        - 데이터 변화에 따라서 View 컨텐츠를 업데이트
        - view들과 함께 사용자 상호작용에 응답
        - view를 리사이징하고 전체적인 인터페이스의 레이아웃 관리
        - 다른 뷰컨트롤러들과 함께 앱을 구성한다.
    - 앱을 사용할 때 화면마다 다른 컨텐츠가 표시되고 화면을 터치해서 다른 화면으로 이동할 수 있는데 이것이 ViewController의 역할

### AutoLayout을 이용하여 화면 그리기
- AutoLayout?
    - 제약 조건(Constraints)을 이용해서 뷰의 위치를 지정하는 것
    - 아이폰의 다양한 해상도 비율에 대응하기위해 나온 개념
    - 아이폰의 크기가 다양해지면서 해상도도 달라졌는데 다른 크기에서도 화면을 똑같이 보여주기 위해 사용
    - 세로보기 화면뿐 아니라 가로보기 화면도 지원
    - 스토리보드에서 해줌

    - Add New Constraint
<image src = "Resource/AddNewConstraint.png" > 
        - 뷰 간의 제약조건 설정 
    - Align
<image src = "Resource/Align" >
        - 뷰 간의 정렬 설정
    - Resolve Auto Layout Issues
        - AutoLayout 관련 이슈를 처리
        - 현재 제약조건 기준으로 제약 업데이트
        - 누락된 제약조건 추가, 삭제 , 추천 등

## IBOutlet & IBAction 알아보기
- storyboard scene과 ViewController를 연결하는 방법은 inspector의 가운데 메뉴에서 Custom Class 지정
- IBOutlet
    - 스토리보드에 등록된 UIObject에 코드로 접근하여 컨트롤하기 위해 변수에 바인딩한 변수
    - 메모리 회수 정책
        - Strong
            - 다른 곳에서 참조하고 있을 경우에 메모리에서 해제되지 않음
        - Weak
            - 다른 곳에서 참조하고 있더라도 시스템이 임의적으로 제거할 수 있음
            - 메모리 누수 방지
- IBAction
    - 동작을 정의하는 함수로 어떠한 동작을 할 수 있도록 정의하고 연결
    - 이벤트 처리


## 명언 생성 기능 구현하기 
- Priority 설정
    - UI Framework에서 제공되는 일부 뷰에는 컨텐츠 고유사이즈라는 개념이 있다.
    - 예를 들면 UI Label, UI Button, text나 image에 따라 크기가 결정되어야하는 컨텐츠
    - 모든 뷰들은 다른 뷰들과 걸린 제약에 의해서 컨텐츠 고유 사이즈보다 늘어나거나 줄어들게 된다.
    - 이때 더 늘어나는 것에 저항하는 제약을 Content Hugging 이라고 한다.
    - 더 줄어드는 것에 저항하는 제약을 Content Compression Resistance 라고 한다.
    - 고유컨텐츠 사이즈 변경에 대한 제약에도 우선순위가 있는데 이 우선순위에 따라 컨텐츠가 고유사이즈가 변경되었을 때 사이즈를 늘어나게 할 것인지 줄어들게 할 것인지 결정
    - Hugging Priority
        - 본래 컨텐츠 사이즈보다 늘어나야 할 경우에 우선순위가 높을수록 내 크기를 유지하고 작을수록 크기가 늘어남
    - Compression Resistance Priority
        - 본래 컨테츠 사이즈보다 줄어들어야 할 경우에 우선순위가 높을수록 내 크기를 유지하고 작을수록 크기가 줄어듦
- label의 side inspector에서 line을 0으로 설정하면 여러줄로 나타낼 수 있음 