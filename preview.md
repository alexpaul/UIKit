# Previewing UIKit Views 

<img width="1694" alt="Screenshot 2023-11-03 at 10 28 40 PM" src="https://github.com/alexpaul/UIKit/assets/1819208/8ab6c7a3-6cb4-438a-8880-fddb87a993d6">

try it out? 

```swift
import UIKit
import SwiftUI

struct Shoe: Hashable {
    var imageName: String

    static var data: [Shoe] {
        [
            Shoe(imageName: "bondi-8-black"),
            Shoe(imageName: "bondi-8-orange"),
            Shoe(imageName: "rocket-x-2-black"),
            Shoe(imageName: "rocket-x-2-green")
        ].shuffled()
    }
}

struct ShoeViewer: View {
    var body: some View {
        TabView {
            ForEach(Shoe.data, id: \.self) { shoe in
                Image(shoe.imageName)
                    .resizable()
                    .aspectRatio(contentMode: .fit)
            }
        }
        .tabViewStyle(.page(indexDisplayMode: .always))
    }
}

final class UIKitView: UIView {
    let title: UILabel = {
        let label = UILabel()
        label.text = "Shoe Viewer"
        return label
    }()

    let shoeViewer: UIView = {
        let hostingVC = UIHostingController(rootView: ShoeViewer())
        hostingVC.view.backgroundColor = .black
        return hostingVC.view
    }()

    override init(frame: CGRect) {
        super.init(frame: frame)
        backgroundColor = .white
        setupConstraints()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    private func setupConstraints() {
        addSubview(title)
        title.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            title.topAnchor.constraint(equalTo: safeAreaLayoutGuide.topAnchor, constant: 40),
            title.centerXAnchor.constraint(equalTo: centerXAnchor)
        ])

        addSubview(shoeViewer)
        shoeViewer.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            shoeViewer.topAnchor.constraint(equalTo: title.bottomAnchor, constant: 20),
            shoeViewer.centerXAnchor.constraint(equalTo: centerXAnchor),
            shoeViewer.heightAnchor.constraint(equalToConstant: 400),
            shoeViewer.widthAnchor.constraint(equalTo: safeAreaLayoutGuide.widthAnchor)
        ])
    }
}

final class ViewController: UIViewController {
    override func loadView() {
        view = UIKitView()
    }
}

#Preview {
    ViewController()
}
```
