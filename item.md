**Endpoint: /api/v1/inventory/item**


[Get](#GET)

[Create or Update](#UPDATE)

[Delete](#DELETE)
  
Items are basic building block of inventory API. They can either be used to track amounts of products themselves, or be used as components for [recipes - temporary link](recipe.md) (something something build complex something something). Along with items, it's important to check [inventory](inventory.md) endpoint for more information about tracking amounts of items and [card](card.md) endpoint for more information about manipulating amounts.

### Get ###

**Request data**

| field name   |       type       | Description                                             |
| :----------- | :--------------: | :------------------------------------------------------ |
| itemIds      | array of strings | Ids of requested stock items. Values are java UUID type |
| productCodes | array of strings | Product codes of requested items                        |
| groupIds     | array of number  | Group ids whose items should be returned                |

For getting stock items from API you have these options.

* Sending empty ```data``` field will make API return all available non-deleted stock items.
* Sending ```itemIds``` will make API return any stock items whose ```id``` matches requested ```itemIds```
* Sending ```productCodes``` will make API return any stock items whose ```productCode``` matches requested ```productCodes```
* Sending ```groupIds``` will make API return groups whose ```id``` matches requested ```groupIds``` and all their items.
* Sending combination of ```ids```, ```productCodes``` or ```groupIds``` will make API return items that match either of requested data. If there are items that match multiple requested fields they will be returned only once.



**Request Examples**

For all items: 

```json 

{
  "action": "GET",
  "data": {
  }
}

```

For request where only items that match ```ids``` are returned:

```json
{
  "action": "GET",
  "data": {
    "itemIds": [1, 2, 3],
  }
}
```

For request where only items that match ```product codes``` are returned: 

```json


{
  "action": "GET",
  "data": {
    "productCodes": ["123", "5555"]
  }
}
```

```json

{
  "action": "GET",
  "data": {
    "groupIds": [1, 2]
  }
}
```

Combined request, getting items that match ```itemIds``` **or** ```productCodes``` **or** are in groups with ```groupIds```.

```json 
{
  "action": "GET",
  "data": {
    "itemIds": ["27fd9916-d6ee-43d7-a3a4-0946f41b3c0f", "27fd9916-aaaa-43d7-a3a4-0946f41b3c0f", "27fd9916-bbbb-43d7-a3a4-0946f41b3c0f", ""27fd9916-cccc-43d7-a3a4-0946f41b3c0f""],
    "productCodes": ["123", "5555"],
    "groupIds": [1, 2]
  }
}

```



**Response**

Response contains array of group objects with their items. For the detailed definition of objects refer to the [Object documentation](storehouse%20objects.md) in wiki. 

> Stock item `amount` will not be set in response. If you want to know amount, use request on [/inventory](inventory.md) endpoint.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                      type                      | Description             |
| :--------- | :--------------------------------------------: | :---------------------- |
| groups     | array of json objects of type Stock Item Group | Groups with stock items |

**Response Example**

```json

{
    "data": {
        "groups": [
            {
                "id": 1,
                "name": "Fruit",
                "items": [
                    {
                        "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
                        "externalId": "ext 123"
                        "measuringUnit": "kg",
                        "minimalStockAmount": 3,
                        "priceFixed": 5,
                        "priceNetAverage": 2.5,
                        "productCode": "12345",
                        "secondaryMeasuringUnit": "g",
                        "title": "Apple",
                        "unitCoefficient": 1000,
                        "vatRate": 20
                    }
                ]
            }
        ]
    },
    "success": true
}
```

### Update ###

Creating and updating items and groups can be done simultaneously in one request. Depending on data provided in request API will accordingly create or update items and groups.

* Sending groups **without** `id` field. API will create new groups and generate `id` for them.
* Sending items **without** `id` field set. API will create new items and generate `id` for them.
* Sending groups **with** `id` field. 
    * In case groups do not exist, API will create new groups with id provided in request.
    * In case groups already exist, their name will be updated if it was provided.
* Sending items **with** `id` field set. 
    * In case item does not exist, API will use provided `id` to create items 
    * In case item already exists it will be updated.


> It is possible to combine both creation methods in one request. Sending both items/groups with and without ids.
>
> All fields both in group and item objects are optional.
>
> `Amount` and `priceNetAverage` fields cannot be set by create or update because they are calculated from different source. For more information refer to [card](card.md) endpoint. 
>
> When updating existing item all its fields must be set.
>
> To move existing item to different group send update request with item in desired group.
>
> Item is always in exactly one group.
>
> Alcohol group name cannot be changed.

**Request data**

| field name |               type                | Description                  |
| :--------- | :-------------------------------: | :--------------------------- |
| groups     | array of stock item group objects | stock item groups with items |

**Request Example**


```json
{
  "action": "UPDATE",
  "data": {
    "groups": [
      {
        "id": 1,
        "name": "Fruit",
        "type": "OTHER",
        "items": [
          {
            "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
            "externalId": "ext 123"
            "title": "Apple",
            "priceFixed": 5,
            "vatRate": 20,
            "measuringUnit": "kg",
            "secondaryMeasuringUnit": "g",
            "unitCoefficient": 1000,
            "productCode": "12345",
            "minimalStockAmount": 3
          }
        ]
      }
    ]
  }
}

```


**Response**

Response contains array of group objects with their items. For the detailed definition of objects refer to the [Object documentation](storehouse%20objects.md) in wiki. 

> Stock item `amount` will not be set in response. If you want to know amount, use request on [/inventory](inventory.md) endpoint.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

| field name |                      type                      | Description                                   |
| :--------- | :--------------------------------------------: | :-------------------------------------------- |
| groups     | array of json objects of type Stock Item Group | Groups and items that were created or updated |

**Response Example**

```json
{
    "data": {
        "groups": [
            {
                "id": 1,
                "name": "Fruit",
                "items": [
                    {
                        "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
                        "externalId": "ext 123"
                        "measuringUnit": "kg",
                        "minimalStockAmount": 3,
                        "priceFixed": 5,
                        "priceNetAverage": 0,
                        "productCode": "12345",
                        "secondaryMeasuringUnit": "g",
                        "title": "Apple",
                        "unitCoefficient": 1000,
                        "vatRate": 20
                    }
                ]
            }
        ]
    },
    "success": true
}

```

### Delete

Currently it is possible to delete individual items or delete whole group along with its items.

* Sending `itemIds` will delete items whose `id` matches `itemIds`.
* Sending `groupIds` will delete groups whose `id` matches `groupIds` and all their items.
* Sending combination of `itemIds` and `groupIds` will delete groups whose `id` matches `groupIds`, their items, and items whose `id` matches `itemIds`.

>

**Request data**

| field name |       type       | Description                          |
| :--------- | :--------------: | :----------------------------------- |
| itemIds    | array of numbers | ids of items that are to be deleted. |
| groupIds   | array of numbers | ids of groups that should be deleted |

**Request Example**

Request for deletion using `itemIds`.

```json
{
  "action": "DELETE",
  "data": {
    "itemIds": ["27fd9916-d6ee-43d7-a3a4-0946f41b3c0f", "27fd9916-aaaa-43d7-a3a4-0946f41b3c0f", "27fd9916-bbbb-43d7-a3a4-0946f41b3c0f", ""27fd9916-cccc-43d7-a3a4-0946f41b3c0f""],
  }
}
```
Request for deletion using `groupIds`.

```json
{
  "action": "DELETE",
  "data": {
    "groupIds": [1, 2]
  }
}
```

Combined request deletion using both `groupIds` and `itemIds`.

```json
{
  "action": "DELETE",
  "data": {
    "itemIds": ["27fd9916-d6ee-43d7-a3a4-0946f41b3c0f", "27fd9916-aaaa-43d7-a3a4-0946f41b3c0f", "27fd9916-bbbb-43d7-a3a4-0946f41b3c0f", ""27fd9916-cccc-43d7-a3a4-0946f41b3c0f""],
    "groupIds": [1, 2]
  }
}
```
**Response**

Successful delete request will return array of groups with items. If there is any problem and response success field is false, then no items were deleted. For the detailed definition of objects refer to the [Object documentation](storehouse%20objects.md) in wiki. 

> Deleted items are always returned in their `groups` even if group was not deleted. This is for consistency with other responses and informative purposes. To delete group you must provide it's `id` in `groupIds`.

> Stock item `amount` will not be set in response. If you want to know amount, use request on [/inventory](inventory.md) endpoint.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                      type                      | Description              |
| :--------- | :--------------------------------------------: | :----------------------- |
| groups     | array of json objects of type Stock Item Group | Deleted groups and items |

**Response Example**


```json

{
    "data": {
        "groups":
            "id": 1,
            "name": "Fruid"
            "items": [
                {
                    "id": "27fd9916-d6ee-43d7-a3a4-0946f41b3c0f",
                    "measuringUnit": "kg",
                    "minimalStockAmount": 3,
                    "priceFixed": 5,
                    "priceNetAverage": 0,
                    "productCode": "12345",
                    "secondaryMeasuringUnit": "g",
                    "title": "Apple",
                    "unitCoefficient": 1000,
                    "vatRate": 20
                }
            ]
        },
    "success": true
}


```
