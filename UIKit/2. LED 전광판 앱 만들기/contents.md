# LED 전광판 앱 만들기
- 기능 상세
    - LED 전광판 화면을 표시합니다.
    - LED 전광판에 표시할 텍스트, 텍스트 컬러, 배경 색상을 설정 화면에서 설정할 수 있습니다.

- 활용 기술
    - UINavigationController
    - 화면 전환 개념
    - ViewController Life Cycle
    - 화면간 데이터 전달하는 방법
    - 에셋 카탈로그

## NavigationController 알아보기
- Content View Controller
    - 화면을 구성하는 뷰를 직접 구현하고 관련된 이벤트를 처리하는 뷰 컨트롤러
    - 스토리보드 생성시 기본으로 생성됨

- Container View Controller
    - 하나 이상의 Child View Controller를 가지고 있다.
    - 하나 이상의 Child View Controller를 관리하고 레이아웃과 화면 전환을 담당한다.
    - 화면 구성과 이벤트 관리는 Child View Controller에서 한다.
    - Container View Controller는 대표적으로 Navigation Controller와 TabBar Controller가 있다.

### UINavigationController?
<image src = "Resource/NavigationController.png" >

- 계층구조로 구성된 content를 순차적으로 보여주는 container view controller
- 아이폰 설정
    - Setting 앱에서 General을 선택하면 General 자식뷰컨트롤러가 표시되고 이전의 뷰컨트롤러는 가려지게 됨
    - 최상단 네비게이션 바에서 뒤로가기를 클릭하면 이전에 숨겨진 뷰컨트롤러가 다시 나타남
    - 네비게이션 뷰컨트롤러는 네비게이션 스택이라고 칭하는 정렬된 배열을 사용하여 자식 뷰컨트롤러를 관리한다.

- 네비게이션 컨트롤러는 네비게이션 스택을 이용하여 화면 전환을 관리
    - 배열의 첫번째 뷰 컨트롤러는 루트뷰컨트롤러를 의미하고 스택에 가장 밑에 있다는 것을 의미
    - 배열의 마지막 뷰 컨트롤러는 스택의 최상단을 의미한다. 현재 화면에 보여지고 있는 것을 의미
    - 개발자는 세그웨이를 사용하거나 UI네비게이션 컨트롤러 메소드를 사용해서 스택으로부터 뷰 컨트롤러를 추가하고 제거할 수 있다
    - 사용자는 최상단 back 버튼을 사용해서 최상단 뷰컨트롤러를 제거할 수 있고 left edge swipe 제스쳐 사용해서 제거할 수 있다.
    - 네비게이션 스택이 어떻게 자식 뷰 컨트롤러를 관리하는지 알아봄

#### Navigation Stack

<image src = "Resource/NavigationStack1.png" >

- 기본적으로 LastInFirstOut의 스택구조
- 설정앱 설정화면에서 알림을 누르면 설정화면 위에 알림화면 표시
- A 화면은 루트뷰컨트롤러 B 화면은 자식뷰컨트롤러, 네비게이션 설정화면 위에 알림화면이 쌓인 구조, 네비게이션 스택에 푸시했다고 말함

<image src = "Resource/NavigationStack2.png" >

- 알림화면에서 Siri 제안을 누르게 되면 알림화면 위에 Siri제안이 표시
- 네비게이션 스택에 Siri제안이 푸시되면서 알림화면 위에 Siri제안이 쌓임
- 이렇게 상위 카테고리에서 하위 카테고리로 넓혀져 가는 구조

<image src = "Resource/NavigationStack3.png" >

- 사용자가 back버튼을 사용하거나 left edge swipe 제스쳐를 사용해서 최상단에 있는 뷰 컨트롤러를 제거
- Siri제안 화면 아래있는 알림화면이 표시되고 Navigation Stack에는 Siri제안 화면이 없어짐
- 네비게이션 스택에 pop 되었다고 말함

<image src = "Resource/NavigationStack4.png" >

- 알림화면에서 back버튼을 사용하거나 left edge swipe 제스쳐를 사용해서 최상단에 있는 뷰 컨트롤러를 제거
- 네비게이션 스택에 루트 뷰컨트롤러인 설정화면만 남게됨

#### Navigation Bar
<image src = "Resource/NavigationBar.png" >
- 네비게이션 컨트롤러 구현할 시 화면 상단에 보여지는 바
- 루트 뷰컨트롤러 외에 모든 뷰컨트롤러에 back버튼 유저가 계층구조에서 다시 뒤로갈 수 있게 해줌
- navigationbar button item, title, prompt로 구성되어있으며 자식 뷰컨트롤러마다 다른 네비게이션바를 구성할 수 있음

## 화면 전환 개념 소개
- iOS에서 화면을 전환하는 방법 크게 2가지
    1. 소스코드를 통해 전환하는 방식
    2. Storyboard를 통해 전환하는 방식
- iOS에서 화면을 전환하는 방법 작게 4가지
    1. View Controller의 View 위에 다른 View를 가져와 바꿔치기 -> 되도록 사용하지 말아야 하는 방법!! 메모리 누수 위험
    2. View Controller에서 다른 View Controller를 호출하여 전환하기
    3. Navigation Controller를 사용하여 화면 전환하기
    4. 화면 전환용 객체 세그웨이(Segueway)를 사용하여 화면 전환하기
     
### View Controller에서 다른 View Controller를 호출하여 전환하기
<image src = "Resource/화면전환1_1.png" >
<image src = "Resource/화면전환1_2.png" >

- 직접 표시한다는 의미에서 presentation view 라고도 함
- 기존 뷰 컨트롤러 위에 새로운 뷰 컨트롤러를 덮는다
- present 메서드에 이동할 화면의 뷰컨트롤러는 넘겨주면 이전화면에서 이동할 화면의 뷰 컨트롤러가 표시됨
- present 메서드 파라미터
    - viewControllerPresent
        - 새로운 화면의 뷰컨트롤러 인스턴스
    - flag
        - 화면을 전환할 때 애니메이션을 줄 것인지 안 줄 것인지 boolean 값
    - completion
        - 화면 전환 후에 실행될 클로저

- 새로운 화면에서 이전화면으로 돌아가는 메서드 dismiss
- dissmiss 파라미터
    - flag
        - 화면을 전환할 때 애니메이션을 줄 것인지 안 줄 것인지 boolean 값
    - completion
        - 화면 전환 후에 실행될 클로저

### Navigation Controller를 사용하여 화면 전환하기
<image src = "Resource/화면전환2_1.png" >
<image src = "Resource/화면전환2_2.png" >

- 계층적인 성격을 띄는 컨텐츠 구조를 관리하기 위한 컨트롤러
- 뷰컨트롤러의 전환을 직접 컨트롤하고 앱에 네비게이션 정보를 표시하는 역할
- 네비게이션 스택으로 자식 뷰 컨트롤러를 관리
- 네비게이션 스택은 후입선출로 나중에 들어오는게 제일 먼저 나가는 방식
- pushViewController 메서드로 네비게이션 컨트롤러에 화면을 추가
- pushViewController 메서드의 파라미터
    - viewController
        - 새로운 화면의 뷰컨트롤러 인스턴스
    - animated
        - 화면을 전환할 때 애니메이션을 줄 것인지 안 줄 것인지 boolean 값
- popViewController 메서드로 네비게이션 컨트롤러에 화면을 제거
- popViewController 메서드의 파라미터
    - animated
        - 화면을 전환할 때 애니메이션을 줄 것인지 안 줄 것인지 boolean 값
### 화면 전환용 객체 세그웨이(Segueway)를 사용하여 화면 전환하기
- 세그웨이는 두개의 뷰 컨트롤러 사이에 연결된 화면 전환용 객체를 의미
- 스토리보드를 통해서 출발지와 목적지를 직접 지정하는 방식을 세그웨이를 이용한 화면전환 이라고 합니다.
- 코드를 사용하지 않고 스토리보드만을 이용해 화면 전환
- 세그웨이 종류
    - Action Segueway
        - 출발점이 버튼, 셀 등인 경우
        - 따로 코드를 추가해주지 않아도 됨
    - Manual Segueway
        - 출발점이 뷰컨트롤러 자체
        - 적절한 시점에 performSegue라는 메서드 호출
- Action Segueway 종류
    - Show
        - 가장 일반적인 세그웨이
        - 네비게이션 컨트롤러를 사용하면 화면전환시 뷰컨트롤러가 네비게이션 스택에 쌓이게 됨
        - 네비게이션 컨트롤러를 사용하지 않으면 프레딧????? 됨
    - Show Detail
        - Split View 에서 사용하는 세그웨이로 아이폰에서 사용하게 되면 Show 세그웨이와 똑같이 동작
        - 아이패드에서 사용하게 되면 스플릿 구조의 마스터슬레이브 구조로 보임
    - Present Modally
        - 이전 뷰 컨트롤러를 덮으면서 새로운 화면이 나타나게 됨
        - 프레젠테이션 방식으로 화면이 전환됨
    - Present As Popover
        - 아이패드에서 팝업창 띄울 때 사용됨
    - Custom
        - 세그웨이를 사용자가 원하는 방식으로 커스텀할 때 사용

## 화면 전환 구현
    - navigation controller
        - main.storyboard에서 네비게이션 컨트롤러와 기존의 뷰컨트롤러 세그웨이로 연결하고 rootViewController로 지정
        - 네비게이션 컨트롤러를 inital View Controller로 체크
    
    - Segue로 Push
        - 스토리보드에서 Show 세그웨이로 연결
    - Segue로 Present
        - 스토리보드에서 Present modally 세그웨이로 연결
        - 세그웨이의 인스펙터에서 Presentation - fullScreen으로 변경 가능
    - 코드로 Push
        - 스토리보드에 있는 뷰컨트롤러를 인스턴스화 해줌, storyboard에서 identifier 정의
```Swift
        // 스토리보드에 있는 뷰컨트롤러를 인스턴스화 해줌, storyboard에서 identifier 정의
        guard let viewController = self.storyboard?.instantiateViewController(identifier: "CodePushViewController") else {
            return
        }
        self.navigationController?.pushViewController(viewController, animated: true)
```

    - 코드로 Present
```Swift
        // 스토리보드에 있는 뷰컨트롤러를 인스턴스화 해줌, storyboard에서 identifier 정의
        guard let viewController = self.storyboard?.instantiateViewController(identifier: "CodePresentViewController") else {
            return
        }
        // fullScreend으로 바꿔줌
        viewController.modalPresentationStyle = .fullScreen
        self.present(viewController, animated: true, completion: nil)
```

## ViewController LifeCycle
<image src = "Resource/뷰컨트롤러생명주기.png" >

- 뷰가 보여지는 상황을 네가지로 분류 가능
    1. Appearing: 뷰가 화면에 나타나는 중
    2. Appeared: 뷰가 화면에 나타나는게 완료 된 상태
    3. Disappearing: 뷰가 화면에서 사라지는 중
    4. Disappeared: 뷰가 화면에서 사라진 상태

- 뷰의 상태 변화에따라 시스템에 의해 특정 메소드들을 호출
- viewDidLoad()
    - 뷰 컨트롤러의 모든 뷰들이 메모리에 로드됐을 때 호출
    - 메모리에 처음 로드될 때 한 번만 호출
    - 보통 딱 한 번 호출될 행위들을 이 메소드 안에 정의 함
    - 뷰와 관련된 추가적인 초기화 작업, 네트워크 호출
    - pop된 뷰컨트롤러를 다시 push하면 메모리에 다시 로드되는 것이기 때문에 호출됨

- viewWillAppear()
    - 뷰가 뷰 계층에 추가되고, 화면에 보이기 직전에 매 번 호출
    - 다른 뷰로 이동했다가 돌아오면 재호출
    - 뷰와 관련된 추가적인 초기화 작업

- viewDidAppear()
    - 뷰 컨트롤러의 뷰가 뷰 계층에 추가된 후 호출
    - 뷰를 나타낼 때 필요한 추가 작업
    - 애니메이션을 시작하는 작업

- viewWillDisappear()
    - 뷰 컨트롤러의 뷰가 뷰 계층에서 사라지기 전에 호출
    - 뷰가 생성된 뒤 작업한 내용을 되돌리는 작업
    - 최종적으로 데이터를 저장하는 작업

- viewDidDisappear()
    - 뷰 컨트롤러의 뷰가 뷰 계층에서 사라진 뒤에 호출
    - 뷰가 사라지는 것과 관련된 추가 작업

## 화면간 데이터 전달하기
- 루트 뷰컨트롤러에서 코드로 전환된 뷰컨트롤러에 데이터 전달해보기
    - 루트 뷰 컨트롤러에서 인스턴스화 해준 뷰컨트롤러를 전달받을 뷰컨트롤러로 다운캐스팅하면 그 컨트롤러에서 정의한 프로퍼티에 접근 가능
    - 그러면 다른 화면에 push와 present 되기 전에 프로퍼티에 값을 넘겨주면 전달될 화면으로 데이터를 전달 할 수 있음

```Swift
        guard let viewController = self.storyboard?.instantiateViewController(identifier: "CodePushViewController") as? CodePushViewController else {
            return
        }
        viewController.name = "Sunghee"
        self.navigationController?.pushViewController(viewController, animated: true)
```

- 전환된 화면에서 이전 화면으로 데이터 전달하기
    - delegate 패턴
        - ios에서 자주 사용되는 디자인 패턴 
        - delegate는 '위임하다'라는 사전적 뜻을 가지고 있는데 여기서 delegate는 위임자
        - 위임자를 갖고있는 객체가 자신의 일을 다른 객체에 위임하는 형태
        - **전달 할 viewController에서 할 일**
        1. 위임할 viewController에서 Delegate 프로토콜 생성
        2. Delegate프로토콜 안에 데이터 전달하는 메서드 만들어줌
        3. delegate 변수 생성
            - delegate변수를 사용할 때 강환 참조로 인한 메모리 누수를 방지하기 위해 weak을 붙여줌
        4. 이전화면에 전달할 데이터를 프로토콜 메서드의 파라미터로 전달
            - 데이터를 전달받은 뷰컨트롤러에서 프로토콜을 채택하고 delegate를 위임받게되면 해당 메서드를 호출할 수 있음
```Swift
import UIKit

protocol SendDataDelegate: AnyObject {  // 1. 위임할 viewController에서 Delegate 프로토콜 생성
    func sendData(name: String)         // 2. 데이터 전달할 메서드 구현
}

class CodePresentViewController: UIViewController {

    @IBOutlet weak var nameLabel: UILabel!
    var name: String?
    weak var delegate: SendDataDelegate?    // 3. delegate변수 생성, delegate변수를 사용할 때 강환 참조로 인한 메모리 누수를 방지하기 위해 weak을 붙여줌
    
    override func viewDidLoad() {
        super.viewDidLoad()
        if let name = name {
            self.nameLabel.text = name
            self.nameLabel.sizeToFit()
        }
    }
    @IBAction func tapBackButton(_ sender: Any) {
        self.delegate?.sendData(name: String)   // 4. 이전화면에 전달할 데이터를 sendData파라미터로 적어줌, 데이터를 전달 받은 뷰컨트롤러에서 SendDataDelegate 프로토콜을 채택하고 Delegate를 위임받게 되면 SendData 호출 가능함
        self.presentingViewController?.dismiss(animated: true, completion: nil)
    }
}

```

- 
    - 
        - **전달 받을 viewController에서 할 일**
        5. Delegate프로토콜 채택
        6. push나 present호출되기 전에 위임받기
        7. 프로토콜 함수 구현

```Swift
    //
//  ViewController.swift
//  ScreenTransactionExample
//
//  Created by 나성희 on 2021/09/08.
//

import UIKit

class ViewController: UIViewController, SendDataDelegate {  // 5. Delegate프로토콜 채택

    @IBOutlet weak var nameLabel: UILabel!
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }

    @IBAction func tapCodePushButton(_ sender: Any) {
        // 스토리보드에 있는 뷰컨트롤러를 인스턴스화 해줌, storyboard에서 identifier 정의
        // 전환되는 화면의 뷰 컨트롤러로 다운캐스팅 -> 그 컨트롤러에 정의한 name 프로퍼티에 접근할 수 있다.
        // 그러면 다른화면에 push와 present되기 전에 name프로퍼티에 값을 넘겨주면 전환된 화면으로 데이터를 전달할 수 있다.
        guard let viewController = self.storyboard?.instantiateViewController(identifier: "CodePushViewController") as? CodePushViewController else {
            return
        }
        viewController.name = "Sunghee"
        
        self.navigationController?.pushViewController(viewController, animated: true)
    }
    
    @IBAction func tapCodePresentButton(_ sender: Any) {
        guard let viewController = self.storyboard?.instantiateViewController(identifier: "CodePresentViewController") as? CodePresentViewController else {
            return
        }
        viewController.name = "Sunghee"
        viewController.modalPresentationStyle = .fullScreen
        // 6. push나 present호출되기 전에 위임받기
        viewController.delegate = self
        self.present(viewController, animated: true, completion: nil)
    }
    func sendData(name: String) {   // 7. 프로토콜 함수 구현
        self.nameLabel.text = name
        self.nameLabel.sizeToFit()
    }
}

```

- 세그웨이를 통한 화면 전환방법에서 데이터 전달하기
    - 세그웨이로 구현된 화면 전달 방법에서 전환되는 화면에 값을 전달하기 제일 좋은 위치는 전처리 prepare메서드임
    - prepare메서드를 오버라이드하면 세그웨이를 실행하기 이전에 시스템에 의해 자동으로 호출 됨
    1. 전달 받을 뷰컨트롤러에서 name 프로퍼티 정의
    ```Swift
        var name: String?
    ```
    2. 전달할 뷰컨트롤러에서 prepare메서드 오버라이드
    ```Swift
        override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if let viewController = segue.destination as? SeguePushViewController { // 다운캐스팅해줌

            viewController.name = "Sunghee"
        }
    }
    ```
## LED전광판 UI 그리기

## 에셋 카탈로그를 이용하여 꾸며보기
## 기능 구현하기
