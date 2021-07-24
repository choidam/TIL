# Unit Test

### 유닛테스트란?

> 유닛 테스트는 컴퓨터 프로그래밍에서 소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차다. 즉, 모든 함수와 메소드에 대한 테스트 케이스(Test case)를 작성하는 절차를 말한다. (위키백과)

unit test 는 작성한 프로그램이 의도한 대로 동작하는지 검증하는 가장 작은 단위의 테스트 입니다. 이를 통해서 각 모듈(클래스, 메소드)들이 잘 동작하는지 확인할 수 있습니다.

<br/>

### 왜 필요한가?

TDD 와 유닛테스트에 대한 내용을 처음 접했을 땐 '굳이?' 라는 생각이 가장 먼저 들었습니다.

프로그램을 개발할 때 분명히 빌드를 하며 제대로 동작하는 것을 확인 하고 커밋을 할텐데..?! 오히려 테스트 코드를 작성하는 시간이 더 오래 걸릴 것 같아 비효율적이라고 생각했습니다.

하지만 이미 많은 기업에서 유닛테스트를 적용하고 있습니다. 사용하는 이유가 있겠져 ㅋ

유닛테스트를 작성하는 이유는

1. 각각의 모듈을 부분적으로 확인할 수 있어 어떤 모듈에서 문제가 발생하는지 빠른 확인이 가능
2. 전체 프로그램을 빌드하는 대신 유닛 단위로 빌드해 확인하므로 시간 절약

이라고 볼 수 있을 것 같습니다.

특히 2같은 경우 빌드될 때까지 기다릴 때 엉겁의 시간처럼 느껴지므로 .. ㅎㅎ 잘 적용한다면 정말 좋은 테스팅 기법이구나! 라는 생각이 듭니당

<br/>

### 어떻게 작성하는가?

각 테스트 케이스는 서로 분리되어야 하므로(isolated), 가짜 객체 mock object(Stub) 를 만들어 테스트 하는 것이 좋다고 합니다. 즉 커플링이 걸려있는 곳은 이를 해제해 각각 분리하는 것이 좋습니다.

말로만 들었던 의존성 주입이 핵심인 것 같습니다. 테스트하는 mock 객체가 추상화 된 프로토콜을 채택하면 테스트하기 용이해 집니다.

<br/>

### 테스트 코드 작성하기

참고한 [튜토리얼 링크](https://www.raywenderlich.com/21020457-ios-unit-testing-and-ui-testing-tutorial)입니다.

프로그램을 처음 만들 때 include Unit Test 를 체크 하거나, 기존 프로젝트에서 추가하고 싶은 경우에는  Command6 > +버튼 > New Unit Test Target 을 누르면 Test 를 작성할 수 있는 파일이 만들어집니다.

```swift
@testable import 프로젝트이름
```

테스트 코드를 작성할 때 가장 먼저 이 구문을 작성해야 합니다. 기존 프로젝트의 코드에 접근할 수 있게 해주는 코드입니다.

```swift
import XCTest
@testable import 프로젝트이름

class NewTests: XCTestCase {

    override func setUpWithError() throws {
        // Put setup code here. This method is called before the invocation of each test method in the class.
    }

    override func tearDownWithError() throws {
        // Put teardown code here. This method is called after the invocation of each test method in the class.
    }
}
```

unit test 추가 시 자동으로 생성되는 코드입니다.

- `setUpWithError` : 테스트 메소드가 실행되기 전 모든 상태를 reset 합니다. (초기화 코드)
- `tearDownWithError` : 테스트 동작이 끝난 후 모든 상태를 clean up 합니다. (해체 코드)

테스트 메소드를 작성해 보겠습니다. 테스트 메소드는 반드시 **test** 키워드로 시작해야 합니다.

```swift
	func testScoreIsComputedWhenGuessIsHigherThanTarget(){
	    // given
	    let guess = sut.targetValue + 5
	    
	    // when
	    sut.check(guess: guess)
	    
	    // then
	    XCTAssertEqual(sut.scoreRound, 95, "Score computed from guess is wrong")
	  }
```

테스트 포맷은 given - when - then 구조로 작성하는 것이 좋습니다.

위 테스트 코드는 guessValue 가 targetValue보다 클 경우(given) check 메소드를 테스트(when)합니다.

- given : 필요한 value 들을 셋팅
- when : 테스트 코드 실행
- then : 결과 확인(출력)

<br/>

### 비동기 네트워크 테스트 코드 작성하기

앞선 방법으로 네트워크 테스트 코드를 작성할 경우 무조건 실패하는 코드가 작성됩니다. URLSession 메소드는 비동기로 진행되기 때문에 네트워크 응답이 오기 전 XCTestExpectaion~ 코드가 먼저 실행되기 때문입니다.

아래는 이를 해결한 비동기로 진행되는 네트워크 테스트 코드입니다.

```swift
class BullsEyeSlowTests: XCTestCase {
  var sut: URLSession!
  let networkMonitor = NetworkMonitor.shared

  // Asynchronous test: success fast, failure slow
  func testValidApiCallGetsHTTPStatusCode200() throws {
    try XCTSkipUnless(
      networkMonitor.isReachable,
      "Network connectivity needed for this test.")

    // given
    let urlString = "<http://www.randomnumberapi.com/api/v1.0/random?min=0&max=100&count=1>"
    let url = URL(string: urlString)!
    let promise = expectation(description: "Status code: 200")

    // when
    let dataTask = sut.dataTask(with: url) { _, response, error in
      // then
      if let error = error {
        XCTFail("Error: \\(error.localizedDescription)")
        return
      } else if let statusCode = (response as? HTTPURLResponse)?.statusCode {
        if statusCode == 200 {
          promise.fulfill() // 응답 조건이 충족된 경우 
        } else {
          XCTFail("Status code: \\(statusCode)")
        }
      }
    }
    dataTask.resume()
  
    wait(for: [promise], timeout: 5)
  }
}
```

핵심은 이 곳입니다.

```swift
wait(for: [promise], timeout: 5) // 5초 기다림
```

위 코드는 200 status code 응답을 받거나 5초간 시간이 지났을 경우 테스트를 진행합니다.

비동기 문제는 해결했으나 좋은 테스트 코드라는 생각은 들지 않습니다. 테스트가 실패할 경우(잘못된 URL 을 입력했을 경우 등) timeout 시간이 만료될 때까지 기다려야 하기 때문입니다.

```swift
func testApiCallCompletes() throws {
  // given
  let urlString = "<http://www.randomnumberapi.com/test>"
  let url = URL(string: urlString)!
  let promise = expectation(description: "Completion handler invoked")
  var statusCode: Int?
  var responseError: Error?

  // when
  let dataTask = sut.dataTask(with: url) { _, response, error in
    statusCode = (response as? HTTPURLResponse)?.statusCode
    responseError = error
    promise.fulfill()
  }
  dataTask.resume()
  wait(for: [promise], timeout: 5)

  // then
  XCTAssertNil(responseError)
  XCTAssertEqual(statusCode, 200)
}
```

위 테스트 코드는 fail 하는 데에 timeout 시간만큼 기다릴 필요가 없습니다.

timeout 시간이 expired 할 때까지 기다리는게 아니라 `XCTAssertEqual(statusCode, 200)` 에서 test fail 이 이루어지기 때문입니다.
