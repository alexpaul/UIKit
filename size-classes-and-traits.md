# Size Classes, UIKit and SwiftUI

> Note: Built against iOS 15.

This demo uses two layouts based on the current size class `UIUserInterfaceSizeClass`. 
* If in the `.compact` size class there is one item per row (or one column). 
* If in the `.regular` size class there are two items per row (or two columns). 

### iPad

Shows the following: 
* `.regular` size class
* `.compact` size class when in split view

https://github.com/alexpaul/UIKit/assets/1819208/69ad52d0-2fe8-4a19-9170-174ee6533cfe

### iPhone 15 Pro Max

Shows the following: 
* `.compact` size in portrait
* `.regular` size in landscape

https://github.com/alexpaul/UIKit/assets/1819208/6c4a711a-8266-49b4-bd9f-366800a1c69b

try? it out 

```swift
// MARK: - Model

struct Item: Hashable {
    var title: String
    var image: String
    
    static var data: [Item] {
        [
            Item(
                title: "Share",
                image: "square.and.arrow.up"
            ),
            Item(
                title: "Bookmark",
                image: "bookmark"
            ),
            Item(
                title: "Cart",
                image: "cart"
            ),
            Item(
                title: "Credit Card",
                image: "creditcard"
            ),
            Item(
                title: "Settings",
                image: "gear"
            ),
            Item(
                title: "Favorite",
                image: "star"
            )
        ]
    }
}

// MARK: - SwiftUI Views

struct ItemView: View {
    let item: Item
    
    var body: some View {
        Button(action: {}) {
            HStack {
                Image(systemName: item.image)
                Text(item.title)
            }
            .frame(maxWidth: .infinity)
            .foregroundColor(.primary)
            .padding(8)
        }
        .overlay {
            RoundedRectangle(cornerRadius: 16)
                .stroke(lineWidth: 1)
        }
    }
}

struct RegularLayoutView: View {
    let columns = [GridItem(.flexible()), GridItem(.flexible())]

    var body: some View {
        VStack {
            LazyVGrid(columns: columns, spacing: 20) {
                ForEach(Item.data, id: \.self) { item in
                    ItemView(item: item)
                }
            }
        }
    }
}

struct CompactLayoutView: View {
    var body: some View {
        VStack(spacing: 20) {
            ForEach(Item.data, id: \.self) { item in
                ItemView(item: item)
            }
        }
    }
}

// MARK: - UIKit Views

final class ContentView: UIView {
    private var isCurrentLayoutApplied = false
    
    private let stackView: UIStackView = {
        let stack = UIStackView()
        return stack
    }()
    
    private let compactView: UIView = {
        let hostingVC = UIHostingController(rootView: CompactLayoutView())
        return hostingVC.view
    }()
    
    private let regularView: UIView = {
        let hostingVC = UIHostingController(rootView: RegularLayoutView())
        return hostingVC.view
    }()
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        backgroundColor = .systemBackground
        addSubViews()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func layoutSubviews() {
        super.layoutSubviews()
        activateConstraintsForStackView()
        configureView(for: traitCollection.horizontalSizeClass)
    }
    
    override func traitCollectionDidChange(_ previousTraitCollection: UITraitCollection?) {
        super.traitCollectionDidChange(previousTraitCollection)
        if traitCollection.horizontalSizeClass != previousTraitCollection?.horizontalSizeClass {
            isCurrentLayoutApplied = false
            stackView.arrangedSubviews.forEach { $0.removeFromSuperview() }
        }
    }
    
    private func addSubViews() {
        addSubview(stackView)
        stackView.translatesAutoresizingMaskIntoConstraints = false
    }
    
    private func configureView(for horizontalSizeClass: UIUserInterfaceSizeClass) {
        if !isCurrentLayoutApplied {
            if horizontalSizeClass == .compact {
                applyCompactStackView()
            } else {
                applyRegularStackView()
            }
        }
        isCurrentLayoutApplied = true
    }
    
    private func activateConstraintsForStackView() {
        NSLayoutConstraint.activate([
            stackView.centerXAnchor.constraint(equalTo: safeAreaLayoutGuide.centerXAnchor),
            stackView.centerYAnchor.constraint(equalTo: safeAreaLayoutGuide.centerYAnchor),
            stackView.widthAnchor.constraint(equalTo: widthAnchor, multiplier: 0.8),
            stackView.heightAnchor.constraint(equalTo: safeAreaLayoutGuide.heightAnchor, multiplier: 0.6)
        ])
    }
    
    private func applyCompactStackView() {
        stackView.addArrangedSubview(compactView)
    }
    
    private func applyRegularStackView() {
        stackView.addArrangedSubview(regularView)
    }
}

// MARK: - View Controller

final class ViewController: UIViewController {
    override func loadView() {
        view = ContentView()
    }
}
```
