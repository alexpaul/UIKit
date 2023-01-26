# Presenting a bottom sheet in UIKit using `UISheetPresentationController`

> Available in iOS 15+

![Screen Shot 2023-01-26 at 10 44 05 AM](https://user-images.githubusercontent.com/1819208/214881851-dab435fa-90fb-4499-ab83-70ba6abc4c7c.png)

![Screen Shot 2023-01-26 at 10 44 14 AM](https://user-images.githubusercontent.com/1819208/214881876-74cfa2cf-d113-4393-ac6f-dbda3343e90e.png)

![Screen Shot 2023-01-26 at 10 44 32 AM](https://user-images.githubusercontent.com/1819208/214881900-b21cd9a6-acb3-48ce-89ea-a4f424571cd5.png)

try? it out 

## `BottomSheetViewController.swift`

```swift
import UIKit

final class BottomSheetViewController: UIViewController {

    private lazy var presentationButton: UIButton = {
        let button = UIButton(frame: CGRect(x: 0, y: 0, width: 300, height: 44))
        button.addTarget(self, action: #selector(presentSheet), for: .touchUpInside)
        button.setTitle("Preset Bottom Sheet using UIKit", for: .normal)
        button.setTitleColor(.white, for: .normal)
        button.center = view.center
        return button
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .orange

        view.addSubview(presentationButton)
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
    }

    @objc func presentSheet() {
        let detailVC = BottomSheetDetailController(rootView: BottomSheetView())

        // 1
        // `sheetPresentationController` is available as of iOS 15
        guard let sheet = detailVC.sheetPresentationController else {
            return
        }

        // 2
        // Customize the `UISheetPresentationController` here

        // 3
        // [UISheetPresentationController.Detents], e.g .medium(), .large()
        // if `.detents` is not set a full sheet will be presented
        sheet.detents = [.medium(), .large()]

        // 4
        // Present the sheet
        present(detailVC, animated: true)
    }
}
```

***


## `BottomSheetDetailController.swift`

```swift
import SwiftUI

final class BottomSheetDetailController: UIHostingController<BottomSheetView> {}
```

***

## `BottomSheetView.swift`

```swift
import SwiftUI

struct BottomSheetView: View {
    private let names = [
        "Combine", "SwiftUI", "Testing", "UIKit",
        "Swift Package", "Swift Concurrency", "Accessibility", "Xcode"
    ]

    var body: some View {
        ScrollView(showsIndicators: false) {
            VStack(alignment: .center, spacing: 20) {
                Text("SwiftUI View hosted by UIKit **`UISheetPresentationController`**")
                    .font(.title3)
                    .multilineTextAlignment(.center)
                Image("swiftui-logo")
                    .resizable()
                    .aspectRatio(contentMode: .fit)
                ForEach(names, id: \.self) { name in
                    Text(name)
                }
            }
            .padding(.top, 40)
        }
    }
}

struct BottomSheetView_Previews: PreviewProvider {
    static var previews: some View {
        BottomSheetView()
    }
}
```
