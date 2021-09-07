### 가변매변수
- 매개변수의 개수를 알 수 없을 때
- 배열로 전달받음

```Swift

func sendMessage(me: String, friend: String...) -> String {
    return "Hello \(friend)! I'm \(me)"
}

sendMessage(me: "Gunter", friend: "Json", "Albert", "Stella")

```

### 옵셔널 바인딩
- 명시적 해제
    - 강제 해제
        - 옵셔널 이름 뒤에 느낌표
    - 비강제 해제(옵셔널 바인딩)
        - if let, guard let
- 묵시적 해제
    - 컴파일러에 의한 자동 해제
        - 비교연산
    - 옵셔널의 묵시적 해제

```Swift

var number: Int? = 3
print(number)
print(number!) // 강제해제

if let result = number {   // 비강제해제 if
    print(result)
} else {
    
}

func test() {       // 비강제해제 guard
    let number: Int? = 5
    guard let result = number else { return }
    print(result)
}

test()

let value: Int? = 6
if value == 6 { // 비교연산자를 이용해 다른값과 비교하면 컴파일러가 자동적으로 해제해줌
    print("value가 6입니다.")
} else {
    print("value가 6이 아닙니다.")
}

// 옵셔널 타입의 물음표를 느낌표로 바꿈 -> 사용할 때 묵시적 해제
let string = "12"
var stringToInt: Int! = Int(string)
print(stringToInt + 1)


```

### 프로퍼티
- 클래스, 구조체 또는 열거형 등에 관련된 값을 뜻한다
- 저장 프로퍼티
    - 인스턴스의 변수 또는 상수
- 연산 프로퍼티
    - 값을 의미하는 게 아니라 특정 연산을 실행하는 결과값 의미
    - get() set() 통해서 값을 연산하여 프로퍼티 저장해줌

- 타입 프로퍼티
    - 특정 인스턴스가 아닌 타입에서 사용되는 프로퍼티
    - 저장 프로퍼티와 연산 프로퍼티에서 사용 가능
    - static 키워드
    - 인스턴스생성 안해도 사용 가능

- 구조체는 값타입, 인스턴스를 상수로 선언하면 구조체 내부의 변수들도 모두 상수가 되어서 값을 변경할 수 없음
- 클래스는 참조타입, 인스턴스는 상수로 선언되어도 클래스 내부의 변수들을 변경할 수 있음.

> get() set()을 사용한 연산 프로퍼티
- 값에 접근할 때 get연산 수행
- 값을 변경할 때 set연산 수행
- 디폴트값은 new Value

```Swift

struct Stock {
    var averagePrice: Int
    var quantity: Int
    var purchasePrice: Int {
        get {   // 접근자
            return averagePrice * quantity
        }
        
        set(newPrice) { // 설정자
            averagePrice = newPrice / quantity
            // averagePrice = newValue / quantity
        }
    }
}
 var stock = Stock(averagePrice: 2300, quantity: 3)

print(stock)
stock.purchasePrice // purchasePrice에 접근할 때 get연산 수행 // 6900
stock.purchasePrice = 3000 // purchasePrice의 값을 변경할 때 set연산 수행  // 3000
stock.averagePrice // 1000 
// get만 구현해서 읽기전용 연산자로도 구현 가능
// set을 설정할 때 매개변수 이름을 지정해주지 않으면 디폴트값인 newValue로 사용 가능

```

 > cf. 프로퍼티 옵저버
    - 프로퍼티의 값의 변화를 관찰하고 반영
    - 새로운 값이 지정된 값과 같더라도 호출
    - 저장 프로퍼티, 오버라이딩된 저장 프로퍼티, 오버라이딩된 연산 프로퍼티
    
```Swift
// 저장프로퍼티에서 어떻게 사용되는지
class Account {
    var credit: Int = 0 {
        // 소괄호 이름 지정
        willSet {   // 값이 저장되기 직전에 호출, 새로 저장될 프로퍼티 값을 상수 매개변수로 전달, 매개변수 이름 넣어주지 않으면 디폴트는 newValue
            print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
        }
        didSet {    // 값이 저장된 직후에 호출, 프로퍼티의 기존 값이 상수 매개변수로 전달, 매개변수 이름 넣어주지 않으면 디폴트는 oldValue
            print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
        }
    }
}

var account = Account()
account.credit = 1000

/* 잔액이 0원에서 1000원으로 변경될 예정입니다.
잔액이 0원에서 1000원으로 변경되었습니다. */

```

> 타입 프로퍼티
```Swift
struct SomeStructure {
    static var storedTypeProperty = "Some Value."   // 스토어
    static var computedTypeProperty: Int { // 컴퓨티드
        return 1
    }
}

SomeStructure.computedTypeProperty
SomeStructure.storedTypeProperty
SomeStructure.storedTypeProperty = "hello"
SomeStructure.storedTypeProperty
```

### 클래스와 구조체의 공통점
- 값을 저장할 프로퍼티를 선언할 수 있다.
- 함수적 기능을 하는 메서드를 선언할 수 있다.
- 내부 값에 .을 사용하여 접근할 수 있다.
- 생성자를 사용해 초기 상태를 설정할 수 있다.
- extension을 사용하여 기능을 확장할 수 있다.
- Protocol을 채택하여 기능을 설정할 수 있다.

### 클래스와 구조체의 차이점
- 클래스
    - 참조타입
    - ARC로 메모리를 관리
    - 상속이 가능
    - 타입캐스팅을 통해 런타임에서 클래스 인스턴스의 타입을 확인할 수 있음
    - deinit을 사용하여 클래스 인스턴스의 메모리 할당을 해제할 수 있음
    - 같은 클래스 인스턴스를 여러 개의 변수에 할당한 뒤 값을 변경 시키면 모든 변수에 영향을 줌
    (메모리가 복사 됨)
- 구조체
    - 값 타입
    - 구조체 변수를 새로운 변수에 할당할 때마다 새로운 구조체가 할당됨
    - 즉 같은 구조체를 여러 개의 변수에 할당한 뒤 값을 변경시키더라도 다른 변수에 영향을 주지 않음
    (값 자체를 복사)

# 상속
- 연산 프로퍼티 저장 프로퍼티를 오버라이딩한 프로퍼티는 get, set 가질 수 있고
-  자식 클래스에서 재정의하려는 프로퍼티는 슈퍼클래스의 프로퍼티 이름과 타입이 일치해야 한다.

- 슈퍼클래스에서 리드라이트로 선언된 프로퍼티를 서브클래스에서 리드온리로 선언할 수 없지만
- 슈퍼클래스에서 리드온리로 선언된 프로퍼티는 서브클래스에서 리드라이트로 오버라이딩 가능

- 상속된 프로퍼티에 오버라이드를 사용하여 프로퍼티 옵저버도 추가할 수 있다.
 
```Swift
 // 프로퍼티옵저버 코드
 class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    
    func makeNoise() {
        print("speaker on")
    }
}

class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}

let car = Car()
car.currentSpeed = 30.0
car.gear = 2
print(car.description)

class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10) + 1
        }
    }
}

let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
```

 - 프로퍼티 재정의를 통해 currentSpeed의 값이 변경되면 프로퍼티 옵저버를 통해 gear프로퍼티의 값이 변경되게 해줄 수 있음
 - 상속된 프로퍼티에 오버라이드를 사용하면 프로퍼티 옵저버를 추가할 때는 상수 저장 프로퍼티나 리드온리 연산 프로퍼티는 프로퍼티 옵저버를 추가할 수 없는데 그 이유는 값을 설정할 수 없기 때문에 willSet이나 didSet을 사용할 수 없기 때문이다.

 - 슈퍼클래스의 메서드나 프로퍼티 앞에 final키워드를 작성하면 오버라이드 불가
 - 클래스 앞에 작성하면 해당클래스를 슈퍼클래스로 하는 서브클래스 작성 불가

### 타입캐스팅
- 인스턴스의 타입을 확인하거나 어떠한 클래스의 인스턴스를 해당 클래스 계층 구조의 슈퍼클래스나 서브 클래스로 취급하는 방법
- is, as
    - 값의 타입을 확인하거나 값을 다른 타입으로 변환하는데 사용

```Swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}

// library배열은 Movie클래스와 Song클래스의 부모클래스인 MediaItem 타입이 된다
let library = [
Movie(name: "기생충", director: "봉준호"),
    Song(name:"Butter", artist: "BTS"),
    Movie(name: "올드보이", director: "박찬욱"),
    Song(name:"Wonderwall", artist: "Oasis"),
    Song(name:"Rain", artist: "이적")]

var movieCount = 0
var songCount = 0

// is 연산자 사용
for item in library {
    if item is Movie {      // movieCount = 2
        movieCount += 1
    } else if item is Song {    // songCount = 3
        songCount += 1
    }
}

// 형을 변환할 수 있는 다운캐스팅
// 특정 클래스 타입의 상수 또는 변수는 하위클래스의 인스턴스를 참조할 수 있다.
// 다운캐스팅은 실패할 수 있기 때문에 as? as! 두가지 키워드 사용
// 조건부 형식인 as? 다운캐스팅하려는 타입의 옵셔널 형태로 반환
// as! 강제로 언래핑하여 값을 반환, 확실할 때 사용


for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}
```

### assert, guard
- assert
    - 특정 조건을 체크하고, 조건이 성립되지 않으면 메세지를 출력하게 할 수 있는 함수
    - assert함수는 디버깅 모드에서만 동작하고 주로 디버깅 중 조건의 검증을 위하여 사용

- guard
    - 뭔가를 검사하여 그 다음에 오는 코드를 실행할지 말지 결정하는 것
    - guard문에 주어진 조건문이 거짓일 때 구문이 실행됨

```Swift
// assert
var value = 0
assert(value == 0)

value = 2
assert(value == 0, "값이 0이 아닙니다")

/* __lldb_expr_1/MyPlayground.playground:226: Assertion failed: 값이 0이 아닙니다 */

```

### 프로토콜
- 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진
- 클래스에서 프로토콜을 채택하려면 상속받은 슈퍼클래스의 이름을 먼저쓰고 그다음에 프로토콜 쓰기
    - class SomeClass: SomeSuperClass, FirstProtocol, AnotherProtocol {

    }
- 프로퍼티 요구사항
    - 프로토콜은 자신을 채택한 타입이 어떤 프로퍼티를 구현해야 하는지 요구할 수 있음
    - 프로토콜이 프로퍼티를 준수하도록 정의할 때 저장프로퍼티인지 연산프로퍼티인지 지정하지 않고 프로퍼티 이름과 티입만 지정하면 된다.
    - 읽기만 가능한지, 읽기 쓰기만 가능한지 get과 set이용해서 지정

- 매서드 요구사항
    - 메서드 문법은 작성하지 않아도되고 생성자 키워드와 매개변수만 작성

```Swift
protocol FirstProtocol {    // 반드시 var (변수)로 지정
    var name: Int { get set }   // 읽기쓰기 가능
    var age: Int { get }        // 읽기전용
}

// 타입프로퍼티 요구하려면 static 키워드
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}

protocol FullyNames {
    var fullName: String { get set }
    func printFullName()
}

struct Person: FullyNames {
    var fullName: String
    
    func printFullName() {
        print(fullName)
    }
}

```

- 생성자 요구사항
    - 생성자 문법은 작성하지 않아도되고 생성자 키워드와 매개변수만 작성

```Swift
// 이니셜라이저(생성자) 요구사항
 
protocol SomeProtocol3 {
    func someTypeMethod()
}

// 생성자 문법은 작성하지 않아도되고 생성자 키워드와 매개변수만 작성
protocol SomeProtocol4 {
    init(someParameter: Int)
}

protocol SomeProtocol5 {
    init()
}

// 클래스에서 프로토콜이 요구하는 생성자를 채택하려면 required 식별자 사용
// 구조체에서는 required 식별자 필요 없음
// 클래스 자체가 상속받을 수 없는 final 클래스라면 required 식별자 붙여 줄 필요가 없다.
class SomeClass: SomeProtocol5 {
    required init() {
        
    }
}
```

### 프로토콜
- 기존의 클래스, 구조체, 열거형, 프로토콜에 새로운 기능을 추가하는 기능
    - 연산 타입 프로퍼티 / 연산 인스턴스 프로퍼티
    - 타입 메서드 / 인스턴스 메서드
    - 이니셜라이저
    - 서브스크립트
    - 중첩 타입
    - 특정 프로토콜을 준수할 수 있도록 기능 추가
- 기존에 있는 기능을 오버로이드할 수는 없다
- 저장프로퍼티는 추가할 수 없다
- 타입에 정의되어있는 기존의 프로퍼티에 프로퍼티 감시자 추가할 수 없다

```Swift
// 연산프로퍼티 기능 추가
// Int 타입 값이 짝수인지 홀수인지
extension Int {
    var isEven: Bool {
        return self % 2 == 0
    }
    
    var isOdd: Bool {
        return self % 2 == 1
    }
}

var number = 3
number.isOdd
number.isEven

// 문자열을 Int타입으로 변환
extension String {
    func convertToInt() -> Int? {
        return Int(self)
    }
}

var string = "0"
string.convertToInt()
```

### 열거형
- 연관성이 있는 값을 모아 놓은 것을 말한다.
- 하나의 새로운 타입으로 사용할 수 있다.
- 열거형 타입에 값을 대입할 때 .키워드로 열거형이름을 생략하고 내부항목 이름만으로 사용해서 가능

```Swift
enum CompassPoint: String { // String 타입의 원시값 가지게 해줌
    case north = "북"
    case south = "남"
    case east = "동"
    case west = "서"
}

var direction = CompassPoint.east   // 컴파일러가 추론 가능
direction = .west

// switch와 사용하기 좋음
// 원시값 출력
switch direction {
case .north:
    print(direction.rawValue)
case .south:
    print(direction.rawValue)   // 서
case .east:
    print(direction.rawValue)
case .west:
    print(direction.rawValue)
    
}

// 원시값을 가지고 열거형 반환하기
// 열거형 인스턴스 생성할 때 매개변수로 원시값 넣어줌
let direction2 = CompassPoint(rawValue: "남")

// 열거형은 연관값을 갖을 수 있다.
// 열거형 내의 항목이 자신과 연관된 값을 가진다.
// 모든 항목이 갖을 필요는 없음
enum PhoneError {
    case unknown
    case batteryLow(String) // 항목옆에 연관값타입을 소괄호로 묶어줌
}

let error = PhoneError.batteryLow("배터리가 곧 방전됩니다.")

// if 또는 Switch 문으로 연관값 추출 가능
switch error {
case .batteryLow(let message):
    print(message)
    
case .unknown:
    print("알 수 없는 에러입니다.")
}
// 에러변수가 batteryLow이니 스위치 문 .batteryLow에 걸리고 거기서 연관값을 추출한다.
```

### 옵셔널 체이닝
- 옵셔널에 속해 있는 nil일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정

```Swift
// 옵셔널 체이닝
struct Developer {
    let name: String
}

struct Company {
    let name: String
    var developer: Developer?
}

var developer = Developer(name: "han")
var company = Company(name: "Gunter", developer: developer)
print(company.developer)
print(company.developer?.name)  //Optional("han")
print(company.developer!.name)  //han

// Optinal 벗겨내고싶으면 옵셔널 바인딩 사용해야함
```

### try - catch
- 프로그램 내에서 에러가 발생한 상황에 대해 대응하고 이를 복구하는 과정
- 스위프트에서는 런타임에 에러가 발생할 경우
    - 발생 (throwing)
    - 감지 (catching)
    - 전파 (propagating)
    - 조작 (manipulating)
- 을 지원하는 일급 클래스를 지원

```Swift
enum PhoneError: Error {
    case unknown
    case batteryLow(batteryLevel: Int)
}

// throw 통해서 오류 발생시키기
throw PhoneError.batteryLow(batteryLevel: 20)

/*
Playground execution terminated: An error was thrown and was not caught:
▿ PhoneError
  ▿ batteryLow : 1 element
    - batteryLevel : 20

*/
```

- 던져진 오류를 처리하는 4가지 방법
    1. 함수에서 발생한 오류를 해당 함수를 호출한 부분에 전파
    2. do - cath 구분
    3. optional값으로 처리 - try?
    4. 오류가 발생하지 않는다고 확신 - try!

```Swift
// 1. 오류가 발생할 수 있음을 나타내기 위해서는 함수, 매개변수, 생성자 매개변수 뒤에 throws라는 키워드
// throwing 함수: guard문과 throw문으로 오류를 처리
func checkPhoneBatteryStatus(batteryLevel: Int) throws -> String {
    guard batteryLevel != -1 else { throw PhoneError.unknown }
    guard batteryLevel > 20 else { throw PhoneError.batteryLow(batteryLevel: 20)}
    return "배터리 상태가 정상입니다."
}

// 2. 함수, 매서드, 생성자 등에서 오류를 던져주면 오류 발생을 전달받은 코드블럭은 do-catch구문을 사용해 오류를 처리
// do 내부 코드에서 오류를 던지고 catch에서 전달받아 예외 처리

/*
 do {
 try 오류 발생 가능코드
 } catch 오류 패턴 {
 처리 코드
 }
 */

do {
    try checkPhoneBatteryStatus(batteryLevel: 20)   // 오류가 발생함
} catch PhoneError.unknown {
    print("알 수 없는 에러입니다.")
} catch PhoneError.batteryLow(let batteryLevel) {
    print("배터리 전원 부족 남은 배터리: \(batteryLevel)%")
} catch {
    print("그 외 오류 발생 \(error)")
}

// 3. try? 사용하면 오류를 옵셔널로 변환하여 처리 가능, try? 통해 동작하던 함수가 오류를 발생시키면 코드의 반환값은 nil이 된다.

let status = try? checkPhoneBatteryStatus(batteryLevel: -1)
print(status)   // nil

let status1 = try? checkPhoneBatteryStatus(batteryLevel: 30)
print(status1)   // Optional("배터리 상태가 정상입니다.")

// 4. try! 오류가 발생하지 않는다고 확신
let status2 = try! checkPhoneBatteryStatus(batteryLevel: 30)
print(status2)  // 배터리 상태가 정상입니다.

let status3 = try! checkPhoneBatteryStatus(batteryLevel: -1)
print(status3)  // 강제 종료됨


````

### 클로저
- 참조 타입이고 코드에서 전달 및 사용할 수 있는 독립기능이며 일급객체 역할
- 일급객체란
    - 전달 인자로 보낼 수 있고, 변수/상수 등으로 저장하거나 전달할 수 있으며, 함수의 반환 값이 될 수도 있다.
- 이름없는 함수, 익명함수

- { (매개 변수) -> 리턴 타입 in
        실행 구문
  }

```Swift
// 클로저
// 파라미터와 리턴타입이 둘 다 없는 클로저
// 스위프트에서 함수(클로저)는 일급 객체이기 때문에 상수나 변수에 클로저 대입할 수 있다


let hello = { () -> () in
    print("hello")
}

hello()

// 파라미터와 리턴타입이 있는 클로저
// 클로저에서는 전달인자 레이블 사용하지 않음
let hello2 = { (name: String) -> String in
    return "Hello, \(name)"
}

//hello2(name: "Sunghee") // error
hello2("Sunghee")   // Hello, Sunghee

// 함수의 파라미터 타입으로 클로저 전달 가능
// doSomething함수 호출되면 closure 호출
func doSomething(closure: () -> ()) {
    closure()
}

doSomething(closure: {() -> () in
            print("hello")
})

// 함수의 반환 타입으로 클로저 사용
func doSomething2() -> () -> () {
    return { () -> () in
        print("hello4")
    }
}

doSomething2()()    // ⭐️

```

- ⭐️ doSomething2함수의 반환타입이 함수인데
doSomething2함수는 doSomething2()으로 호출한거구
doSomething2()의 반환타입 함수에 대한것을 호출 안해서 print가 안직히는거징!
그래서 doSomething2()() 이렇게하면
doSomething2()의 반환타입 클로저를
()를 한번 더써서 호출하니깐 print가 찍히는거야!

doSomething2() <- doSomething2 함수호출
근데 doSomething2 <— 얘의 리턴타입이 함수자나!(클로저)
클로저도 호출할수있으니깐
doSomething2()()
이렇게하면
1. doSomething2 호출
2. doSomething2이 클로저 반환
3. 반환된 클로저를 호출

즉위의 코드는
let closure = doSomething2()
closure() 
변경이 가능한거징!


- 클로저가 길어지거나 가독성이 떨어지면 후행 클로저로 바꾸어 쓸 수 있음
- 맨 마지막 매개변수로 전달되는 클로저에만 해당됨

```Swift
doSomething() { // 후행 클로저로
    print("hello2")
}

doSomething {   // 단 하나의 클로저만 매개변수로 전달하는 경우에는 소괄호도 생략 가능
    print("hello3")
}
```

- 매개변수에 클로저가 여러개 있는 경우 다중 후행 클로저 사용 가능
```Swift
func doSomething2(success: () -> (), fail: () -> ()) {
    
}

// xcode 자동완성 기능, 중괄호를 열고 닫음으로써 클로저를 표현하고 첫번째 매개변수 이름은 생략
doSomething2 {
    <#code#>
} fail: {
    <#code#>
}
```

- 클로저 표현을 단순화
- 문법을 사용해서 클로저 표현을 단순화할 수 있다.

```Swift
func doSomething3(closure: (Int, Int, Int) -> Int) {
    closure(1,2,3)
}

// 표현을 단순화하지 않고 표현
doSomething3(closure: { (a, b, c) in
    return a + b + c
})

// 경량 문법으로 단순하게 파라미터형식 생략, 약식인수(매개변수 이름 대신하여 사용) 이용해서 매개변수 이름 생략
doSomething3(closure: {
    return $0 + $1 + $2
})

// 단일 리턴문만 남으면 리턴타입도 생략 가능
doSomething3(closure: {
    $0 + $1 + $2
})

// 후행클로저로 작성 가능
doSomething3() {
    $0 + $1 + $2
}

// 단하나의 클로저만 매개변수로 전달하는 경우는 소괄호도 생략 가능
doSomething3 {
    $0 + $1 + $2
}
```

- etc
    - @escaping 
        - 탈출클로저
    - 강한순환참조
        - 클래스 인스턴스의 프로퍼티로 클로저를 할당하면 클로저와 인스턴스사이에 강한순환참조가 생겨 메모리 릭 발생

### 고차함수
- 다른 함수를 전달 인자로 받거나 함수 실행의 결과를 함수로 반환하는 함수
- 스위프트의 함수는 일급객체이기 때문에 함수를 함수의 전달인자로 전달할 수 있고 함수의 반환값으로 함수를 반환할 수 있다.
- map, filter, reduce -> collection type

- map
    - 컨테이너 내부의 기존 데이터를 변형하여 새로운 컨테이너를 생성함
```Swift
let numbers = [0, 1, 2, 3]
let mapArray = numbers.map { (number) -> Int in
    return number * 2
}

print("map \(mapArray)")    // map [0, 2, 4, 6]
```

- filter
    - 컨테이너 내부의 값을 걸러서 새로운 컨테이너로 추출
```Swift
let intArray = [10, 5, 20, 13, 4]
let filterArray = intArray.filter { $0 > 5 }
print("filter \(filterArray)")  // filter [10, 20, 13]
```

- reduce
    - 컨테이너 내부의 요소를 하나로 통합
```Swift
let someArray = [1, 2, 3, 4, 5]
let reduceResult = someArray.reduce(0) {
    (result: Int, element: Int) -> Int in
    print("\(result) + \(element)") // result 는 누적값, element 는 배열의 요소
    return result + element
}

print("reduce \(reduceResult)") // reduce 15
```


