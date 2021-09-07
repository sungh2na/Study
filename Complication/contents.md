# Create complications for Apple Watch
- https://developer.apple.com/videos/play/wwdc2020/10046/
- Apple Watch용 컴플리케이션 생성
- Watch 앱에 정보 표시를 추가하면 사람들이 시계 모드에서 직접 한눈에 볼 수 있는 최신 정보에 엑세스할 수 있다.
- 처음부터 컴플리케이션을 만들고 구축하는 방법을 보여주고 다중 컴플리케이션을 소개한다.
- 타임라인을 구성하고, 패밀리와 테플릿을 사용하고, 철저한 컴플리케이션 경험을 만들기 위한 모밤사례를 발견하는 법을 배우자.

- SwiftUI에서 그래픽 컴플리케이션을 구축하는 방법을 알게되면 이를 다중 컴플리케이션 및 와치페이스공유와 같은 다른 watchOS7기능과 결합하여 맞춤화한 시계페이스를 만들 수 있다.
- 앱에 어떻게 컴플리케이션을 추가할 수 있을까?

- Timelines
- Complication building blocks
- Providing data
- Multiple complications
- Example

- 컴플리케이션의 뒷받침 구조인 타임라인에 대해 알아야 한다. 진행하는데 필요한 모든 부분, 데이터를 컴플리케이션으로 가져오는 방법, 새로운 watchOS 7 기능을 확용하여 앱에서 둘 이상의 컴플리케이션을 제공하는 방법, 그리고 예제를 통해 실행해 본다. 타임라인에 대한 이야기부터 시작한다.

## Timeline
- A representation of your complication’s data over time
- Enables ClockKit to query your app once and get all the information needed
- Extend of invalidate as necessary to ask ClockKit to re-query your app

- 타임라인은 컴플리케이션에서 중심적인 역할을 한다. 시간 경과에 따른 컴플리케이션 데이터를 나타낸다.
- 타임라인은 컴플리케이션의 모든 데이터를 제공하기 때문에 ClockKit은 지정한 날짜까지 컴플리케이션을 자동으로 구동하는데 필요한 모든 정보를 앱에 한번에 요청할 수 있다.
- 물론 앱에서 새로운 데이터를 얻고 더 많은 항목을 제공해야 하는 경우, ClockKit에 타임라인을 연장하거나 완전히 무효화 하도록 요청할 수 있다.

## Complication Building Blocks
- 컴플리케이션에 표시할 항목을 실제로 지정하는 방법에 대해 알아봄
- 컴플리케이션 패밀리는 우리가 그것을 다른 시각적 그룹으로 나누는 방법이다. 그래픽 패밀리는 watchOS 7에 새로 추가된 Graphic Extra Large를 제외하고 watchOS 5에 도입되었다.
- 이러한 컴플리케이션 패밀리를 사용하면 데이터를 보다 시각적으로 표시할 수 있다. 다른 시계모드는 다른 컴플리케이션 패밀리를 사용한다. 
    - Graphic Corner
    - Graphic Bezel
    - Graphic Circular
    - Graphic Rectangular
    - Graphic Extra Large

    - Modula Small
    - Modular Large
    - Utilitarian Small Flat
    - Utilitarian Samall
    - Utilitarian Large
    - Extra Large

- 이상적으로는, 컴플리케이션에 대한 가능한 많은 컴플리케이션 패밀리를 지원하기를 원할 것이다. 이렇게 하면 모든 사용자가 원하는 페이스에 컴플리케이션을 사용할 수 있다.

- 컴플리케이션 템플릿은 패밀리 내의 다양한 시각적 레이아웃을 나타낸다. 다음은 몇가지 다른 제품군에서 사용할 수 있는 일부 컴플리케이션 템플릿의 예이다.
- **CLKComplicationTemplate**
    - GraphicCircularImage
    - GraphicCircularStackImage
    - GraphicCircularStackText
    - GraphicCircularOpenGaugeImage
    - > CLKComplicationFamily GraphicCircular
    -
    - GraphicCornerCircularImage
    - GraphicCornerGaugeImage
    - GraphicCornerStackText
    - GraphicCornerGaugeText
    - > CLKComplicationFamilyGraphicCorner
    -
    - GraphicRectangularLargeImage
    - GraphicRectangularTextGauge
    - GraphicRectangularStandardBody
    - GraphicRectangularFullImage
    - > CLKComplicationFamilyGraphicRectangular

- 특정 패밀리와 연결되어 있음에도 불구하고 각 템플릿은 공통 기본 클래스인 CLKComplicationTemplate에서 상속한다.
- 표시하려는 데이터를 가장 잘 자나태기 위해 선택할 수 있는 옵션이 많이 있다.
- 개발자 웹 사이트의 설명서에서 사용 가능한 항목에 대해 자세히 알아볼 수 있다.
- 이제 타임라인을 제공하기 위한 코드의 간단한 에를 살펴 보겠다. 첫번째 유형은 CLKComplicationTimelineEntry이다.
- 이러한 목록을 제공하여 컴플리케이션 타임라인을 채운다. 각각은 특정 시점에서 컴플리케이션이 어떻게 생겼는지 나타낸다. 

```Swift
// CLKComplicationTimelineEntry
Class CLKComplicationTimelineEntry: NSObject {
	var date: Date
	var complicationTemplate: CLKComplicationTemplate
}
```

- 이 항목이 표시되어야 하는 날짜인 date
- 이 항목에 대해 표시하려는 데이터가 포함된 템플릿인 complicationTemplate의 두가지 속성만 있다.

- ClockKit과의 주요 상호작용은 CLKComplicationDataSource를 준수하는 생성 객체를 통해 이루어진다. 이 프로토콜에는 단 하나의 필수 메소드가 있다. 주어진 컴플리케이션에 대한 현재 타임라인 항목으로 핸들러를 호출하기만 하면 이를 구현할 수 있다.

```Swift
// CLKComplicationDataSource - required
Class ComplicationController:  NSObject, CLKComplicationDataSource {
	func getCurrentTimelineEntry(
		for complication: CLKComplication,
		withHandler handler: @escaping (CLKComplicationTimelineEntry?) -> Void)
	{
		// Call the handler with the current timeline entry
		handler(createTimelineEntry(forComplication: complication, date: Date()))
	}
}
```

- 일부 컴플리케이션의 경우 현재 항목으로 충분하다. (예: 야구 경기의 현재 점수 또는 현재 Apple 주가). 그러나 정보 표시가 미래의 항목에 있는 타임라인을 제공할 수 있는 경우 다음 두 가지 방법도 구현해야 한다.

```Swift
// CLKComplicationDataSource - Timeline support
Extension ComplicationController {
	
	func getTimelineEndDate(
		for complication: CLKComplication,
		withHandler handler: @escaping (Date?) -> Void)
	{
		handler(timeline(for: complication)?.endDate)
	}
	
	func getTimelineEntries(
		for complication: CLKComplication,
		after date: Date,
		limit: Int,
		withHandler handler: @escaping ([CLKComplicationTimelineEntry]?) -> Void)
	{
		handler(timeline(for: complication)?.entries(after: date, limit: limit))
	}
}
```

getTimelineEndDate with Handler 앞으로 어느정도 미래까지 항목을 제공할 수 있는지를 지정한다.
GetTimelineEntries with Handler 날짜 제한 이후 정보 표시?
그것은 약간이지만 주어진 날짜 이후에 한도까지 보유한 데이터에 대해 적절한 만큼 많은 항목을 제공하기만 하면 된다.
날짜는 우리가 이미 가지고있는 마지막 타임라인 항목을 나타낸다. 제한은 한번에 필요한 만큼만 항목을 가져오는 것이다. 더 많이 제공하면 한도를 초과하는 모든 항목을 삭제한다.
앰에 새 데이터가 있고 타임라인을 다시 로드해야 하는 경우 어떻게 하겠는가? 두가지 옵션이 있다.

// Reloading the Timeline
Class CLKComplicationServer: NSObject {
	func reloadTimelind(for complication: CLKComplication)

	func extendTimeline(for complication: CLKComplication)

	var activeComplication: [CLKComplication]?
}

먼저 데이터가 완전히 변경되고 제공한 모든 항목이 유효하지 않은 경우
CLKComplicationServer의 공유 인스턴스에서 정보 표시를 위해 reloadTimeline을 호출할 수 있다.
예를들어, 에상치 못한 폭풍이 마우이에 오면 모든 고래 관찰 투어가 취소되고 이를 반영하기 위해 전체 일정을 무효화 하려고 한다.

그러나 이전에 제공한 항목이 여전히 유효하고 더 제공할 수 있음을 알리고 싶다면 복잡한 문제를 위해 extendTimeline을 호출하여라.

ClockKit은 현재 사용자의 시계 화면에 있는 컴플리케이션에 대한 타임라인만 추적한다. activeComplications 속성을 통해 이에대해 알려준다.

CLKComplication 객체를 직접 만들지 않는다. 항상 여기에서 가져오거나 구현하는 데이터 소스 메서드로 전달한다.

Providing data
Complication data needs to adapt to different sizes, layouts, and styles
Space is often constrained
Tell us your intentions
We handle the formatting

이제 이러한 컴플리케이션 템플릿을 만드는 데 실제로 무엇이 필요한지 이야기해 보깄다.
컴플리케이션에는 몇가지 제한 사항이 있다. 시계화면이 작고 컴플리케이션은 더 작다. 레이아웃 제약조건이 매우 다른 템플릿 또는 컴플리케이션 패밀리에 동일한 문자열을 표시할 수 있다.
최상의 경험을 제공하기 위해 data provider라는 개념이 있다. 이를 통해 ClockKit에서 형식화된 다양한 위치와 컨텍스트에서 동일한 정보를 일관되게 표현할 수 있다.
귀하를 대신해 컴플리케이션의 특정 레이아웃 세부 정보를 처리하므로 유연하게 수행할 수 있는 충분한 정보가 필요하다.

Text Providers
- Wednesday, September 23

텍스트로 시작한다. 시계는 시간에 관한 것이므로 날짜를 표시하는 것은 자주 하고 싶은 일이다. 여기에 긴 날짜를 기록하는 좋은 방법이 있다.
하지만 대부분의 컴플리케이션 텍스트에서 이런식으로 끝난다. 그것이 무엇을 말하는지 명확하지 않다. 그럼 어떻게하면 더 잘 할 수 있을까?
CLKDateTextProvider라는 데이터 공급자를 사용한다. 이 경우 Wednesday, September 23과 같이 원하는 것을 선언하면 최선을 다해 보여준다.
그러나 공간이 제한되면 더 짧은 버전으로 대체하기 시작한다. 
- Wednesday, Sep 23
결국 평일과 같이 덜 유용한 정보를 삭제하고 필요한 경우 해당 월의 날짜까지 모두 삭제한다.
- Wed, Sep 23
- Sep 23
- 23

코드로 보면 다음과 같다. 
Let longDate: Date = …
Let units: NSCalendar.Unit = [.weekday, .month, .day]
Let textProvider = CLKDateTextProvider(date: longDate, units: units)

가장 긴 경우 선호하는 날짜와 달력 단위를 제공하여 CLKDateTextProvider를 만든다. “ 내 날짜가 현재 시간과 얼마나 떨어져 있습니까?” 또는
 “다른 날짜에서 내 날짜는 얼마나 떨어져 있습니까?” 같은 질문에 답하고 싶다면 CLKRelativeDateTextProvider를 사용할 수 있다. 
첫번째 질문의 경우 상대 날짜 텍스트 공급자는 현재 시간이 무엇이든 상관없이 텍스트를 자동 업데이트 한다. 이것은 추가 작업 없이 초단위까지 정확할 수 있다.

다양한 형식으로 사용할 수 있다. 문자열, 지금부터 일몰까지의 시간, 또는 다음과 같은 문자열을 표시할 수 있다. 만들고 있는 반죽이 부풀어 오를 떄까지 남은 시간

Clock kit
- 와치페이스에 앱별 데이터를 표시합니다.
- Clock Kit 프레임워크를 사용하여 앱에 대한 컴플리케이션을 구현한다. 컴플리케티션은 시계 모드에 표시되는 작은 인터페이스 요소로 사람들이 자주 사용하는 데이터에 빠르게 엑세스할 수 있다. 앱은 컴플리케이션의 모양과 Clock kit이 표시하는 날짜에 대한 템플릿을 제공하는 타임라인 항목을 사용하여 컴플리케이션을 정의한다.
- 시스템은 타임라인에 따라 컴플리케이션 모양을 업데이트 한다. 
- ClockKit은 각 시계 모드에서 컴플리케이션의 크기와 배치를 정의한다. Clock Kit은 컴플리케이션을 패밀리 크기와 스타일에 따라 분류하고 패밀리별로 다양한 템플릿을 제공한다.
- 템플릿을 사용하여 텍스트, 이미지, 그래픽게이지를 표시한다. 

Creating a Timeline Entry
- 타임라인 항목 생성
- 앱별 데이터를 템플릿으로 패키징하고 해당 템플릿에 대한 타임라인 항목을 만든다.
- ClockKit은 컴플리케이션의 데이터소스에서 타임라인 항목을 주기적으로 요청한다.
- 타임라인 항목은 앱 데이터로 구성한 정보표시 템플릿과 ClockKit이 해당 템플릿을 표시하는 시기를 지정하는 날짜로 구성된다.
- 데이터소스는 앱별 데이터를 ClockKit이 화면에 표시할 수 있는 템플릿으로 패키징한다.
- 템플릿을 만드려면 제공된 CLKComplication개체를 검사하고 적절한 템플릿을 선택하고 템플릿을 앱의 데이터로 채운 다음 템플릿에 대한 타임라인 항목을 만든다.

Examine the Complication Object
- 컴플리케이션 객체 설명
- ClockKit은 템플릿을 요청할 때 항상 컴플리케이션을 설명하는 CLKComplication 을 제공한다. 컴플리케이션은 앱을 정의하는 identifier와
-  CLKComplicationFamily 열거형의 Family를 갖는다.
- 각 identifier는 지정된 family 에 대한 별도의 컴플리케이션 타입을 정의한다.
- identifier가 어떤 종류의 컴플리케이션을 만들고 있는지 확인해라. wathOS 7 이상에서 너의 앱은 family당 하나또는 더 많은 컴플리케이션을 제공한다. 
- 예를들어 날씨 앱은 기후, 온도, 강수량에대한 분리된 컴플리케이션을 지원한다.
witch complication.identifier {
case complicationConditionIdentifier:
    templateOrNil = myGetConditionTemplate(for: family, date: date)
    
case complicationTemperatureIdentifier:
    templateOrNil = myGetTemperatureTemplate(for: family, date: date)
    
case complicationPrecipitationIdentifier:
    templateOrNil = myGetPrecipitationPercentageTemplate(for: family, date: date)

- clockKit 프레임워크는 컴플리케이션을 family로 구성한다. 각 family는 컴플리케이션의 사이즈와 모양을 정의한다.
- family가 어떤 종류의 템플릿을 사용할 수 있는지 알아보자
- - 전체 패밀리 리스트는 CLKComplicationFamily를 참고
switch complication.family {
case .circularSmall:
    // Create a template from the circular small family.
    
case .modularSmall:
    // Create a template from the modular small family.
    
case .modularLarge:
    // Create a template from the modular large family.
    
// Continue with all the templates supported by the specified type.
    
@unknown default:
    print("*** Unknown Complication Family: \(complication.family) ***")
    // Handle the error here.
}

- 각 family는 컴플리케이션 내에서 데이터의 위치와 데이터 타입을 정의하는 하나 이상의 템플릿을 제공한다.
- 다음 페이지에서 지정된 패밀리에 대한 템플릿을 선택하고 생성한다.
* Circular Small
* Extra Large
* Modular Small
* Modular Large
* Utilitarian
* Graphic
* 
- Memo
- watchOS 6 이전 버전의 경우 앱은 Family당 하나의 컴플리케이션 정보만 제공할 수 있다.
- 이러한 버전의 watchOS를 지원하려면 사용할 템플릿을 결정할 때 컴플리케이션의 family 속성만 확인하면 된다.


Create Data Providers to Fill the Template
- 템플릿을 채울 data provider만들기
- Data provider는 원시 값을 가져와서 템플릿에 맞게 형식을 지정한다. 예를들어, CLKSimpleTextProvider는 두개의 문자열(짧은 문자열과 긴 문자열)을 갖는다.
- 둘다 제공하면 ClockKit은 템플릿 크기에 가장 적합한 문자열을 선택한다.
let temperature = myCity.temperature(date)


let nameProvider = CLKSimpleTextProvider(
    text: myCity.name,
    shortText: myCity.abbreviation)


let temperatureProvider = CLKSimpleTextProvider(
    text: "\(longNumberFormatter.string(from: NSNumber(value: temperature)) ?? "Unknown")º F",
    shortText: shortNumberFormatter.string(from: NSNumber(value: temperature)) ?? "??")

- ClockKit은 다음을 포함하는 텍스트, 이미지, 게이지에 대한 데이터 공급자를 사용한다.
* CLKSimpleTextProvider
* CLKDateTextProvider
* CLKTimeTextProvider
* CLKRelativeDateTextProvider
* CLKTimeIntervalTextProvider
* CLKImageProvider
* CLKFullColorImageProvider
* CLKSimpleGaugeProvider
* CLKTimeIntervalGaugeProvider

- CLKRelativeDateTextProvider 와 CLKTimeIntervalGaugeProvider 같은 몇가지 데이터 프로바이더는 컴플리케이션의 백그라운드 실행 예산에 영향을 주지 않고 시간이 지남에 따라 컴플리케이션을 자동적으로 업데이트한다.  
- 데이터 프로바이더를 사용하여 템플릿을 만들고 채운다.
let template = CLKComplicationTemplateModularSmallStackText(
    line1TextProvider: temperatureProvider,
    line2TextProvider: nameProvider)

Use Conplication Data
- 컴플리케이션 데이터 사용
- CLKComp,icationDescriptor 클래스의 userInfo와 userActivity 속성은 컴플리케이션에 대한 추가적인 데이터를 전달하게 해준다. 이 데이터를 사용하여 타임라인 항목 생성을 단순화하고 앱에서 특정 장면을 시작할 수 있다.
- 예를들어, 사용자가 즐겨찾는 도시 목록에서 컴플리케이션 집합을 만들 수 있다. Dynamically Define Descriptors(동적으로 설명자 정의)를 보여줌으로써 
- 컴플리케이션 identifier를 파싱하는 대신에, 도시 및 날씨 데이터 유형을 컴플리케이션 userInfo 사전에 추가할 수 있다.
- 그런다음 타임라인 항목을 생성하기 위해 템플릿을 선택하고 채우기 전에 사전으로부터 데이터를 읽는다.
let city: City


// Read the city id from the complication's userInfo dictionary.
if let cityID = complication.userInfo?[myCityIDKey] as? String {
    city = myData.lookupCity(byID: cityID) ?? myData.currentCity
} else {
    city = myData.currentCity
}


let nameProvider = CLKSimpleTextProvider(
    text: city.name,
    shortText: city.abbreviation)


// Read the complication type from the userInfo dictionary.
let typeIdentifier = complication.userInfo?[myTypeIdentifierKey] as? String ?? "Undefined"


Create the Timeline Entry Object
- 타임라인 항목 객체 만들기
- 템플릿과 원하는 날짜를 사용하여 CLKComplicationTimelineEntry 객체를 만든다. 현재 타임라인 항목의 경우, 현재 시간과 같거나 이전 날짜를 지정해야 한다.
CLKComplicationTimelineEntry(date: Date(), complicationTemplate: template)

- 향후 타임라인 항목에서 데이터가 컴플리케이션에 표시될 날짜와 시간을 지정하라
- 미팅 컴플리케이션에서, 예정된 시작 시간 전에 회의에 대한 정보를 표시할 수 있다.
- 미래 타임항목 생성에 대한 자세한 내용은 Loading Future Timeline Events를 참조하라.

https://developer.apple.com/videos/play/wwdc2020/10048
https://developer.apple.com/videos/play/wwdc2020/10049
https://developer.apple.com/videos/play/wwdc2021/10002/