# Presenting a bottom sheet in UIKit using `UISheetPresentationController`

> Available in iOS 15+

## Files 

* `BottomSheetViewController`: the main view controller responsible for presenting the sheet
* `BottomSheetView`: Presented bottom sheet written in SwiftUI 
* `BottomSheetDetailController`: hosts the SwiftUI View

![Screen Shot 2023-01-27 at 4 23 21 PM](https://user-images.githubusercontent.com/1819208/215203281-4c077ba2-e867-41f2-9568-279d335e6387.png)

![Screen Shot 2023-01-27 at 4 20 43 PM](https://user-images.githubusercontent.com/1819208/215203033-d8836e42-22a3-4b34-9eec-f6f27d5a23b0.png)

![Screen Shot 2023-01-27 at 4 20 57 PM](https://user-images.githubusercontent.com/1819208/215203053-98c66a43-25d4-40af-9eea-f4684f87bb09.png)

![Simulator Screen Shot - iPad Pro (11-inch) (4th generation) - 2023-01-27 at 16 16 14](https://user-images.githubusercontent.com/1819208/215202317-fdefde12-45de-42d5-b83f-cba4b2b3ef64.png)

![Simulator Screen Shot - iPad Pro (11-inch) (4th generation) - 2023-01-27 at 16 17 43](https://user-images.githubusercontent.com/1819208/215202528-e41067b9-eaaf-474a-9624-831bb3b9c269.png)


try? it out 

## `BottomSheetViewController.swift`

```swift
import UIKit

final class BottomSheetViewController: UIViewController {
    private lazy var presentationButton: UIButton = {
        let button = UIButton()
        button.addTarget(self, action: #selector(presentSheet), for: .touchUpInside)
        button.setTitle("Preset Bottom Sheet using UIKit", for: .normal)
        button.setTitleColor(.white, for: .normal)
        return button
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .orange
        view.addSubview(presentationButton)
    }

    override func viewWillLayoutSubviews() {
        super.viewWillLayoutSubviews()

        presentationButton.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            presentationButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            presentationButton.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
    }

    @objc func presentSheet() {
        // 1
        // Create an instance of the view controller that will be presented
        // in this case it host a SwiftUI View
        let detailVC = BottomSheetDetailController(rootView: BottomSheetView())

        // 2
        // Support `popover` for iPad
        detailVC.modalPresentationStyle = .popover

        // 3
        // Get the `popoverPresentationController` from the view controller
        guard let popover = detailVC.popoverPresentationController else {
            return
        }

        // 4
        // Configure the `popover` instance
        popover.permittedArrowDirections = [.up]
        popover.sourceView = presentationButton

        // 5
        // `adaptiveSheetPresentationController` is available as of iOS 15
        let sheet = popover.adaptiveSheetPresentationController

        // 6
        // Configure the `UISheetPresentationController` here
        sheet.prefersEdgeAttachedInCompactHeight = true
        sheet.widthFollowsPreferredContentSizeWhenEdgeAttached = true
        sheet.preferredCornerRadius = 24.0
        sheet.prefersGrabberVisible = true

        // 7
        // [UISheetPresentationController.Detents], e.g .medium(), .large()
        // if `.detents` is not set a full sheet will be presented
        sheet.detents = [.medium(), .large()]

        // 8
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
    @Environment(\.dismiss) var dismiss

    private let names = [
        "Combine", "SwiftUI", "Testing", "UIKit",
        "Swift Package", "Swift Concurrency", "Accessibility", "Xcode"
    ]

    var body: some View {
        VStack {
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
            Button(action: dismissView) {
                Text("Dismiss")
                    .font(.title3)
                    .bold()
                    .foregroundColor(.white)
                    .frame(maxWidth: .infinity)
            }
            .padding(.top, 20)
            .background(.blue)
        }
    }

    private func dismissView() { dismiss() }
}

struct BottomSheetView_Previews: PreviewProvider {
    static var previews: some View {
        BottomSheetView()
    }
}
```

***

## Resources 

* [Apple docs: UISheetPresentationController](https://developer.apple.com/documentation/uikit/uisheetpresentationcontroller)
* [Customize and Resize Sheets in UIKit - WWDC21](https://developer.apple.com/wwdc21/10063)
