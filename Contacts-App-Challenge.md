# Contacts app

## Level 1 

#### Prerequisties 

* Arrays
* Dictionaries 
* Working with UIViewController's
* UITableView and UITableViewDataSource

Contacts Dictionary 

```swift 
let contactsDict = [03364152046: ("Christin", "Böttger"),
                    927525456: ("Joaquin", "Bravo"),
                    6868840334: ("David", "Edwards"),
                    07905753: ("Roope", "Mattila"),
                    27991860: ("Lærke", "Wist"),
                    957021797: ("Jonathan", "Diez"),
                    01768757320: ("Emily", "Long"),
                    0501439641: ("Noe", "Roussel"),
                    375351453: ("Justin", "Harris"),
                    3028950023: ("Ezra", "Lee"),
                    0478121870: ("Ninon", "Bernard"),
                    60749217: ("Helene", "Strange"),
                    7638623154: ("Estefânia", "Barros"),
                    2945132492: ("Gül", "Sinanoğlu"),
                    1963139555: ("George", "Miller"),
                    64513463: ("Cecilie", "Peterson"),
                    01539627648: ("Jared", "Mitchelle"),
                    0157693915: ("Valdelaine", "de Souza"),
                    07798852536: ("Kristin", "Tausch"),
                    00499228235: ("Marissa", "Rode"),
]
```

1. Use the contacts dictionary provided to create an array of `Contact` objects. 
2. Show the list of contacts in a table view. 
2. Use the built-in table view cell's subtitle option:
   1. Show the contact's first and last name on the cell's text label. Create a computed property in your Contact struct to return `fullname`.
   2. Show the contact's phone number in the detail text label

## Level 2 

#### Prerequisties 

* UITableView
* UITableViewDataSource
* UITableViewDelegate
* Segueing in iOS 
* Custom Delegation, Unwind Segue
* UIImagePickerController

Build a Contacts app that does the following: 

1. User should be able to add new contact. 
2. User should be able to add at minimum the following: first name, last name, photo, a phone number and email. 
3. User should be able to click on a contact and segue to a detail screen.
4. User should be able to click on a phone number in the detail screen to place a phone call.
5. User should be able to delete a contact. 
6. Contacts data should persist through app launches.

