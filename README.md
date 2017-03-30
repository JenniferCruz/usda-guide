# USDA Food Composition Databases
## List API, Search API & Food Report Version 2 API
### Getting Started Guide
_____

### Table of content
1. [Which API should I use?](#chooseapi)
2. [The API Key](#apikey)
3. [The List API](#listapi)
4. [The Search API](#searchapi)
5. [The Food Report Version 2 API](#foodapi)
6. [About](#aboutthis)
___

### <a name="chooseapi"></a>1. Which API should I use?


| Use the         | To get      | What you'll get / Notes |
| ------          | ----------- | -----|
| LIST API        | a list of up to 1500 foods, nutrients or food groups | __you don't get to specify any search term__, so you get a pseudo-arbitrary list. Every result in the resultset will contain its _name_ and its _id_, _group number_ or _nutrient number_|
| SEARCH API      | a list of up to 1500 foods __matching a search term__ | Every food in the resultset will contain its _name_, _ndbno_ (id), _data source_ (Branded Food Products or Standard Release) and _food group_ (Beverages, Branded Food Products, Spices and Herbs, Baked Products, etc...) |
| FOOD REPORT API | a list of up to 50 foods with their nutrients | You need to know the ndbno (id) of each food you want to request. You can get it __through the Search API__ or __the List API__ |

_____

### <a name="apikey"></a>2. The API Key
You will need a key provided by [Data.gov](https://www.data.gov/) for all your requests. 
Data.gov is _"the home of the U.S. Government’s open data"_. 

#### To get the API key
 * Go to the [NDB API website](https://ndb.nal.usda.gov/ndb/api/doc#)
 * Look for the __Gaining Access__ section.
 * Click on `sign up` (or something similar)
 * Fill out the form
 * You should get the API key right away and/or at an email.


_______________________________

### <a name="listapi"></a>3. The List API

You may request a list of up to 1,500 foods, nutrients or food groups.

#### Making a REQUEST
##### URL Pattern 

    https://api.nal.usda.gov/ndb/list?format=JSON_or_XML&amplt=LIST_TYPE&ampsort=BY_NAME_OR_ID&ampmax=NUMBER_OF_ITEMS_IN_RESPONSE&offset=DEFAULT_IS_0&api_key=DEMO_KEY

An actual request might look like this:

    https://api.nal.usda.gov/ndb/list?format=json&lt=f&sort=n&max=3&offset=0&api_key=DEMO_KEY

##### The request parameters in detail
* __api_key__: required _(see section 2: [The API key](#apikey))_
* __format__ : the response's format. `xml` or `json` _(default)_.
* __lt__ : list type. Possible options are:
    * `f` = food _(default)_
    * `n` = all nutrients
    * `ns`= specialty nutrients
    * `nr`= standard release nutrients only
    * `g` = food group
* __sort__ : Possible options are:
    * `n` : name
    * `id`: may refer to:
        * nutrient number for a nutrient list
        * NDBno for a foods list 
        * food group id for a food group list
* __max__ : default is 50. Maximum per request is 1500.
* __offset__: default is 0. If you specify a number larger than 0, their response will ignore the items in their resulset in positions lower than your specified value.

#### The RESPONSE 

The exact response, of course, will vary according to the request parameters. To give you an idea of what you might be dealing with, here's an example:

__The request__

    https://api.nal.usda.gov/ndb/list?format=json&lt=f&sort=n&max=2&offset=385&api_key=DEMO_KEY
    
__Produced the following JSON result__

    {
        "list": {
            "lt": "f",
            "start": 385,
            "end": 2,
            "total": 2,
            "sr": "28",
            "sort": "n",
            "item": [
                {
                    "offset": 385,
                    "id": "45237594",
                    "name": "100% NATURAL COCONUT FLAVORED SPARKLING WATER, UPC: 012993102029"
                },
                {
                    "offset": 386,
                    "id": "45143296",
                    "name": "100% NATURAL COCONUT WATER, UPC: 042743191706"
                }
            ]
        }
    }


_______________________________

### <a name="searchapi"></a>4. The Search API
You may request a list of foods which contain at least one of the query keywords, in the food description or name (scientific or commercial).

#### Making a REQUEST

##### URL Pattern 

    https://api.nal.usda.gov/ndb/search/?q=SEARCH_TEARMS&format=JSON_or_XML&sort=NAME_or_RELEVANCE&max=NUMBER_OF_ITEMS&offset=DEFAULT_IS_0&api_key=DEMO_KEY

An actual request might look like this:

    https://api.nal.usda.gov/ndb/search/?q=raw%20pepper&format=json&sort=r&max=5&offset=0&api_key=DEMO_KEY

##### The request parameters in detail
* __api_key__: required (see section 2: [The API key](#apikey))
* __q__: Search terms.
* __ds__: Data source. Must be either `Branded Food Products` or `Standard Reference`.
* __fg__: Food group ID.
* __sort__: Sort the results by:
    * `n`: food name
    * `r`: search relevance (how closely a food matches the query terms). This is the default.
* __max__: maximum rows to return. Default is 50.	
* __offset__: beginning row in the result set to begin. Default is 0.
* __format__: json _(default)_ or xml.


#### The RESPONSE 

The exact response, of course, will vary according to the request parameters. To give you an idea of what you might be dealing with, here's an example:

__The request__

    https://api.nal.usda.gov/ndb/search/?q=raw%20pepper&format=json&sort=r&max=5&offset=0&api_key=DEMO_KEY
    
__Produced the following JSON result__

    {
        "list": {
            "q": "pepper",
            "sr": "28",
            "ds": "any",
            "start": 0,
            "end": 2,
            "total": 2516,
            "group": "",
            "sort": "r",
            "item": [
                {
                    "offset": 0,
                    "group": "Vegetables and Vegetable Products",
                    "name": "Peppers, sweet, green, cooked, boiled, drained, without salt",
                    "ndbno": "11334",
                    "ds": "SR"
                },
                {
                    "offset": 1,
                    "group": "Vegetables and Vegetable Products",
                    "name": "Peppers, sweet, green, frozen, chopped, boiled, drained, without salt",
                    "ndbno": "11338",
                    "ds": "SR"
                }
            ]
        }
    }

_______________________________

### <a name="foodapi"></a>5. The Food Report Version 2 API
There are currently 2 versions available for the Food Report API (Version 1 and Version 2). They seem to be quite similar, but this guide was created for version 2.

The Food Report API allows you to request up to 50 specific food items, by specifying up to 50 ndbno (IDs). You will receive detailed nutrional information on them.

#### Making a REQUEST
##### URL Pattern 
    https://api.nal.usda.gov/ndb/V2/reports?ndbno=FOOD_ID&ndbno=ANOTHER_FOOD_ID&ndbno=AND_ANOTHER_FOOD_ID&type=BASIC_FULL_or_STATSformat=JSON_OR_XML&api_key=DEMO_KEY

An actual request might look like this:
    
    https://api.nal.usda.gov/ndb/V2/reports?ndbno=01009&ndbno=45202763&ndbno=35193&type=b&format=json&api_key=DEMO_KEY

##### The request parameters in detail
* __api_key__: required _(see section 2: [The API key](#apikey))_
* __ndbno__: at least one is required. This is the unique ID of the food, which you can obtain through the [list API](#listapi) or the [search API](#searchapi).
* __format__: _(optional)_ _xml_ or _json_ (default).
* __type__: 
    * `b` for basic (default). Contains limited set of nutrients, similar to that in a food package.
    * `f` for full. Contains all nutrients found for that food.
    * `s` for stats. Containts all statistical parameters available.
    * _Branded food products are only available in __basic__ format._    

    
#### The response

The exact response, of course, will vary according to the request parameters. To give you an idea of what you might be dealing with, here's an example:

__The request__

    https://api.nal.usda.gov/ndb/V2/reports?ndbno=35193&type=b&format=json&api_key=DEMO_KEY

__Produced the following JSON result__

(If you find it hard to read, I suggest you paste it in a [tool to visualize JSON files](https://www.google.com/search?q=visualize%20json))

    {
        "foods":
            [
                {"food":
                    {
                        "sr":"28",
                        "type":"b",
                        "desc":{"ndbno":"35193","name":"Agave, cooked (Southwest)","ds":"Standard Reference","ru":"g"},
                        "nutrients":[
                            {"nutrient_id":"255","name":"Water","group":"Proximates","unit":"g","value":"65.40","measures":[]},
                            {"nutrient_id":"208","name":"Energy","group":"Proximates","unit":"kcal","value":"135","measures":[]},
                            {"nutrient_id":"203","name":"Protein","group":"Proximates","unit":"g","value":"0.99","measures":[]},
                            {"nutrient_id":"204","name":"Total lipid (fat)","group":"Proximates","unit":"g","value":"0.29","measures":[]},
                            {"nutrient_id":"205","name":"Carbohydrate, by difference","group":"Proximates","unit":"g","value":"32.00","measures":[]},
                            {"nutrient_id":"291","name":"Fiber, total dietary","group":"Proximates","unit":"g","value":"10.6","measures":[]},
                            {"nutrient_id":"269","name":"Sugars, total","group":"Proximates","unit":"g","value":"20.87","measures":[]},
                            {"nutrient_id":"301","name":"Calcium, Ca","group":"Minerals","unit":"mg","value":"460","measures":[]},
                            {"nutrient_id":"303","name":"Iron, Fe","group":"Minerals","unit":"mg","value":"3.55","measures":[]},
                            {"nutrient_id":"304","name":"Magnesium, Mg","group":"Minerals","unit":"mg","value":"39","measures":[]},
                            {"nutrient_id":"305","name":"Phosphorus, P","group":"Minerals","unit":"mg","value":"9","measures":[]},
                            {"nutrient_id":"306","name":"Potassium, K","group":"Minerals","unit":"mg","value":"59","measures":[]},
                            {"nutrient_id":"307","name":"Sodium, Na","group":"Minerals","unit":"mg","value":"13","measures":[]},
                            {"nutrient_id":"309","name":"Zinc, Zn","group":"Minerals","unit":"mg","value":"0.25","measures":[]},
                            {"nutrient_id":"401","name":"Vitamin C, total ascorbic acid","group":"Vitamins","unit":"mg","value":"0.3","measures":[]},
                            {"nutrient_id":"404","name":"Thiamin","group":"Vitamins","unit":"mg","value":"0.012","measures":[]},
                            {"nutrient_id":"405","name":"Riboflavin","group":"Vitamins","unit":"mg","value":"0.099","measures":[]},
                            {"nutrient_id":"406","name":"Niacin","group":"Vitamins","unit":"mg","value":"0.162","measures":[]},
                            {"nutrient_id":"415","name":"Vitamin B-6","group":"Vitamins","unit":"mg","value":"0.087","measures":[]},
                            {"nutrient_id":"435","name":"Folate, DFE","group":"Vitamins","unit":"\u00b5g","value":"3","measures":[]},
                            {"nutrient_id":"418","name":"Vitamin B-12","group":"Vitamins","unit":"\u00b5g","value":"0.00","measures":[]},
                            {"nutrient_id":"320","name":"Vitamin A, RAE","group":"Vitamins","unit":"\u00b5g","value":"6","measures":[]},
                            {"nutrient_id":"318","name":"Vitamin A, IU","group":"Vitamins","unit":"IU","value":"113","measures":[]},
                            {"nutrient_id":"323","name":"Vitamin E (alpha-tocopherol)","group":"Vitamins","unit":"mg","value":"0.36","measures":[]},
                            {"nutrient_id":"328","name":"Vitamin D (D2 + D3)","group":"Vitamins","unit":"\u00b5g","value":"0.0","measures":[]},
                            {"nutrient_id":"324","name":"Vitamin D","group":"Vitamins","unit":"IU","value":"0","measures":[]},
                            {"nutrient_id":"430","name":"Vitamin K (phylloquinone)","group":"Vitamins","unit":"\u00b5g","value":"4.9","measures":[]},
                            {"nutrient_id":"601","name":"Cholesterol","group":"Lipids","unit":"mg","value":"0","measures":[]},
                            {"nutrient_id":"262","name":"Caffeine","group":"Other","unit":"mg","value":"0","measures":[]}],
                        "footnotes":[]
                    }
                }
            ],
        "count":1,
        "notfound":0,
        "api":2.0
    }


___
### <a name="aboutthis"></a>6. About this guide

This is an _unofficial guide_ on how to use the listed APIs of the [USDA Food Composition Databases](https://ndb.nal.usda.gov/ndb/api/doc).
 It was developed on 03/2017, based on their documentation, but with what is to me a more user-friendly style (of course, we might have different opinions on what's _user-friendly_). 
 For up-to-date information or additional details, you should probably go the [official website](https://ndb.nal.usda.gov/ndb/api/doc).

#### Author
[Jennifer M. Cruz](https://github.com/JenniferCruz/)

