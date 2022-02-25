## Initialization (Default Initializers 까지만)

___초기화___는 클래스, 구조체, 열거형의 인스턴스를 사용하기 위해 준비하는 과정이다. 해당 인스턴스에서 저장 프로퍼티에 초기 값을 설정하고, 새로운 인스턴스를 사용할 준비되기 전에 필요한 다른 설정 또는 초기화를 수행한다.

특정 타입의 새로운 인스턴스를 생성하기 위해 호출되는 특별한 메소드인 이니셜라이저(initializer)를 정의하는 것으로 초기화 과정을 구현한다. Objective-C의 이니셜라이저와는 달리 Swift의 이니셜라이저는 값을 반환하지 않는다. 이니셜라이저의 주요 역할은 새로운 인스턴스가 처음 사용되기 이전에 올바르게 초기화되도록 하는 것이다.

클래스 타입의 인스턴스는 디이니셜라이저(deinitializer)를 구현할 수 있다. 이는 해당 클래스의 인스턴스가 할당 해제되기 전에 사용자 지정을 초기화한다.

### 저장 프로퍼티에 초기 값 설정하기
클래스와 구조체는 해당 인스턴스가 생성될 때 모든 저장 프로퍼티에 적절한 초기 값을 설정해야 한다. 저장 프로퍼티는 알 수 없는 상태로 존재할 수 없다.

이니셜라이저를 통해 저장 프로퍼티에 초기 값을 설정하거나, 프로퍼티의 정의의 일부로 프로퍼티의 기본 값을 할당하여 설정할 수 있다.
> 저장 프로퍼티에 기본 값을 할당하거나 이니셜라이저 내에서 초기 값을 설정하면 프로퍼티 옵저버를 호출하지 않고 해당 프로퍼티의 값이 직접적으로 설정된다.

#### 이니셜라이저(Initializers)
이니셜라이저는 특정 타입의 새로운 인스턴스를 생성하기 위해 호출된다. 가장 단순한 형태의 이니셜라이저는 init키워드를 사용하여 매개변수가 없는 인스턴스 메소드처럼 작성하는 것이다.
```swift
init() {
    // perform some initialization here
}
```

#### 프로퍼티 기본 값
이니셜라이저 내에서 저장 프로퍼티의 초기 값을 설정하는 대신에, 프로퍼티 선언의 일부로 프로퍼티의 기본 값을 할당하여 저장할 수 있다. 프로퍼티가 정의될 때 초기 값을 기본 값으로 할당한다.
> 프로퍼티가 항상 동일한 초기 값을 사용한다면, 이니셜라이저 내에서 값을 설정하기보다는 기본 값을 제공하는 것이 좋다. 결과는 동일하지만 기본 값은 프로퍼티의 선언과 초기화를 더 밀접하게 연결하기 때문이다. 이는 더 짧고 명확한 초기화 방법이며, 기본 값에서 프로퍼티의 타입을 유추할 수 있게 한다. 기본 값은 또한 기본 이니셜라이저와 이니셜라이저의 상속을 더 쉽게 사용할 수 있게 한다.
```swift
struct Fahrenheit {
    var temperature = 32.0
}
```

### 이니셜라이저 커스터마이징(Customizing Initialization)
입력 매개변수와 옵셔널 프로퍼티 타입을 사용하거나, 또는 초기화 과정에서 상수 프로퍼티를 할당하여 초기화 과정을 커스터마이징할 수 있다.

#### 초기화의 매개 변수
이니셜라이저의 정의 과정에서 초기화 매개 변수를 제공하여 초기화 과정에서 값의 타입과 이름을 정의할 수 있다. 초기화 매개 변수는 함수와 메소드의 매개 변수와 기능 및 문법적 사용이 동일하다.
```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius is 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius is 0.0
```

#### 매개변수의 이름과 인자의 라벨
함수나 메소드의 매개 변수와 마찬가지로 초기화의 매개 변수에는 이니셜라이저 본문 내에서 사용할 매개 변수의 이름과 호출 시 사용할 인자의 라벨을 가질 수 있다.
하지만 이니셜라이저는 함수와 메소드의 경우처럼 괄호 앞의 함수 이름을 식별하지 않는다.(이 말은 함수와는 다르게 모든 이니셜라이저의 이름이 init이라는 뜻인 것 같음.) 따라서 이니셜라이저의 매개 변수 이름과 타입에 따라 어떤 이니셜라이저를 호출할 것인지 식별한다. 이 때문에 Swift는 개발자가 인자 라벨을 붙이지 않은 경우에 모든 매개 변수에 자동적으로 인자 라벨을 제공한다.   

```swift
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}

let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```

인자 라벨 없이 이니셜라이저를 호출할 수 없다는 것에 주의하라. 인자 라벨은 이니셜라이저의 정의에서 항상 사용되어야 하고 이를 생략할 시 컴파일 과정에서 에러를 발생시킨다.
```swift
let veryGreen = Color(0.0, 1.0, 0.0)
// this reports a compile-time error - argument labels are required
```
#### 인자 라벨이 없는 이니셜라이저의 매개 변수
이니셜라이저의 매개 변수에 인자 라벨을 사용하지 않으려면 밑줄(_)을 인자 라벨 대신 사용할 수 있다.
```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}
let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius is 37.0
```
이니셜라이저 호출하는 코드인 ```Celsius(37.0)```은 인자 라벨 없이 그 의도가 명확하므로 이니셜라이저를 ```init(_ celsius: Double)```로 작성하여 명명되지 않은 Double값을 제공하도록 호출할 수 있게 만드는 것이 적절하다.

#### 옵셔널 프로퍼티 타입
만약 커스텀 타입의 저장 프로퍼티에 '초기화 중 값을 할당할 수 없거나' 혹은 '어느 시점에서 값이 없을 경우가 있을 수 있는 경우'로 "값이 없는" 상태가 논리적으로 허용되는 경우, 프로퍼티를 옵셔널 타입으로 선언한다.
옵셔널 프로퍼티는 자동으로 ```nil```값으로 초기화되며, 초기화 중에 의도적으로 "아직 값이 없음"을 나타낸다.
```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// Prints "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```
위의 예제에서 SurveyQuestion에 대한 응답을 요청하기 전까지는 response를 알 수 없으므로 response 프로퍼티의 타입이 String? 으로 선언되었다. SurveyQuestion의 새로운 인스턴스가 초기화되면 nil을 기본 값으로 초기화되고 이는 "아직 값(문자열)이 없음"을 의미한다.

#### 초기화 과정에서 상수 프로퍼티 할당
초기화할 때 상수 프로퍼티를 할당할 수 있다. 상수가 값에 할당된 경우, 더 이상 수정할 수 없다.
> 클래스 인스턴스에서 상수 프로퍼티는 해당 프로퍼티를 정의한 클래스에서만 초기화 과정에서 수정이 가능하다. 자식 클래스에서는 수정할 수 없다,

```swift
import Foundation

class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.text = "Do you like beets?"   // Error! Not Allowed.
beetsQuestion.ask()
// Prints "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```
클래스의 text 프로퍼티가 상수이더라도, 클래스의 이니셜라이저에서 값을 설정할 수 있다.
text 프로퍼티가 변수가 아닌 상수이기 때문에, SurveyQuestion의 인스턴스가 생성되고 나면 그 값을 변경할 수 없음을 볼 수 있다.

### 기본 이니셜라이저(Default Initializers)
Swift는 모든 프로퍼티가 기본 값을 갖고 있는 상황에서 이니셜라이저를 정의하지 않는다면, 기본 이니셜라이저를 제공한다. 기본 이니셜라이저는 모든 프로퍼티 값을 기본 값으로 초기화한다.
```swift
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```
모든 프로퍼티가 기본 값을 갖고 있으면서 상위 클래스가 없는 기본 클래스이기 때문에, ShoppinhListItem은 새로운 인스턴스가 생성될 때 모든 프로퍼티가 기본 값으로 설정된 이니셜라이저의 구현을 자동적으로 얻게 된다.(name 프로퍼티의 경우 옵셔널 문자열 프로퍼티이기 때문에 자동적으로 nil을 기본 값으로 부여받는다.) 

#### 구조체가 제공하는 멤버 이니셜라이저
구조체 타입은 어떠한 커스텀 이니셜라이저도 정의하지 않는다면 자동으로 멤버 이니셜라이저를 갖는다. 기본 이니셜라이저와 달리, 구조체는 저장 프로퍼티가 기본 값이 없더라도 멤버 이니셜라이저를 갖는다.

멤버 이니셜라이저는 구조체의 새로운 인스턴스가 멤버 프로퍼티를 초기화하는 방법을 축약한 것이다. 새로운 인스턴스 내 프로퍼티의 초기 값은 이름을 통해 멤버 이니셜라이저에 전달된다.

아래 예시는 width와 height 두 프로퍼티를 갖는 구조체 Size의 정의이다. 두 프로퍼티는 Double 타입의 기본 값 0.0을 할당 받는다. Size는 자동적으로 멤버 이니셜라이저인 init(width: height: )를 얻게 된다.
```swift
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
``` 
멤버 이니셜라이저를 사용하면, 기본값이 있는 프로퍼티의 값을 생략할 수 있다. 다음 예에서 Size의 프로퍼티 height와 width는 기본 값을 갖고 있다. 속성 중 하나 또는 두 속성 모두를 생략할 수 있으며, 이니셜라이저는 생략한 항목에 대하여 기본 값을 사용한다.
```swift
let zeroByTwo = Size(height: 2.0)
print(zeroByTwo.width, zeroByTwo.height)
// Prints "0.0 2.0"

let zeroByZero = Size()
print(zeroByZero.width, zeroByZero.height)
// Prints "0.0 0.0"
```