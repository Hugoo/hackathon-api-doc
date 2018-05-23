**Enter leboncoin (light) API**
----

- [Introduction](#introduction)
- [Search Parameters](#search-parameters)
  - [**filters**](#filters)
    - [- **category:**](#category)
    - [- **keywords:**](#keywords)
    - [- **price range**](#price-range)
    - [- **location:**](#location)
    - [- **urgent:**](#urgent)
  - [**limit**](#limit)
  - [**sort_by**](#sort-by)
  - [**sort_order**](#sort-order)
  - [**owner_type**](#owner-type)
- [Samples](#samples)
- [Dictionaries](#dictionaries)
  - [Categories](#categories)
  - [French Regions:](#french-regions)
- [Final Notes](#final-notes)

# Introduction
This is a very simple API documentation that you can use to fill your database with actual but anonymized data.

Each team will receive an api-key to be able to request the API. If you don't have one, please ask Gildas, he'll be happy to provide you with one.

There is a limit rate to the api: 1000 requests / hours. If you need more, please reach out to us so that we can satisfy your need :)

We've opened an API endpoint to let you search our listing of ads. It's a simple POST request that accept parameters in the body and returns an array of results.

* **Base URL**: `http://35.233.99.28`

* **Method available :**
  `POST`

* **API KEY**

  You'll need to set the api-key in the header as follow:  
  `api-key: your_api_key`

  
*  **URL Params**

   You won't need to pass any URL params :)

* **Data Params**

  It's inside the body that you'll be able to specify some parameters to refine your request. You'll be able to precise the location, the category of product or even ads price. Let's dig into the parameters!

# Search Parameters

Here are the highest level params for the body of your request:

```json
{
  "filters": Object,
  "limit": Int,
  "sort_by": String,
  "sort_order": String,
  "owner_type": String
}
```

Let's see the values each of them can take, and what it means for your request.

## **filters**

The `filters` key is the main place where you refine your request. It's an object that can take several different keys:

### - **category:**

  The `category` param is an object with the id of the category. You can find a dictionnary of these category above. This will let you precise the category you're willinng to do the search.

  Ex:

  ```json
  {
    "filters": {
      "category": {
        "id": "39"
      }
    }
  }
  ```

  *Note: you cannot search for various categories at the same time*


### - **keywords:**

  The `keywords` key lets you filter your search according to some... well, keywords.

  Here's how it looks like:

  ```json
  {
    "filters": {
      "keywords": {
        "text": "software engineer job leboncoin"
        },
    }
  }
  ```

  By default, it will search both in the title of an ad and its description. You can decide to search only in the title of the ad by adding a key `type` of value `subject` as follows:

  ```json
  {
    "filters": {
      "keywords": {
        "type": "subject",
        "text": "air jordan"
      }
    }
  }
  ```

### - **price range**

  You can also filter your search by specifying a price range, a min and/or a max. Units are in euros. It's as simple as:

  With a price range:

  ```json
  {
    "filters": {
      "range": {
        "price": {
          "min": 15,
          "max": 50
        }
      }
    }
  }
  ```

  With a price minimum only:

  ```json
  {
    "filters": {
      "range": {
        "price": {
          "min": 15
          }
      }
    }
  }
  ```

  With a price maximum only:

  ```json
  {
    "filters": {
      "range": {
        "price": {
          "max": 2000
          }
      }
    }
  }
  ```


### - **location:**

  You can filter your search by location. There are two ways of doing so. The first one is by giving latitudes, longitudes and a radius keys (in meters), while the second one let's you do it by giving a French region or department.

  With coordinates:

  ```json
    {
      "filters": {
        "location": {
          "area": {
            "lat": 48.8729527,
            "lng": 2.3573570999999998,
            "radius": 30000
          }
        }
      }
    }
  ```

  With region or department:

  ```json
    {
      "filters": {
        "location": {
          "region": "12",
          "department": "75"
        }
      }
    }
  ```

  *Note: you can find a dictionnary of the regions below*

  *Note 2: you cannot cross several regions or department at the same time:*
    ```
      "location": {
        "region": ["12", "14"],
        "department": ["75", "23"]
      }
      ``` wont work.

### - **urgent:**

  You can request only ads that are labeled "urgent" on leboncoin. Users need to pay a small fee to be able to do so.

  To actually do it, you'll need to add an `enums` key with urgent key nested inside, as follows:

  ```json
  {
    "filters": {
      "enums": {
        "urgent": ["1"]
      }
    }
  }
  ```


## **limit**

You can precise a limit in the number of items you want to receive.
```json
{
  "filters": {...},
  "limit": 30
}
```

## **sort_by**

You can sort your result by date, and price. Here's how we do it:

```json
{
  "filters": {...},
  "sort_by": "time"
}
```

```json
{
  "filters": {...},
  "sort_by": "price"
}
```

## **sort_order**

When you sort your results, you can specify in you want the more recent (or the older) ads as well as from the cheapest or the most expensive ones:

From the highest value to the lowest:

```json
{
  "filters": {...},
  "sort_order": "desc"
}
```


From the lowest value to the highest:

```json
{
  "filters": {...},
  "sort_order": "asc"
}
```

## **owner_type**

The `owner_type` key lets you filter the ads depending on the type of user who posted it: a professional vs. a private person. Here's how it works:

To get ads posted by pros, simply add:

```json
{
  "filters": {...},
  "owner_type": "pro"
}
```

To get ads posted by private persons, simply add:

```json
{
  "filters": {...},
  "owner_type": "private"
}
```

*Note: by default, if you don't specify that field, you'll search for results that mixes both private persons and professionals*

# Samples

Here are couple of examples of search params that you might want to put in a request to the API, using a combination of various parameters:

A simple one:

```json
{
  "filters": {
    "keywords": {
      "text": "job leboncoin"
    }
  },
  "sort_by": "date",
  "sort_order": "asc",
  "limit": 35
}
```

A more complex one:

```json
{
  "filters": {
    "category": {
      "id": "53"
    },
    "enums": {
      "urgent": ["1"]
    },
    "keywords": {
      "type": "subject",
      "text": "nike"
    },
    "location": {
      "area": {
        "lat": 48.8732495,
        "lng": 2.356591,
        "radius": 30000
      }
    },
    "ranges": {
      "price": {
        "max": 50
      }
    }
  },
  "owner_type": "private",
  "sort_by": "price",
  "sort_order": "asc",
  "limit": 35
}
```

# Dictionaries

As mentioned above, here are tables to let you map categories and French regions to their matching ids (department ids are the one you already know, ie: Paris === '75'):

## Categories

_Note: the table below groups sub-categories together, preceded by the CATEGORY NAME (followed by a *). In the API, you can treat categories and subcategories the same way, but the name of the key will always be `category`_

**Category Id**|**Category Name**
:-----:|:-----:
71|JOBS*
33|Job offer
1|VEHICLES
2|Cars
3|Motos
4|Caravans
5|Transport Utilities
6|Car Equipement
44|Moto Equipement
50|Caravan Equipement 
7|Boating
51|Boating Equipement
8|REAL ESTATE*
9|Real estate sales
10|Real estate Locations
11|Real estate Colocations
13|Offices & Shops
66|HOLIDAYS RENTALS*
12|Rentals & Houses
67|Guest Rooms
68|Campings
69|Hotels
70|Unusual Accommodation
14|MULTIMEDIA*
15|Computers & Equipment
43|Video games & Game Consoles
16|Audio & Video
17|Phones
24|ENTERTAINMENT*
25|DVD / Movies
26|CD / Music
27|Books
28|Animals
55|Cycling
29|Sports & Hobbies
30|Music Instruments
40|Collections
41|Toys & Games
48|Wines & Food
56|PROFESSIONNAL EQUIPMENT*
57|Agricultural Material
58|Transport - Handling
59|Construction, sites
60|Construction Tooling
32|Industrials Equipements
61|Catering - Hotel
62|Office Supplies
63|Shops & Markets
64|Medical Equipment
31|SERVICES*
34|Service Delivery
35|Ticketing
49|Events
36|Private Lessons
65|Carpooling
18|HOME*
19|Furnishing
20|Home Appliances
45|Decorative arts, tableware
39|Decoration
46|Linens
21|Craft Equipment
52|Gardening
22|Clothes
53|Shoes
47|Accessories & Luggage
42|Watches & Jewelery
23|Baby Equipement
54|Baby Clothes
37|OTHERS*
38|Others

## French Regions:

TO DO: get the list of regions with their ids

# Final Notes

  Keep in mind that the data you'll get are real ones, despite they're anonymised (aka: no phone number nor email adresses).

  These data are here for you to be able to build something around it, to tweak them or to simply let you use real data for your demo.

  Mentors from leboncoin are here for you so do not hesitate to reach out to us for any question or advice :)

  Please, be respectful, be bold and have fun hacking!