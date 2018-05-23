**Enter leboncoin (light) API**
----
# Introduction
This is a very simple API documentation that you can use to fill your database with actual but anonymized data.

Each team will receive an api-key to be able to request the API. If you don't have one, please ask Gildas, he'll be happy to provide you with one.

There is a limit rate to the api: 1000 requests / hours. If you need more, please reach out to us so that we can satisfy your need :)

We've open an api endpoint to let you search our listing of ads. It's a simple POST request that accept parameters in the body.

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

## Search Parameters

Here are the highest level params for the body of your request:

```json
{
  "filters": Object,
  "limit": Int,
  "sort_by": String,
  "sort_order": String
}
```

Let's see the values each of them can take, and what it means for your request.

### **filters**

The `filters` key is the main place where you refine your request. It's an object that can take several different keys:

* **category:**

  The `category` param is an object with the id of the category. You can find a dictionnary of these category above. This will let you precise the category you're willinng to do the search.

  Ex:

  ```json
  "category": {
    "id": "39"
  }
  ```

  *Note: you cannot search for various categories at the same time*


* **keywords:**

  The `keywords` key lets you filter your search according to some... well, keywords.

  Here's how it looks like:

  ```json
  "keywords": {
    "text": "your awesome keywords"
    },
  ```

  By default, it will search both in the title of an ad and its description. You can decide to search only in the title of the ad by adding a key `type` of value `subject` as follows:

  ```json
  "keywords": {
    "type": "subject",
    "text": "le rouge"
  }
  ```

* **price**

  You can also filter your search by specifying a price range, a min and/or a max. It's as simple as:

  With a price range:

  ```json
    "price": {
      "min": "15",
      "max": "50"
    }
  ```

  With a price minimum only:

  ```json
    "price": {
      "min": "15"
    }
  ```

  With a price maximum only:

  ```json
    "price": {
      "max": "50"
    }
  ```


* **location:**

  You can filter your search by location. There are two ways of doing so. The first one is by giving latitudes, longitudes and a radius keys (in meters), while the second one let's you do it by giving a French region or department.

  With coordinates:

    ```json
    "location": {
      "area": {
        "lat": "48.8729527",
        "lng": "2.3573570999999998",
        "radius": "30000"
      }
    }
    ```

  With region or department:

    ```json
    "location": {
      "region": "12",
      "department": "75"
    }
    ```

    *Note: you cann find a dictionnary of the regions below*

    *Note 2: you cannot cross several regions or department at the same time:*
      ```
        "location": {
          "region": ["12", "14"],
          "department": ["75", "23"]
        }
        ``` wont work.

* **urgent:**

  You can request only ads that are labeled "urgent" on leboncoin. Users need to pay a small fee to be able to do so.

  To actually do it, you'll need to add an `enums` key with urgent key nested inside, as follows:

  ```json
  "enums": {
    "urgent": ["1"]
  }
  ```


### **limit**

You can precise a limit in the number of items you want to receive.

### **sort_by**

You can sort your result by date, and price. Here's how we do it:

```json
  "sort_by": "time"
```

```json
  "sort_by": "price"
```

### **sort_order**

When you sort your results, you can specify in you want the more recent (or the older) ads as well as from the cheapest or the most expensive ones:

From the highest value to the lowest:

```json
  "sort_order": "desc"
```


From the lowest value to the highest:

```json
  "sort_order": "asc"
```

### **owner_type**

The `owner_type` key lets you filter the ads depending on the type of user who posted it: a professional vs. a private person. Here's how it works:

To get ads posted by pros, simply add:

```json
"owner_type": "pro"
```

To get ads posted by private persons, simply add:

```json
"owner_type": "private"
```

## Samples

TO DO

## Final Notes

  Keep in mind that the data you'll get are real ones, despite they're anonymised (aka: no phone number nor email adresses).

  These data are here for you to be able to build something around it, to tweak them or to simply let you use real data for your demo.

  Please, be kind, be bold and have fun hacking!