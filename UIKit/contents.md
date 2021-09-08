### delegate 패턴
- 객체지향 프로그래밍에서 하나의 객체가 모든 일을 처리하는 것이 아니라 처리해야할 일 중 일부를 다른 객체에 넘기는 것을 뜻한다.
- UITextFieldDelegate 예제

```Swift

import UIKit

class ViewController: UIViewController {

    
    @IBOutlet weak var enteredLabel: UILabel!
    @IBOutlet weak var textField: UITextField!
    
    
//    @IBAction func buttonClicked(_ sender: Any) {
//        enteredLabel.text = textField.text
//    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        textField.delegate = self   // 2. 위임자를 지정함, textField의 일 중 일부를 ViewController가 대신할 수 있음
    }


}

extension ViewController: UITextFieldDelegate {     // 1. UIViewController에서 UITextFieldDelegate 채택함
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {      // 3. 대신해주는 함수
        enteredLabel.text = textField.text
        return true
    }
}
```

- 버튼을 누르지 않고도 텍스트필드 엔터치면 label의 값이 변경됨
- PickerView는 UIPickerViewDataSource, UIPickerViewDelegate 채택한 후에 구현해주어야 하는데
- 프로토콜 채택할 떄는 반드시 같이 선언해주어야하는 함수가 있는지 확인하는 습관들 들여야한다.
- protocol안에 optional로 선언되어 있는 함수는 반드시 구현하지 않아도 되지만
- optional이 붙지 않은 함수는 반드시 구현해야한다.

### NavigationController BackButton색 바꾸기
- backButton이 생기는 ViewController를 SecondViewController라고 했을 때
- SecondViewController의 viewDidLoad()에
```Swift
    self.navigationController?.navigationBar.tintColor = .green
```
- back 대신 다른 title 써주려면
```Swift
    self.navigationController?.navigationBar.topItem?.title = ""
```
- 또는 firstViewController에서 
```Swift
    let backBarButtonItem = UIBarButtonItem(title: "NSH", style: .plain, target: self, action: nil)
    backBarButtonItem.tintColor = .gray
    self.navigationItem.backBarButtonItem = backBarButtonItem
    
```
### ViewController의 생명주기
1st viewDidLoad
1st viewWillAppear
1st viewDidAppear
---------------------
1st viewWillDisappear
2nd viewDidLoad
2nd viewWillAppear
1st viewDidDisppear
2nd viewDidAppear

- viewWillDisappear다음에 ViewDidDisppear가 호출될 것 같지만 그렇지 않음
- 두번째뷰가 로드되고 두번째 뷰의 viewWillAppear가 호출된 다음에 첫번째 뷰의 viewDidDisappear가 호출됨

1. 네비게이션 컨트롤러의 동작은 자료구조에서의 '스택'과 같다!
2. pop을 하면 스택에서 빠져나간 뷰 컨트롤러는 메모리에서 사라진다.
3. 다시 push하면 viewDidLoad() 다시 호출된다.

### UserDefaults
- 데이터 저장소
- 앱의 어느 곳에서나 데이터를 쉽게 읽고 저장할 수 있게 됨
- 사용자 기본 설정과 같은 단일 데이터 값에 적합
- 앱 제거만 안하면 유지됨

### CGPoint, CGSize, CGRect
- CGPoint
    - 정의: 2차원 좌표계의 점을 포함하는 구조체
- CGSize
    - 정의: 너비와 높이 값을 포함하는 구조체
- CGRect
    - 정의: 사각형의 위치와 크기를 포함하는 구조체

```Swift
public struct CGRect {
    public var origin: CGPoint
    public var size: CGSize
    public init()
    public init(origin: CGPoint, size: CGSize)
}
```

<image src = "Resource/CGRect.png" >

```Swift
var rectangle = CGRect(origin: CGPoint(x: 0, y: 0), size: CGSize(width: 50, height: 30))
```

### Frame, Bounds 차이
- frame
    - SuperView(상위뷰)의 좌표시스템안에서 View의 위치와 크기를 나타낸다.
- Bounds
    - View의 위치와 크기를 자신만의 좌표시스템안에서 나타낸다.
    - 상위뷰와 아무런 상관이 없으며, 오직 자신이 기준
    - Bounds의 origin은 default로 (0,0)
    - 오히려 하위뷰가 움직이는 것처럼 보임
    - 스크롤뷰 원리

### TableView cell의 separator padding 문제
- 테이블 뷰 작업할때 앞에는 공간이 넓고 뒤에는 공간이 없음
- 이렇게 하면 앞의 공간이 사라짐
```Swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {

.

. 

.

cell.separatorInset = UIEdgeInsets.zero

.

.

}



출처: https://zeddios.tistory.com/235?category=682195 [ZeddiOS]
```

### frame.height VS frame.size.height
- frame.height는 CGRect의 height로 CGFloat타입이고 get만 가능
- frame.size.heigh는 CGRect의 size의 height로 CGFloat타입이고 get, set 가능
```Swift
myView.frame.height = 10//error!!!! frame.height는 get이므로.

myView.frame.size.height = 10//가능



출처: https://zeddios.tistory.com/242?category=682195 [ZeddiOS]
```
