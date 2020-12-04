# URLSession Cheatsheet

## Using a closure to capture the `Result` of the asynchronous network request 

```swift 
func fetchWebData(completion: @escaping (Result<[ModelObjects], Error>) -> ()) {
 // netowrking code here
}
```

## Perform a GET request 

```swift 
let dataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
  // networking code here
}
dataTask.resume()
```
