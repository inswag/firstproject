## 프로퍼티(Properties)

<br>

### Ref. [스위프트 공식 문서 번역 : Properties - SWIFT : The Swift Programming Language](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)

### 코드는 번역 X / Ref 는 번역 고민..
### 영어 단어 자체로 표기하는 것이 자연스러운 어휘는 '한(영)' 처럼 둘 다 표기하도록 했습니다.
<br>

프로퍼티는 특정 클래스, 구조체, 열거형의 값과 관련이 있다. 저장 프로퍼티들은 어느 인스턴스의 일부로서 상수 그리고 변수의 값을 저장하고 있고, 반면 연산 프로퍼티들은 값을 저장하기 보다는 계산을 한다. **연산 프로퍼티**는 **클래스, 구조체, 열거형**에서 제공된다. **저장 프로퍼티**는 열거형에서는 제공되지 않고, 오직 **클래스와 구조체**에서 제공된다.

저장 및 연산 프로피티들은 일반적으로 특정 타입의 인스턴스와 관련이 있다. 하지만, 프로퍼티들은 또한 타입 그 자체와도 연관이 있는데, 그러한 프로퍼티를 **타입 프로퍼티**라 부른다. 

이외에도, 여러분은 프로퍼티의 값의 변화를 감시하기 위해 **프로퍼티 옵저버**를 정의할 수 있습니다. 이러한 변화를 여러분은 사용자가 정의한 액션을 통해서 응답할 수 있습니다. 프로퍼티 옵저버는 **여러분이 정의한 저장 프로퍼티**에 추가 될 수 있으며, **서브 클래스가 슈퍼 클래스로부터 상속받은 프로퍼티**에도 추가 할 수 있습니다.

<br>

### 저장 프로퍼티(Stored Properties)

<br>

가장 단순한 형태에서 저장 프로퍼티는 특정 클래스나 구조체의 인스턴스의 일부로서 상수나 변수를 의미합니다(우리가 가장 일반적으로 사용하는 형태). 저장 프로퍼티들은 변수로서 저장 프로퍼티(var) 혹은 상수로서 저장 프로퍼티(let) 둘 중 하나 일 수 있습니다.

저장 프로퍼티를 정의하는 일부로서, 이 프로퍼티에 default 값을 할당할 수 있습니다. 이것은 *Default Property Values* 라 합니다. 또한 초기화 중에 저장 프로퍼티의 초기 값을 설정하고 수정할 수 있습니다.
이것을 *Assigning Constant Properties During Initialization* 이라 합니다. 

다음 예시는 *FixedLengthRange* 라 불리는 구조체를 정의하고 있습니다. 만들어진 후에는 길이의 범위가 변할 수 없는 정수의 범위를 설명하고 있죠. 

<br>

```Swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// the range represents integer values 0, 1, and 2
rangeOfThreeItems.firstValue = 6
// the range now represents integer values 6, 7, and 8
```

<br>

이 구조체의 인스턴스는 변수 저장 프로퍼티(firstValue)와 상수 저장 프로퍼티(length)를 가지고 있습니다. 위의 에시에서 길이는 새로운 범위가 생성될 때 초기화되고 그 후에는 변화되지 않죠, 상수 프로퍼티이기 때문입니다. 

<br>

#### Ref. Initialization - Default Property Values

* You can set the initial value of a stored property from within an initializer, as shown above. Alternatively, specify a default property value as part of the property’s declaration. You specify a default property value by assigning an initial value to the property when it is defined.

>NOTE
>
>If a property always takes the same initial value, provide a default value rather than setting a value within an initializer. The end result is the same, but the default value ties the property’s initialization more closely to its declaration. It makes for shorter, clearer initializers and enables you to infer the type of the property from its default value. The default value also makes it easier for you to take advantage of default initializers and initializer inheritance, as described later in this chapter.

You can write the Fahrenheit structure from above in a simpler form by providing a default value for its temperature property at the point that the property is declared:

```Swift
struct Fahrenheit {
    var temperature = 32.0
}
```
<br>

#### Ref. Initialization - Assigning Constant Properties During Initialization

You can assign a value to a constant property at any point during initialization, as long as it is set to a definite value by the time initialization finishes. Once a constant property is assigned a value, it can’t be further modified.

>NOTE
>
>For class instances, a constant property can be modified during initialization only by the class that introduces it. It cannot be modified by a subclass.

You can revise the SurveyQuestion example from above to use a constant property rather than a variable property for the text property of the question, to indicate that the question does not change once an instance of *SurveyQuestion* is created. Even though the *text* property is now a constant, it can still be set within the class’s initializer:

```swift
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
beetsQuestion.ask()
// Prints "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```

<br>

#### 상수로 정의한 구조체 인스턴스의 저장 프로퍼티(Stored Properties of Constant Structure Instances)

<br>

여러분이 구조체의 인스턴스를 생성하고 상수에 인스턴스를 할당한다면, 여러분은 인스턴스의 프로퍼티들을 수정할 수 없어요, 비록 프로퍼티들이 변수로 선언되어 있어도 말이죠 :

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// this range represents integer values 0, 1, 2, and 3
rangeOfFourItems.firstValue = 6
// this will report an error, even though firstValue is a variable property
```

*rangeOfFourItems* 이 상수(let)로 선언되었기 때문에, *firstValue* 프로퍼티를 변경하는 것은 가능하지 않아요, 비록 애가 가지고 있는 프로퍼티들이 변수 프로퍼티여도 그래요.

이러한 반응(Behavior)은 구조체가 **값 타입**이라는 것에서 기인합니다. 값 타입의 인스턴스에 상수가 할당되어 있을 때, 이 상수로 정의된 인스턴스의 모든 프로퍼티들도 동일합니다(When an instance of a value type is marked as a constant, so are all of its properties.).

클래스에서도 같을까요 ? 아닙니다. 클래스는 **참조 타입**이기 때문이죠. 만약 참조 타입의 인스턴스를 상수에 할당해도, 여러분은 여전히 인스턴스의 변수 프로퍼티들을 변경할 수 있습니다. 

<br>

#### 지연 저장 프로퍼티(Lazy Stored Properties)

<br>

*lazy stored property* 는 초기 값이 처음 사용될 때까지 연산되지 않는 프로퍼티입니다. 여러분은 
지연 저장 프로피티의 선언은, lazy 수식어를 작성함(Ex. lazy var) 으로서 지연 저장 프로퍼티를 나타낼 수 있어요.

<br>

>NOTE
>
>여러분은 항상 지연 프로퍼티를 변수(var)로 선언해야만 합니다. **인스턴스의 초기화가 완료될 때 까지 초기 값을 검색 할 수 없기 때문**이죠. **상수로 정의된 프로퍼티들은 초기화 완료 전에 값을 항상 가지고** 있어야만 합니다. 그러한 연유로 lazy 로 선언될 수가 없는 것이죠.

<br>

지연 프로퍼티들은 프로퍼티의 초기 값이 외부 요소에 의존적일 때 유용합니다. 여기서 외부 요소들이란 인스턴스의 초기화가 완료될 때 까지 값을 알 수 없는 값들을 말합니다(outside factors whose values are not known until after an instance’s initialization is complete.). 지연 프로퍼티들은 또한 프로퍼티의 초기 값이 복잡하거나(expensive) 계산상으로 비용이 많이 드는 설정(expensive setup)을 필요로 할 때 유용하며, 필요치 않으면 혹은 필요할 때까지는 실행(performed)되어서는 안됩니다. 

다음의 예는 복잡한 클래스의 불필요한 초기화를 피하기 위한 지연 저장 프로퍼티를 사용한 예시입니다. *DataImporter* 와 *DataManager* 라 불리는 두 개의 클래스를 정의하고 있고, 아래의 두 클래스는 모든 코드를 보여주고 있지는 않다는 걸 참고하세요 : 

<br>

```Swift
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

<br>

*DataManager* 클래스는 data 라는 저장 프로퍼티를 가지고 있고, String 타입의 값을 가지는 빈 배열과 함꼐 초기화되었습니다. 비록 기능의 나머지를 보진 못하지만, 이 클래스의 목적은 String 타입의 데이터 배열을 관리하고 접근을 제공하기 위함입니다.

이 클래스의 기능 중 일부는 파일로부터 데이터를 불러오는 기능입니다. 이 기능은 *DataImporter* 클래스에 의해 제공되며, 초기화 하기 위하여 적지 않은 양의 시간이 추정됩니다. 이는 *DataImporter* 인스턴스가 파일을 열어야만 하고 *DataImporter* 인스턴스가 초기화 될 때 그것의 내용을 메모리로 읽어들어야 하기 때문일지도 모릅니다. 

*DataManager* 인스턴스는 자신의 데이터를 파일로부터 데이터를 불러오지 않고서 관리하는 것이 가능합니다. 그래서 *DataManager* 자신이 생성될 때, 새로운 *DataImporter* 인스턴스를 생성할 필요가 없습니다. 대신에, *DataImporter* 인스턴스를 처음 사용할 때 생성하는 것이 더 바람직합니다. 

왜냐하면 lazy 수식어로 표시되었기 때문에, *importer* 프로퍼티에 대한 *DataImporter* 인스턴스는 오직 importer 프로퍼티가 처음 접근 되었을 때 생성됩니다, such as when its filename property is queried:

<br>

```Swift
print(manager.importer.filename)
// the DataImporter instance for the importer property has now been created
// Prints "data.txt"
```

<br>

>NOTE
>
>만약 lazy 수식어로 표기된 프로퍼티가 많은 프로세스에 의해 동시에 접근된다면 그리고 프로퍼티가 아직 초기화되지 않았다면, 프로퍼티가 한 번만 초기화될 것이라는 보장은 없습니다.

<br>

#### 저장 프로퍼티와 변수로 선언된 인스턴스(Stored Properties and Instance Variables)

<br>

(Objective-C 관련 내용) If you have experience with Objective-C, you may know that it provides two ways to store values and references as part of a class instance. In addition to properties, you can use instance variables as a backing store for the values stored in a property.

Swift unifies these concepts into a single property declaration. A Swift property does not have a corresponding instance variable, and the backing store for a property is not accessed directly. This approach avoids confusion about how the value is accessed in different contexts and simplifies the property’s declaration into a single, definitive statement. All information about the property—including its name, type, and memory management characteristics—is defined in a single location as part of the type’s definition.

<br>

### 연산 프로퍼티(Computed Properties)

<br>

앞서 살펴본 저장 프로퍼티 이외에도, 클래스, 구조체, 열거형은 **연산 프로퍼티**를 정의할 수 있어요. 얘는 실제로 값을 저장하고 있지 않아요. 대신에, *getter* 와 옵셔널 타입의 *setter* 를 검색(retrieve)을 위해 그리고 다른 프로퍼티와 값을 간접적으로 설정하기 위해 제공합니다. 

<br>

```Swift
struct Point {
    var x = 0.0, y = 0.0
}
struct Size {
    var width = 0.0, height = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
                  size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// Prints "square.origin is now at (10.0, 10.0)"
```

<br>

이 예시는 3개의 구조체를 정의하고 있어요, 기하학적 모양으로 작동(for working with geometric shapes)하는 구조체 입니다 :

* Point 는 점의 x 및 y 좌표를 캡슐화합니다.
* Size 는 width 와 height 값을 캡슐화합니다.
* Rect 는 원점(origin point)과 사이즈를 사용해 사각형을 정의합니다.

*Rect* 구조체는 *center* 라 불리는 연산 프로퍼티를 또한 제공합니다. Rect 의 현재 센터 포지션은 항상 origin 과 size 로부터 결정되며, 따라서 여러분은 센터 좌표를 명시적인 *Point* 값으로 저장할 필요가 없습니다. 대신, *Rect* 는 사용자 정의 getter 와 setter 를 *center* 라 불리는 변수로 된(var) 연산 프로퍼티에 정의하였습니다. 마치 실제로 저장된 프로퍼티인 것처럼 사각형의 센터를 사용하여 작업할 수 있게 해줍니다.  

위의 코드는 *square* 라는 이름의 변수로 선언된 *Rect* 를 생성합니다. *square* 변수는 원점 (0, 0) 에서 초기화되며, 너비와 높이는 10으로 초기화 됩니다. 이 사각형은 아래 다이어그램의 파란 사각형으로 표현되고 있습니다.

*square* 변수의 *center* 프로퍼티는 점 문법(dot syntax)를 통해(square.center) 접근할 수 있습니다. 이는 *center* 의 getter 가 호출되도록 하며, 현재 프로퍼티 값을 검색하기 위함입니다. 존재하는 값을 리턴하기보다는, getter 는 실제로 연산하고 정사각형의 센터를 나타내기 위한 새로운 *Point* 를 리턴합니다. 위에서 볼 수 있었던 것처럼, getter 는 정확히 센터 좌표 (5, 5) 를 리턴하고 있습니다. 

*center* 프로퍼티는 따라서 새 값 (15, 15) 로 설정되며, 정사각형을 오른쪽 상단으로 움직이게 하며, 아래의 다이어그램의 오렌지 색 정사각형에 의해 새로운 위치가 보여지고 있습니다. *center* 프로퍼티를 설정하는 것은 *center* 의 setter 를 호출하며, 이는 저장 프로퍼티 *origin* 의 x와 y 값 수정합니다. 그리고 사각형은 새로운 위치로 움직이게 되죠. 

![Diagram](https://i.imgur.com/RmkcYsP.png)

#### 속기 Setter 선언(Shorthand Setter Declaration)

만약 연산 프로퍼티의 Setter 가 설정할 새로운 값에 대한 이름을 정의하지 않을 경우, *newValue* 라는 디폴트 이름이 사용된다. *Rect* 구조체의 대안 버전이 여기 있는데, 속기 표기법(shorthand notation)의 이점을 가져봅시다 : 

```Swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
```

#### 읽기 전용 연산 프로퍼티(Read-Only Computed Properties)

getter 를 갖지만 setter 가 없는 연산 프로퍼티는 *Read-Only Computed Properties* 로 알려져 있습니다. 읽기 전용 프로퍼티는 항상 값을 리턴하며, 점 문법을 통해 접근할 수 있지만, 그러나 다른 값이 설정될 수는 없습니다.

>NOTE
>
>여러분은 읽기 전용 프로퍼티를 포함한 모든 연산 프로퍼티를 변수로(var) 선언한 프로퍼티를 사용해야만 합니다. 왜냐하면 얘내 값은 고정되어있지 않기 때문이죠. let 이라는 것은 오직 상수로 선언된 프로퍼티에만 사용될 수 있고, 얘내들은 값이 한번 인스턴스의 초기화의 일부분으로 설정된 이상(once they are set as part of instance initialization) 변화할 수 없다는 것을 나타냅니다.

읽기 전용 프로퍼티의 선언을 get 키워드와 {중괄호} 를 제거함으로서 단순화 할 수 있습니다 :

```Swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// Prints "the volume of fourByFiveByTwo is 40.0"
```

위의 예시는 *Cuboid* 라는 새로운 구조체를 정의하고 있어요. 3차원의 사각형 상자를 *width*, *height*, *depth* 프로퍼티로 나타내고 있죠. 이 구조체는 또한 *volume* 이라 불리는 읽기 전용 연산 프로퍼티를 가지고 있는데, 얘는 계산하고 현재 cuboid의 볼륨을 리턴하죠. *volume* 이 setter로 설정되기에는 뭔가 적절하지 않아요, 그것은 특정 *volume* 값에 사용되어야 하는 width, height, depth 의 어떤 값을 사용해야 하는지 다소 모호하기 때문이죠. 그렇기는 하지만, *Cuboid* 가 읽기 전용 프로퍼티를 제공하는 것은 유용하죠. 외부 유저들에게 얘의 현재 계산된 부피(volume)를 발견하게끔 해주니까요.


