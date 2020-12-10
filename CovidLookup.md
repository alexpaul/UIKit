# CovidLookup

## Objective

* Creating a Swift model(s) from Web API JSON data.
* Using `URLSession` natively. 
* Populating a Table View with parsed JSON data. 

## COVID API

1. Given the following endpoint: `https://api.covid19api.com/summary` verify the JSON payload via Postman or other. 
1. Create a model(s) and an API client using the endpoint above. 
1. Convert JSON property names to Swifty names using `CodingKeys` as needed.
1. Populate a Table View with the `countries` array from the JSON data. 

![covid lookup](https://user-images.githubusercontent.com/1819208/101109279-85c32700-35a4-11eb-9f58-864bbc5fdf5a.png)

Bonus: 
1. Add a search bar to the view controller. 
2. The user should be able to search by country name e.g `Saint Lucia`
3. Add a Table View Header to show Global Statistics of Covid cases.
