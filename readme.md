**Enter leboncoin (light) API**
----

- [Introduction](#introduction)
- [Search Parameters](#search-parameters)
  - [**filters**](#filters)
    - [**category:**](#--category)
    - [**keywords:**](#--keywords)
    - [**price range**](#--price-range)
    - [**location:**](#--location)
    - [**urgent:**](#--urgent)
  - [**limit**](#limit)
  - [**sort_by**](#sort_by)
  - [**sort_order**](#sort_order)
  - [**owner_type**](#owner-type)
- [Code Examples](#code-examples)
  - [API Request](#api-request)
  - [API Response](#api-response)
- [Dictionaries](#dictionaries)
  - [Categories](#categories)
  - [French Regions:](#french-regions)
- [Final Notes](#final-notes)

# Introduction
This is a very simple API documentation that you can use to fill your database with actual but anonymized data.

Each team will receive an API Key to be able to request the API. If you don't have one, please ask some to our mentors, we'll be happy to provide you with one.

There is a limit rate to the api: 1000 requests / hour. If you need more, please reach out to us so that we can - reasonably - increase your limitation :)

We've opened a unique API endpoint to let you search our listing of ads. It's a simple POST request that accepts parameters in the body and returns an array of results.

* **API Endpoint URL**: `https://vivatech.leboncoin.io/search`

* **Method available :**
  `POST`

* **API KEY**

  You'll need to set the `api_key` in the header as follow:  
  `api_key: your_api_key`

  
*  **URL Params**

   You won't need to pass any URL params :)

* **Data Params**

  It's inside the body that you'll be able to specify some parameters to refine your request. You'll be able to filter by location, product category or to sort by ascending price order. Let's dig into the parameters!

# Search Parameters

Here are the highest level params to add to the body of your request:

```json
{
  "filters": Object,
  "limit": Int,
  "sort_by": String,
  "sort_order": String,
  "owner_type": String
}
```

Let's see the values each of them can take, and what to except as result.

## **filters**

The `filters` key is the main place where you refine your request. It's an object that can take several different keys:

### - **category:**

  The `category` param is an object with the id of the category. You can find a dictionnary of these category [above](#categories). This will let you precise the category you're willinng to do the search.

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

  You can filter your search by location. There are two ways of doing so. The first one is by giving latitudes, longitudes and a radius keys (in meters), while the second one let's you do it by giving a [French region or department ids](#french-regions).

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

  *Note
  bis: you cannot cross several regions or department at the same time:*
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

# Code Examples

## API Request

Here are couple of examples of search params that you might want to put in a request to the API, using a combination of various parameters:

A simple one to get the 35 most recent ads that contains the text "software engineer @leboncoin":

```json
{
  "filters": {
    "keywords": {
      "text": "software engineer job @leboncoin"
    }
  },
  "sort_by": "date",
  "sort_order": "desc",
  "limit": 35
}
```

The full cURL:
```shell
curl -X POST \
  https://vivatech.leboncoin.io/search \
  -H 'Content-Type: application/json' \
  -H 'api_key: user1' \
  -d '{"filters":{"keywords":{"text":"software engineer @leboncoin"}},"sort_by":"date","sort_order":"desc","limit":35}'`
```

*Note: the actual url to see current tech positions at leboncoin can be found [here](https://corporate.leboncoin.fr/nos-offres/?utm_source=github&utm_medium=repo&utm_content=api-doc&utm_campaign=hackathon-vivatech-api-25052018).*

A more complex one to get a "nike air force" ad with the parameters you'll guess (location is 30km around us):

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
      "text": "nike air force"
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
  "limit": 1
}
```

The full cURL:
```shell
curl -X POST \
  https://vivatech.leboncoin.io/search \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: aca30f84-302e-d56c-e2eb-d88eb284b40a' \
  -H 'api_key: user1' \
  -d '{
  "filters": {
    "category": {
      "id": "53"
    },
    "enums": {
      "urgent": ["1"]
    },
    "keywords": {
      "type": "subject",
      "text": "nike air force"
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
  "limit": 1
}'
```


## API Response

You'll receive an object with some meta data about your result, and an array of results matching your criteria.

```json
{
  "total": 1,
  "total_all": 1,
  "total_pro": 0,
  "total_private": 1,
  "ads": [
    {
      "list_id": 1421168173,
      "first_publication_date": "2018-04-23 16:01:19",
      "expiration_date": "2018-06-22 16:01:19",
      "index_date": "2018-04-23 16:01:19",
      "category_id": "53",
      "category_name": "Chaussures",
      "subject": "Nike air force ",
      "body": "Vend pers de air force bonne état  état prix non négociable ",
      "ad_type": "offer",
      "url": "https://www.leboncoin.fr/vi/1421168173.htm/",
      "price": [
        50
      ],
      "price_calendar": null,
      "images": {...},
      "attributes": [
        {
          "key": "shoe_type",
          "value": "2",
          "key_label": "Type",
          "value_label": "Homme",
          "generic": true
        },
        {
          "key": "shoe_size",
          "value": "29",
          "key_label": "Pointure",
          "value_label": "44",
          "generic": true
        }
      ],
      "location": {
        "region_id": "12",
        "region_name": "Ile-de-France",
        "department_id": "75",
        "department_name": "Paris",
        "city_label": "Paris 75014",
        "city": "Paris",
        "zipcode": "75014",
        "lat": 48.83277,
        "lng": 2.32487,
        "source": "city",
        "provider": "lbc",
        "is_shape": true
      },
      "owner": {
        "type": "private",
        "name": "djik's",
        "no_salesmen": true
      },
      "options": {...},
      "has_phone": true
    }
  ]
}
```

# Dictionaries

As mentioned above, here are tables to let you map categories and French regions to their matching ids (department ids are the one you already know, ie: Paris => "75"):

## Categories

_Note: the table below groups sub-categories together, preceded by the CATEGORY NAME (followed by a *). In the API, you can treat categories and subcategories the same way, but the name of the key will always be `category`_

**Category Id**|**Category Name**
:-----:|:-----:
71|JOBS*
33|Job offer
1|VEHICLES*
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

**Region Id**|**Region Name**
:-----:|:-----:
1|Alsace
2|Aquitaine
3|Auvergne
4|Basse-Normandie
5|Bourgogne
6|Bretagne
7|Centre
8|Champagne-Ardenne
9|Corse
10|Franche-Comté
11|Haute-Normandie
12|Ile-de-France
13|Languedoc-Roussillon
14|Limousin
15|Lorraine
16|Midi-Pyrénées
17|Nord-Pas-de-Calais
18|Pays de la Loire
19|Picardie
20|Poitou-Charentes
21|Provence-Alpes-Côte d'Azur
22|Rhône-Alpes
23|Guadeloupe
24|Martinique
25|Guyane
26|Réunion

# Final Notes

  Keep in mind that the data you'll query are real ones, but are anonymised (aka: no phone number nor email adresses).

  These data are here for you to be able to build something around it, to tweak them or to simply let you use real data for your demo.

  Mentors from leboncoin are here to help, so do not hesitate to reach out to us for any question or advice :)

  Please, be respectful, be bold and have fun hacking!
