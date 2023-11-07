# Updating a SwiftUI View 

![Screenshot 2023-11-07 at 1 05 08 PM](https://github.com/alexpaul/UIKit/assets/1819208/47fc67f9-a965-4937-af20-c8c574ea27b4)

try? it out

```swift
import UIKit
import SwiftUI

// MARK: - SwiftUI Views
struct SwiftUIView: View {
    var content: (() -> String)

    var body: some View {
        Text(content())
    }
}

// MARK: - UIKit Views

final class UIKitView: UIView {
    private let hostingVC = UIHostingController(
        rootView: SwiftUIView(
            content: { "Initial SwiftUI Content" }
        )
    )

    private lazy var swiftUIView: UIView = {
        hostingVC.view
    }()

    override init(frame: CGRect) {
        super.init(frame: frame)
        backgroundColor = .systemBackground
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func layoutSubviews() {
        super.layoutSubviews()
        addSubview(swiftUIView)
        swiftUIView.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            swiftUIView.centerXAnchor.constraint(equalTo: safeAreaLayoutGuide.centerXAnchor),
            swiftUIView.centerYAnchor.constraint(equalTo: safeAreaLayoutGuide.centerYAnchor),
            swiftUIView.widthAnchor.constraint(equalTo: safeAreaLayoutGuide.widthAnchor)
        ])
    }

    func updateSwiftUIView(content: @escaping (() -> String)) {
        hostingVC.rootView.content = content
    }
}

// MARK: - View Controller

final class ViewController: UIViewController {
    private let uikitView = UIKitView()

    override func loadView() {
        view = uikitView
    }

    override func viewDidLoad() {
        super.viewDidLoad()

        DispatchQueue.main.asyncAfter(deadline: .now() + 1) { [weak self] in
            self?.uikitView.updateSwiftUIView {
                "Updated SwiftUI Content"
            }
        }
    }
}

#Preview {
    ViewController()
}
```
