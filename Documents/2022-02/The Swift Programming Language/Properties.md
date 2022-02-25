## Properties (Stored Properties 까지만)

프로퍼티는 클래스, 구조체, 열거형을 값과 연결한다. 저장 프로퍼티는 인스턴스의 일부로써 상수와 변수에 값을 저장하고, 계산 프로퍼티는 값을 저장하기보다는 계산하는 역할을 한다. 계산 프로퍼티는 클래스와 구조체, 열거형 모두에서 사용 가능하고, 저장 프로퍼티는 클래스와 구조체에서만 제공된다.

저장 프로퍼티와 계산 프로퍼티는 일반적으로 특정 타입의 인스턴스와 연관되어 있다. 하지만 프로퍼티는 타입 자체와 연결될 수도 있다. 이러한 프로퍼티를 타입 프로퍼티라고 한다.

프로퍼티 옵저버를 통해 프로퍼티 값의 변경 사항을 모니터링할 수 있으며 프로퍼티 값의 변경에 대한 응답으로 액션을 지정할 수도 있다. 프로퍼티 옵저버는 정의된 저장 프로퍼티 그 자체에, 혹은 부모로부터 상속받은 자식 클래스의 프로퍼티에도 추가할 수 있다.

프로퍼티 래퍼를 사용하여 여러 프로퍼티의 getter와 setter에서 코드를 재사용할 수 있다.

### 저장 프로퍼티
가장 간단한 형태인 저장 프로퍼티는 특정 클래스 또는 구조체의 인스턴스의 일부로써 값을 저장하는 상수 또는 변수이다. 저장 프로퍼티는 가변 저장 속성(var 키워드)이 될 수 있고 상수 저장 속성(let 키워드)가 될 수 있다.

저장 프로퍼티를 정의할 때 기본 값을 제공할 수 있다. 초기화할 때 저장 프로퍼티에 초기 값을 설정하거나 수정할 수도 있다.
```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// the range represents integer values 0, 1, and 2
rangeOfThreeItems.firstValue = 6
// the range now represents integer values 6, 7, and 8
```
구조체 FixedLengthRange의 인스턴스를 생성하는 예시이다. 위의 예시에서 인스턴스 생성 시 두 저장 프로퍼티가 초기화되고 변수 저장 프로퍼티인 firstValue를 6으로 변경하고 있다. length는 상수 저장 프로퍼티이기 때문에 새로운 인스턴스가 만들어질 때 초기화되고 그 이후에는 수정할 수 없다.


#### 상수 구조체 인스턴스의 저장 프로퍼티
구조체 인스턴스를 상수로 선언한 경우, 인스턴스의 프로퍼티를 변경할 수 없다. 구조체 내 저장 프로퍼티가 변수로 정의되어 있더라도 불가능하다.
```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// this range represents integer values 0, 1, 2, and 3
rangeOfFourItems.firstValue = 6
// this will report an error, even though firstValue is a variable property
```
rangeOfFourItems가 상수로 선언되었기 때문에, firstValue 프로퍼티가 변수로 정의되어 있음에도 값을 변경할 수 없다. 이것은 구조체가 값 타입(value types)이기 때문이다. 값 타입의 인스턴스가 상수로 지정되면, 인스턴스의 모든 프로퍼티까지 상수가 된다.

그러나 클래스와 같이 참조 타입(reference types)은 다르다. 참조 타입은 인스턴스를 상수로 선언해도 그 인스턴스의 변수 프로퍼티의 값은 변경이 가능하다.

#### 지연 저장 프로퍼티
지연 저장 프로퍼티는 처음 사용하기 전까지 초기 값이 계산되지 않는 프로퍼티이다. 선언부 앞에 lazy 키워드를 작성하여 지연 저장 프로퍼티임을 나타낸다.
> 지연 저장 프로퍼티는 항상 변수로 선언해야 한다. 인스턴스 초기화가 완료된 이후에 초기 값을 검색하기 때문이다. 상수 프로퍼티는 초기화가 완료되기 전에 반드시 값이 있어야 하므로 lazy로 선언할 수 없다.

지연 저장 프로퍼티는 프로퍼티의 초기값이 인스턴스의 초기화가 완료될 때까지 값을 알 수 없는 외부 요인에 의존적일 때 유용하다. 프로퍼티의 초기 값이 필요하지 않은 경우 또는 필요할 때까지 수행해서는 안 되는 복잡하거나 계산 비용이 많이 드는 설정을 요구하는 경우에 유용하다.
아래 예시는 복잡한 클래스에서 불필요한 초기화를 피하기 위해 지연 저장 프로퍼티를 사용한 예이다.

```swift
class DataImporter {
    /*
    DataImporter is a class to import data from an external file.
    The class is assumed to take a nontrivial amount of time to initialize.
    */
    var filename = "data.txt"
    // the DataImporter class would provide data importing functionality here
}

class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // the DataManager class would provide data management functionality here
}

let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
// the DataImporter instance for the importer property has not yet been created
```

```swift
print(manager.importer.filename)
// the DataImporter instance for the importer property has now been created
// Prints "data.txt"
```
설명:
DataManager클래스에는 빈 문자열 배열로 초기화되는 data라는 프로퍼티가 있다. DataManager의 역할은 문자열 배열에 접근하고 관리하는 것이다.
DataManager의 기능 중 하나는 파일에서 데이터를 가져오는 것이다. 이 기능은 DataImporter클래스를 통해 제공되며 DataImporter의 인스턴스가 초기화될 때, 파일을 열고 그 내용을 메모리로 읽어야 하기 때문에 이 클래스를 초기화하는 데 상당한 시간이 걸린다고 가정한다.

DataManager는 파일에서 데이터를 가져오지 않아도 데이터를 다루는 것이 가능하므로, DataManager의 인스턴스가 생성될 때 DataImporter의 인스턴스까지 생성하지 않도록 한다는 것이다. 그 대신 DataImporter가 처음 사용될 때 그제서야 DataImporter 인스턴스를 만드는 것이 낫다.

lazy 키워드로 선언되었기 때문에 importer 프로퍼티의 DataImporter 인스턴스는 클래스의 인스턴스가 생성될 때 만들어지지 않고, 프로퍼티에 처음 액세스할 때 생성된다.

> 만약 초기화되지 않은 lazy 프로퍼티에 여러 스레드가 동시에 접근할 경우, 프로퍼티가 하나만 생성되도록 보장할 수 없다.

#### 저장 프로퍼티와 인스턴스 변수
Objective-C를 다뤄본 경험이 있다면 클래스 인스턴스의 일부분으로써 값을 저장하거나 이를 참조하는 두 가지 방법을 제공한다는 것을 알 수 있을 것이다. 프로퍼티 외에도 인스턴스 변수를 프로퍼티에 저장된 값의 저장소로 사용할 수 있다.

Swift는 이러한 개념을 단일 프로퍼티 선언으로 통일한다. Swift의 프로퍼티에는 해당 인스턴스 변수가 없으며 프로퍼티의 저장 공간에 직접적으로 접근할 수 없다. 이 방식은 서로 다른 문맥에서 값에 접근하는 방식에 따른 혼란을 피하고, 프로퍼티 선언을 하나의 정확한 구문으로 단순화한다. 이름, 타입, 메모리 관리 특성을 포함하여 프로퍼티에 대한 모든 정보는 타입 정의의 일부분으로 하나의 위치에 정의된다.