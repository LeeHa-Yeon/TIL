# 캡스톤 - 소켓 IO

# 1. 소켓통신

- 네트워크를 통해 서버-클라이언트 양쪽에 링크를 생성하고 그 링크를 통해 데이터를 주고 받는 것
- 이때 서버에 임의의 포트번호를 설정한 상태에서 클라이언트에서 해당 포트로 접속 시도하면 서버와 클라이언트가 그 포트번호로 서로 연결되는 상태가 됨

    ⇒ 여기서 데이터를 주고 받을 수 있음

## 사용하는 이유

- 서버와 클라이언트가 계속 연결을 유지하는 양방향 통신
- 실시간으로 데이터를 주고받는 상황이 필요한 경우에 사용
- 간결하고 가볍고 빨라서 빠른 데이터 전달한다는 특징 지님

## SocketIO

- 소켓통신 사용법 : 서버와 클라이언트가 포트번호로 서로 연결된 상태로 데이터 주고받음
- 서버 : Node.js
- 클라이언트 : Swift

# 2. 소켓 구현

1. [https://github.com/appcoda/SocketIOChat/blob/master/srv-SocketChat.zip](https://github.com/appcoda/SocketIOChat/blob/master/srv-SocketChat.zip) 에서 View raw 눌러서 다운받기 ( 이때 아는 경로에 다운받기 )
2. [https://node.js.org/en/](https://node.js.org/en/) 의 대부분 사용자에게 권장으로 다운받기
3. 터미널 - index.js가 있는 디렉토리로 들어간 다음 "node index.js" 치기
4. 아래와 같이 뜨면 성공

    ![%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%86%AB%20-%20%E1%84%89%E1%85%A9%E1%84%8F%E1%85%A6%E1%86%BA%20IO%20d291f27adcac40e1b76597d50f66926a/_2021-05-10__10.07.22.png](%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B3%E1%84%90%E1%85%A9%E1%86%AB%20-%20%E1%84%89%E1%85%A9%E1%84%8F%E1%85%A6%E1%86%BA%20IO%20d291f27adcac40e1b76597d50f66926a/_2021-05-10__10.07.22.png)

5. 터미널에서 ifconfig치면 en0에 나오는 IP주소 기억하기  ⇒ 예 ) inet 192.168.0.20
6. xcode 프로젝트에 클래스 하나 만들기 ( 싱글톤 형태 )
    1. pod 'Socket.IO-Client-Swift', '~> 15.2.0' 넣기
    2. pod install

```swift
import Foundation
import SocketIO
class SocketIOManager:NSObject{
    static let shared = SocketIOManager()
    override init() {
        super.init()
        socket = self.manager.socket(forNamespace: "/")
    }
    var manager = SocketManager(socketURL: URL(string: "http://192.168.0.20:3000")!, config: [.log(true), .compress])
    var socket : SocketIOClient!
    func establishConnection(){
        socket.connect()
    }
    func closeConnection(){
        socket.disconnect()
   
```

⇒ SocketManager의 ip 부분만 바꾸면 됨

  7. 앱 델리게이트

```swift
import UIKit

@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        ...
    }

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        ...
    }
    
    func applicationDidBecomeActive(_ application: UIApplication) {
            SocketIOManager.shared.establishConnection()
    }
    func applicationDidEnterBackground(_ application: UIApplication) {
            SocketIOManager.shared.closeConnection()
    }

}
```

8. 마지막으로 서버를 킨 상태에서 앱을 실행하면 터미널에 a user connect가 뜸

- 만약 안되는 경우엔 처음 로드하는 부분 class의 viewDidLoad에 추가
    - SocketIOManager.shared.establishConnection() 추가

    ```swift
    override func viewDidLoad() {
            super.viewDidLoad()
            SocketIOManager.shared.establishConnection()
    }
    ```

# 3. [Socket.IO](http://socket.IO) 주요 메서드

1. socket.connet 
    - 설정한 주소와 포트로 소켓 연결 시도
2. socket.disconnet
    - 소켓 연결 종료
3. socket.emit("이벤트이름",["데이터1","데이터2"]) 
    - 예시 ) socket.emit("ab",["data1","data2"])
    - 해석 ) "ab"란 이벤트이름으로 "data1","data2" 송신
4. socket.on("이벤트이름")
    - 예시 ) socket.on("abc")
    - 해석 ) 이름이 "abc"로 emit된 이벤트를 수신
