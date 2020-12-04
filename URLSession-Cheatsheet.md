# URLSession Cheatsheet

## Perform a GET request 

```swift 
let dataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
  // networking code here
}
dataTask.resume()
```
