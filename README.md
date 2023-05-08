# SwiftUI_Inject_EnvironmentObject_Into_ViewModel

### EnvironmentObject

```swift
final class UserModel: ObservableObject {
    
    @Published var userName: String = "new user"
    
    func changeUserName(_ userName: String) {
        self.userName = userName
    }
        
}

```

### App

```swift
import SwiftUI

@main
struct InjectEnvironmentObjectApp: App {
    
    @StateObject private var userModel: UserModel = UserModel()
     
    var body: some Scene {
        WindowGroup {
            ContentRoot()
                .environmentObject(self.userModel)
        }
    }
}

```

### ContentRoot 

```swift
import SwiftUI

struct ContentRoot: View {
    
    @EnvironmentObject var userModel: UserModel
    
    var body: some View {
        ContentView(vm: ContentView.ViewModel(userModel: self.userModel))
    }
}
```

### ContentView & ContentViewModel

```swift

import SwiftUI

struct ContentView: View {
    
    @StateObject var vm: ViewModel
    
    init(vm: ViewModel) {
        self._vm = StateObject(wrappedValue: vm)
    }
    
    var body: some View {
        VStack {
            Text(self.vm.userName)
            Button("Change User Name") {
                self.vm.changeUserName(UUID().uuidString)
            }
        }
        .padding()
        
        
    }
}

extension ContentView {
    
    final class ViewModel: ObservableObject {
        
        @Published private var userModel: UserModel
        
        var userName: String {
            self.userModel.userName
        }
        
        init(userModel: UserModel) {
            self.userModel = userModel
        }
        
        func changeUserName(_ name: String) {
            self.userModel.changeUserName(name)
        }
        
        
    }

}
```
