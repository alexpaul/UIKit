# iOS Assessment

This assessment covers the following topics in iOS: 

* UIKit 
* View Layout - Auto Layout via Storyboard or Programmatically 
* Animation
* Windows and Screens 
* Persistence e.g UserDefaults, FileManager or Core Data
* Networking with URLSession, Codable 
* MapKit

## Rubric 

| Evaluation | Description |
|:------:|:------:|
| MVC Architecture | Clean MVC architecture, avoid M(assive) V(iew) C(controllers) by separating application / business logic |
| DRY | Use of function, do not repeat yourself |
| Unit Test | Unit test your functions |
| Animation | Subtle animations where it will deliver great user feedback |
| Naming Conventions | Adhere to good naming conventions e.g s`truct person` should be `struct Person` |
| Memory Managment | Break retain cycles where needed e.g `[weak self]` if capturing `self` by an unowned object |
| Encapsulation | Use custom initializers and dependency injection to pass data and marking your properties private to avoid mutating from outside objects |
| URLSession | Please write native netowrking code and don't use a third party library e.g `Alamofire` |


## Assessment 

During COVID we all look to enjoy the outdoors when we can and want to help our local restaurants while doing so. In this application you will be building an outdoor dining app. 

Yelp API `https://api.yelp.com/v3/businesses/search?term=coffee&location=10023`

The app will perform the following: 

- [ ] As a user I can search for restaurants around me, or by provided location. 
- [ ] As a user I am able to see restaurants on a map. 
- [ ] As a user I can tag a restaurant as providing outdoor dining options. 
- [ ] As a user I can view all my tagged restaurnats in a Profile page. 
