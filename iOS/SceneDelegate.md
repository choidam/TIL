## SceneDelegate

Xcode 에서 프로젝트를 생성하면 자동으로 추가되는 `SceneDelegate.swift` 에 대해 알아보도록 하겠습니다.

<div align="center"><img src="./screenshots/scene1.png" width="400"> <img src="./screenshots/scene2.png" width="400"></div>

> ~ iOS 12 버전, iOS 13 버전 이상

- 기존 iOS12 버전 에서는 하나의 AppDelegate 에서 `process Lifecycle`,` UI Lifecycle` 을 담당했습니다.

- iOS 13 버전 이상부터 AppDelegate 의 몇 가지 역할을 SceneDelegate 가 담당하게 되었습니다.

  iOS 13 부터는 여러 UI 인스턴스, Scene session 이 있을 수 있으므로  SceneDelegate  를 따로 빼내 `UILifeCycle` 에 대한 역할을 분리했습니다. (UI state 와 관련된 작업은 SceneDelegate 에서 처리)  

- `AppDelegate.swift` 에서는 Scene Session 을 통해 scene 에 대한 정보를 업데이트 받습니다.

<br/>

<div align="center"><img src="./screenshots/scene3.png" width="600"> </div>


- `scene(_:willConnectTo:options:)`
  - 기존 AppDelegate 의 `application(_:didFinishLaunchingWithOptions:)` 와 기능이 유사합니다. 앱에 scene 이 추가될 때마다 호출됩니다.
- `sceneDidDisconnect(_:)` 
  - scene이 앱에서 disconnect 될 때 호출됩니다. 
- `sceneDidBecomeActive(_:)` 
  - scene이 사용자와 상호작용을 시작할 때 호출됩니다. 
- `sceneWillResignActive(_:)` 
  - scene과 상호작용을 중지할 때 호출됩니다. 
- `sceneWillEnterForeground(_:)` 
  - scene이 foreground에 들어갈 때 호출됩니다. (앱이 시작하거나 background에서 다시 시작될 때)
- `sceneDidEnterBackground(_:)` 
  - scene이 background에 들어갈때 때 호출됩니다.
