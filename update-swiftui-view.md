# Updating a SwiftUI view from a UIKit app

## 1. Using `ObservableObject` and `@Published` Property Wrapper

![Screenshot 2023-11-09 at 7 22 31 PM](https://github.com/alexpaul/UIKit/assets/1819208/54e2aea1-c6a7-40e7-a239-3e1f12be87a7)

```swift
import UIKit
import SwiftUI

// MARK: - Model
final class Content: ObservableObject {
    @Published var imageName: String

    init(imageName: String) {
        self.imageName = imageName
    }
}

// MARK: - SwiftUI Views
struct ImageCard: View {
    @ObservedObject var content: Content

    var body: some View {
        VStack(alignment: .leading) {
            Image(content.imageName)
                .resizable()
                .aspectRatio(contentMode: .fit)
                .cornerRadius(8)
        }
    }
}

// MARK: - UIKit Views

final class UIKitView: UIView {
    private var content = Content(imageName: "ocean")

    private lazy var hostingVC = UIHostingController(
        rootView: ImageCard(
            content: content
        )
    )

    private lazy var swiftUIView: UIView = {
        hostingVC.view
    }()

    override init(frame: CGRect) {
        super.init(frame: frame)
        backgroundColor = .systemBackground

        DispatchQueue.main.asyncAfter(deadline: .now() + 2) { [weak self] in
            self?.content.imageName = "room"
        }
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
            swiftUIView.leadingAnchor.constraint(equalTo: safeAreaLayoutGuide.leadingAnchor, constant: 20),
            swiftUIView.trailingAnchor.constraint(equalTo: safeAreaLayoutGuide.trailingAnchor, constant: -20),
        ])
    }
}

// MARK: - View Controller

final class ViewController: UIViewController {
    override func loadView() {
        view = UIKitView()
    }
}

#Preview {
    ViewController()
}
```

***

## 2. Using `rootView`

![Screenshot 2023-11-07 at 4 04 32 PM](https://github.com/alexpaul/UIKit/assets/1819208/001cd381-c115-4600-867e-b8482be45297)

try? it out

```swift
import UIKit
import SwiftUI

// MARK: - SwiftUI Views
struct SwiftUIView: View {
    var content: String

    var body: some View {
        Text(content)
    }
}

// MARK: - UIKit Views

final class UIKitView: UIView {
    private let hostingVC = UIHostingController(
        rootView: SwiftUIView(
            content: "Initial SwiftUI Content"
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

    func updateSwiftUIView(content: String) {
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
            self?.uikitView.updateSwiftUIView(content: "Updated SwiftUI Content")
        }
    }
}

#Preview {
    ViewController()
}
```
