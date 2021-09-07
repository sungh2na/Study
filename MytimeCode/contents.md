# Mrtime Code
## pod 파일
### pod 'lottie-ios', '~> 2.5'
    - 에어비엔비 자체적으로 만든 라이브러리
    - 벡터로 되어있어서 이미지 용량 줄여서 만들 수 있음

LaunchScrrenViewController.swift
- lottie 애니메이션 실행
- ResourceManager -> 싱글톤 객체로 선언 되어 있음
- 서버와 네트워크 통신 할 때, URL Session 대신에 Alamofire 이걸 한번 더 묶어준 것이 Moya
    - ResourceManager.checkLastVersion 에서 사용
