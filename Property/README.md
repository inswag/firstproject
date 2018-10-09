## 프로퍼티(Properties)

<br>

## Ref. [스위프트 공식 문서 번역 : Properties - SWIFT : The Swift Programming Language](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)

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

*rangeOfFourItems* 이 상수(let)로 선언되었기 때문에, *firstValue* 프로퍼티를 변경하는 것은 가능하지 않아요, 비록 애가 변수 프로퍼티여도 그래요.

이러한 반응(Behavior)은 구조체가 **값 타입**이기 때문입니다. 값 타입의 인스턴스에 상수가 표시되어 있을 때, 이 상수 인스턴스의 모든 프로퍼티들도 동일합니다(When an instance of a value type is marked as a constant, so are all of its properties.).

클래스에서도 같을까요 ? 아닙니다. 클래스는 **참조 타입**이기 때문이죠. 만약 참조 타입의 인스턴스를 상수에 할당해도, 여러분은 여전히 인스턴스의 변수 프로퍼티들을 변경할 수 있습니다. 

<br>

#### 지연 저장 프로퍼티(Lazy Stored Properties)

<br>

*lazy stored property* 는 초기 값이 처음 사용될 때까지 연산되지 않는 프로퍼티입니다. 여러분은 
지연 저장 프로피티의 선언 전에 lazy 수식어를 작성함으로서 지연 저장 프로퍼티를 나타낼 수 있어요.

>NOTE
>
>여러분은 항상 지연 프로퍼티를 변수(var)로 선언해야만 합니다. 인스턴스의 초기화가 완료될 때 까지 초기 값을 검색 할 수 없기 때문이죠. 상수로 정의된 프로퍼티들은 초기화 완료 전에 값을 항상 가지고 있어야만 합니다. 그러한 연유로 lazy 로 선언될 수가 없는 것이죠.

<br>

지연 프로퍼티들은 프로퍼티의 초기 값이 외부 요소에 의존적일 때 유용합니다. 여기서 외부 요소들이란 인스턴스의 초기화가 완료될 때 까지 값을 알 수 없는 값들이 있죠(outside factors whose values are not known until after an instance’s initialization is complete.). 지연 프로퍼티들은 또한 프로퍼티의 초기 값이 복잡하거나(expensive) 계산상으로 비용이 많이 드는 설정을 필요로 할 때 유용하며, 필요치 않으면 혹은 필요할 때까지는 실행(performed)되어서는 안됩니다. 

다음의 예는 복잡한 클래스의 불필요한 초기화를 피하기 위한 지연 저장 프로퍼티를 사용한 예시입니다. *DataImporter* 와 *DataManager* 라 불리는 두 개의 클래스를 정의하고 있고, 어느 클래스도 모든 코드를 보여주고 있지는 않습니다 : 

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

#### ~ Stored Properties 까지 완료.

