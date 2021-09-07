# Combine

'Combine' 프레임워크는 시간이 지남에 따라 값을 처리하기 위한 선언적 스위프트 API를 제공한다. 이러한 값은 여러 종류의 비동기 이벤트를 나타낼 수 있다. 게시자를 결합하여 시간이 지남에 따라 변경될 수 있는 값을 노출시키고, 가입자는 게시자로부터 해당 값을 수신한다.

- Apple 에서 직접 만든 Reactive 프로그래밍을 위한 프레임 워크
- iOS 13 이상 버전부터 사용이 가능
- 자체적으로 내장된 프레임워크 -> 시간이나 메모리 할당 면에서 성능이 더 우수
- SwiftUI와 UI바인딩을 하도록 설계되어짐

- Publisher, Cancellable, Subscriber, Subject, Scheduler가 존재

## Publisher

Publisher는 하나 이상의 Subscriber 인스턴스로 요소를 전달한다. Subscriber의 Input 및 Failure 관련 타입은 게시자가 선언한 Output 및 Failure 타입과 일치해야 한다. Publisher는 Subscriber를 받기 위해서 receive(subscruber:)방식을 구현해야 한다.

- Publisher를 채택하는 struct나 class를 생성하거나
- 자체적으로 Combine Framework에 내장되어 있는 AnyPublisher나 Convenience Publisher로 정의 되어 있는 Future, Just, Deffered, Fail, Record 사용

## Cancellable

- 이를 자체적으로 구현하여 사용하거나 Swift에서 자체적으로 제공하는 AnyCancellable 사용
- deinit될 때 자동으로 cancel이 됨

## Subscriber
- Subscriber 인스턴스는 Publisher로부터 요소의 Stream을 수신하며, 관계의 변화를 설명하는 라이프 사이클 이벤트도 함께 수신한다. 주어진 Subscriber의 Input 및 Failure 관련 유형은 해당 게시자의 출력 및 실패와 일치해야 한다.

```Swift
func sink(receiveCompletion: @escaping ((Subscribers.Completion<Never>) -> Void), receiveValue: @escaping ((ClosedRange<Int>.Element) -> Void)) -> AnyCancellable
```

- 파라미터인 receiveCompletion과 receiveValue은 클로져 형태로 받을 수 있으며 publisher를 통해 전달된 element를 받아오거나, 전달이 완료된 경우 동작에 대해 구현해 줄 수 있다.