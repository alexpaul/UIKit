# URLSession Cheatsheet

## Vocabulary 

* JSON
* endpoint 
* RESTFul API
* URLSession 
* URLSession.shared
* JSONDecoder, JSONEncoder 
* URL 
* URLRequest 
* URLResponse
* HTTPURLResponse
* Data 
* Codable
* Encodable 
* Decodable 
* HTTP methods: GET, POST, PUT, DELETE, PATCH, UPDATE
* Asynchronous
* Result 
* @escaping closures 
* capture list e.g [weak self], [unowned self]
* weak vs unowned 

## Using a closure to capture the `Result` of the asynchronous network request 

`Result` type is an `enum` type that has two arguments, a `success` state and an `failure` state. 

```swift 
func fetchWebData(completion: @escaping (Result<[ModelObject], Error>) -> ()) {
 // netowrking code here
}
```

## Perform a GET request using `URLSession`

`URLSession.shared` is a singleton instance on `URLSession` with basic networking configurations. 

```swift 
let dataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
  // networking code here
}
dataTask.resume()
```

## Check that the `HTTPURLResponse` status code is within the valid range of `200...299` indicating a successful response

```swift 
guard let httpResponse = response as? HTTPURLResponse,
      (200...299).contains(httpResponse.statusCode) else {
  print("bad status code")
  return
}
```

## Converting `JSON` data to Swift objects 

```swift 
do {
  let topLevelModel = try JSONDecoder().decode(TopLevelModel.self, from: jsonData)
  let modelObjects = topLevelModel.modelObjects
  completion(.success(modelObjects)
} catch {
  // decoding error
  completion(.failure(error))
}
```

## Completed API Client to fetch web data 

```swift 
 func fetchWebData(completion: @escaping (Result<[ModelObject], Error>) -> ()) {
    // 1. - endpoint URL string
    let endpointURLString = "https://........"
    
    // 2. - convert the string to an URL
    guard let url = URL(string: endpointURLString) else {
      print("bad url")
      return
    }
    
    // URL vs URLRequest
    
    // 3. - make the request using URLSession
    // .shared is an singleton instance on URLSession comes with basic configuration needed for most requests
    let dataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
      if let error = error {
        return completion(.failure(error))
      }
      
      // first we have to type cast URLResponse to HTTPURLRepsonse to get access to the status code
      // we verify the that status code is in the 200 range which signals all went well with the GET request
      guard let httpResponse = response as? HTTPURLResponse,
            (200...299).contains(httpResponse.statusCode) else {
        print("bad status code")
        return
      }
      
      if let jsonData = data {
        // convert data to our swift model
        do {
          let topLevelModel = try JSONDecoder().decode(TopLevelModel.self, from: jsonData)
          let modelObjects = topLevelModel.modelObjects
          completion(.success(modelObjects))
        } catch {
          // decoding error
          completion(.failure(error))
        }
      }
    }
    dataTask.resume()
  }
```
