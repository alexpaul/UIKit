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

## Converting `JSON` data to Swift objects 

```swift 
do {
  let topLevelModel = try JSONDecoder().decode(TopLevelModelself, from: jsonData)
  completion(.success(topLevelModel)
} catch {
  // decoding error
  completion(.failure(error))
}
```
