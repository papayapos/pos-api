**Endpoint Summary**




| Endpoint                                          | Description                                     |
| ------------------------------------------------- | ----------------------------------------------- |
| [/api/v1/inventory](#/api/v1/inventory)           | CRUD operations for inventories                 |
| [/api/v1/inventory/card](#/api/v1/inventory/card) | Receive or issue the stock with inventory cards |
| [/api/v1/inventory/item](#/api/v1/inventory/item) | Work with stock items                           |
| /api/v1/inventory/recipe                          | Create and assign recipes to menu items         |
| /api/v1/inventory/movement (todo define)          | Read inventory movements not grouped in cards   |
| [/api/v1/supplier](#/api/v1/supplier)             | CRUD for supplier                               |
| /api/v1/order                                     | Create order to POS                             |
| /api/v1/                                          |                                                 |



## /api/v1/inventory

**Request**

Fetch storehouse and its stock items

**Get**

| field name |  type  | Description                 |
| :--------- | :----: | :-------------------------- |
| name       | string | name of storehouse to fetch |

Empty request will return error.

**Request Example**


```json

{
  "action": "GET",
  "data": {
    "ids": [1, 2, 3]
  }
}
```

**Response**
Response contains storehouse object with current state of stock items in it, stock items are stored in stock item group objects. For the definition of objects refer to the Object documentation in wiki.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |       type        | Description                                                  |
| :--------- | :---------------: | :----------------------------------------------------------- |
| storehouse | storehouse object | storehouse object that matches requested name. Field will not be set if no object matches name. |

**Response Example**

```json

{
    "data": {
        "storehouses": [
            {
                "description": "This is description",
                "groups": [
                    {
                        "id": 1,
                        "items": [
                            {
                                "amount": 0,
                                "id": 35,
                                "measuringUnit": "L",
                                "minimalStockAmount": 0,
                                "priceFixed": 0,
                                "priceNetAverage": 0,
                                "secondaryMeasuringUnit": "cl",
                                "title": "Vodka Nicolaus Liter",
                                "unitCoefficient": 100,
                                "vatRate": 20
                            },
                            {
                                "amount": -0.08,
                                "id": 37,
                                "measuringUnit": "L",
                                "minimalStockAmount": 0,
                                "priceFixed": 0,
                                "priceNetAverage": 0,
                                "secondaryMeasuringUnit": "cl",
                                "title": "Fernet Stock 38% liter",
                                "unitCoefficient": 100,
                                "vatRate": 20
                            }
                        ],
                        "name": "Alkohol",
                        "type": "ALCOHOL"
                    },
                    {
                        "id": 2,
                        "items": [
                            {
                                "amount": 102,
                                "id": 34,
                                "measuringUnit": "ks",
                                "minimalStockAmount": 10,
                                "priceFixed": 0,
                                "priceNetAverage": 0.5,
                                "secondaryMeasuringUnit": "ks",
                                "title": "Coca Cola 0,5L",
                                "unitCoefficient": 1,
                                "vatRate": 20
                            },
                            {
                                "amount": -0.3,
                                "id": 36,
                                "measuringUnit": "L",
                                "minimalStockAmount": 0,
                                "priceFixed": 0,
                                "priceNetAverage": 0,
                                "secondaryMeasuringUnit": "cl",
                                "title": "1724 Tonic Vantguard Liter",
                                "unitCoefficient": 100,
                                "vatRate": 20
                            }
                        ],
                        "name": "Nealko",
                        "type": "OTHER"
                    },
                    {
                        "id": 3,
                        "items": [
                            {
                                "amount": 99,
                                "id": 1,
                                "measuringUnit": "kg",
                                "minimalStockAmount": 0,
                                "priceFixed": 1,
                                "priceNetAverage": 2.0,
                                "secondaryMeasuringUnit": "g",
                                "title": "Kuracie mäso kg",
                                "unitCoefficient": 1000,
                                "vatRate": 20
                            },
                            {
                                "amount": 103,
                                "id": 33,
                                "measuringUnit": "kg",
                                "minimalStockAmount": 0,
                                "priceFixed": 0,
                                "priceNetAverage": 0.5,
                                "secondaryMeasuringUnit": "g",
                                "title": "Zemiaky kg",
                                "unitCoefficient": 1000,
                                "vatRate": 20
                            }
                        ],
                        "name": "Material",
                        "type": "OTHER"
                    }
                ],
                "id": 1,
                "name": "Storehouse"
            }
        ]
    },
    "success": true
}

```

**/api/v1/storehouse**

**Request**

Create or update stock items and store houses

**Update/Create**

| field name  |            type             | Description                                                  |
| :---------- | :-------------------------: | :----------------------------------------------------------- |
| storehouses | array of storehouse objects | Each contains array of stock item groups and items that belong to it. Store houses and stock items to create or update |

If id field in any of storehouses is not provided, API will return error 2.

**Request Example**

```json
{
  "action": "UPDATE",
  "data": {
    "storehouses": [
      {
        "id": 1,
        "name": "New storehouse name",
        "description": "New description"
      }
    ]
  }
}
```


**Response**
Response contains array of store houses with all their stock item objects (not just ones that were updated or created).

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:
Data object in response is empty.

**Response Example**

```json
{
    "data": {},
    "success": true
}


{
    "error": "2: Field value required: storehouse.id",
    "success": false
}


```

**Method: DELETE**

**/api/v1/storehouse**

**Request**

It is not allowed to delete default storehouse, with id = 1. Sending 1 in *ids* field will cause API to return error 4000. Ids that are not in database will be ignored and response will contain only deleted storehouses.

**DELETE request**

| field name |       type       | Description                                |
| :--------- | :--------------: | :----------------------------------------- |
| ids        | array of numbers | ids of storehouses that are to be deleted. |

**Request Example**

```json
{
  "action": "DELETE",
  "data": {
    "ids": [2, 3, 4]
  }
}
```
**Response**

Successful delete request will return array of order objects that were deleted. If there is any problem and response success field is false, then no orders were deleted.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name  |                   type                   | Description                                                  |
| :---------- | :--------------------------------------: | :----------------------------------------------------------- |
| storehouses | array of json objects of type storehouse | Deleted storehouses. Note that field *groups* will not be set even if storehouse is not empty. |

**Response Example**


```json

{
  "success": true,
  "data" {
    "storehouses": [
      {
        "id": 2,
        "name": "Storehouse 2",
        "description": "Random items"
      },
      {
        "id": 3,
        "name": "Pet items",
        "description": "Pet food and such"
      },
    ]
  }
}


{
    "error": "4000: Default storehouse cannot be deleted",
    "success": false
}
```



## /api/v1/inventory/card

**Request**

As list of stock change cards can be extremely big and grows with time, there is no option to retrieve everything. In future we may add some limitations for now though there are none. 

**Get**

It is required that either *ids* or *from* and *to* fields are set. If you are selecting by *ids* field you cannot use other fields, they will be ignored. If you are selecting by date range you should not set *ids*. 

If you request ids that are not in database they will be ignored and only objects found will be returned.

Option 1: ids, other fields are ignored

| field name |       type       | Description                               |
| :--------- | :--------------: | :---------------------------------------- |
| ids        | array of numbers | long, id of stock taking card in database |

Option 2: from, to range, be sure to not set ids field

| field name   |  type  | Description                                                |
| :----------- | :----: | :--------------------------------------------------------- |
| from         | string | start of interval that is to be retrieved                  |
| to           | string | optional, end of interval that is to be retrieved          |
| storehouseId | number | long, optional                                             |
| documentType | number | long, optional, 1 = fiscal receipt, 2 = invoice, 3 = other |
| supplierName | string | optional, name of supplier                                 |


**Request Example**


```json
{
  "action": "GET",
  "data": {
    "ids": [1, 2, 3, 999]
  }
}
```

```json
{
  "action": "GET",
  "data": {
    "from": "11.03.2020 01:01:03",
    "to": "11.03.2022 01:01:03"
  }
}
```

**Response**
Response contains array of recipe objects.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                 type                 | Description                                                  |
| :--------- | :----------------------------------: | :----------------------------------------------------------- |
| cards      | array of json objects of type recipe | List of requested recipes. If no recipes were found this object will contain empty array. |

**Response Example**


```json

{
    "data": {
        "cards": [
            {
                "createTime": "11.03.2021 11:04:30",
                "documentType": 1,
                "id": 1,
                "items": [
                    {
                        "accountingEntryId": "4812736b-8462-48c9-b8ba-51bf0908bd51",
                        "amount": 1,
                        "id": 1,
                        "measuringUnit": "ks",
                        "priceNet": 0,
                        "stockChangeCardId": 1,
                        "stockItemId": 1,
                        "stockItemTitle": "Kuracie mäso",
                        "vatRate": 20
                    }
                ],
                "placedTime": "11.03.2021 11:04:29",
                "storehouseId": 1,
                "supplier": {}
            },
            {
                "createTime": "11.03.2021 11:05:14",
                "documentType": 1,
                "id": 2,
                "items": [
                    {
                        "accountingEntryId": "555d9243-2285-4560-b66d-ffe931c9ddcf",
                        "amount": 1,
                        "id": 2,
                        "measuringUnit": "ks",
                        "priceNet": 0,
                        "stockChangeCardId": 2,
                        "stockItemId": 1,
                        "stockItemTitle": "Kuracie mäso",
                        "vatRate": 20
                    }
                ],
                "placedTime": "11.03.2021 11:05:13",
                "storehouseId": 1,
                "supplier": {}
            },
            {
                "createTime": "11.03.2021 14:04:49",
                "id": 3,
                "items": [
                    {
                        "amount": 100,
                        "id": 3,
                        "measuringUnit": "ks",
                        "priceNet": 0.5,
                        "stockChangeCardId": 3,
                        "stockItemId": 34,
                        "stockItemTitle": "Coca Cola 0,5L",
                        "vatRate": 20
                    },
                    {
                        "amount": 100,
                        "id": 4,
                        "measuringUnit": "kg",
                        "priceNet": 2,
                        "stockChangeCardId": 3,
                        "stockItemId": 1,
                        "stockItemTitle": "Kuracie mäso kg",
                        "vatRate": 20
                    },
                    {
                        "amount": 100,
                        "id": 5,
                        "measuringUnit": "kg",
                        "priceNet": 0.5,
                        "stockChangeCardId": 3,
                        "stockItemId": 33,
                        "stockItemTitle": "Zemiaky kg",
                        "vatRate": 20
                    }
                ],
                "placedTime": "11.03.2021 14:04:03",
                "storehouseId": 1,
                "supplier": {}
            }
        ]
    },
    "success": true
}



```
**Endpoint: /api/v1/inventory/card**

**Request**

Create or update stock change cards.

Sending empty request will cause API to return error 1.

Sending objects without their id will cause API to return error 2.

**Update/Create**

| field name |               type                | Description                  |
| :--------- | :-------------------------------: | :--------------------------- |
| cards      | array of stock item group objects | stock item groups with items |

**Stock change card** 

| field name   |      type       | Description                                                  |
| :----------- | :-------------: | :----------------------------------------------------------- |
| id           |     number      | long, id of card in database                                 |
| storehouseId |     number      | long, storehouse id                                          |
| createTime   |     string      | create time,                                                 |
| placedTime   |     string      | placed time, plays role in stock taking process which is currently not part of API, use same time as create time for now |
| accType      |     number      | type of stock change card, currently 1 = Operating costs, 2 = SALE, 3 = Refund of operating costs, 4 = Refund of sale, 5 = Stock taking receipt, 6 = Stock taking invoice, stock taking is currently not part of API |
| documentType |     number      | type of bill associented with card, 1 = Fiscal receipt, 2 = invoice, 3 = other |
| documentId   |     string      | id of document associated with card, for example accounting transaction |
| type         |     string      | type of change, RECEIPT_CARD = when goods are added to storehouse, ISSUE_CARD = when goods are removed from storehouse |
| supplier     | supplier object | supplier                                                     |
| orderNumber  |     string      | order number                                                 |
| deliveryNote |     string      | delivery note                                                |
| note         |     string      | another note                                                 |


**Request Example**


```json
{ 
  "action": "UPDATE",
  "data": {
    "cards": [
      {
        "id": 50,
        "storehouseId": 1,
        "createTime": "17.02.2022 14:04:03",
        "placedTime": "17.02.2022 14:04:03",
        "type": "RECEIPT_CARD",
        "documentId": "1234",
        "documentType": 1,
        "supplier": {
        },
        "orderNumber": "12",
        "deliveryNote": "delivery note",
        "note": "Note",
        "items": [
          {
            "amount": 100,
            "id": 70,
            "measuringUnit": "ks",
            "priceNet": 0.5,
            "stockChangeCardId": 3,
            "stockItemId": 50,
            "stockItemTitle": "Custom title",
            "vatRate": 20
          }
        ]
      }
      ]
  }
}
```


**Response**

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

Data field will be empty in response.

**Response Example**

```json
{
    "data": {},
    "success": true
}


```

**Method: DELETE**

**Endpoint: /api/v1/inventory/card**

**Request**

**DELETE request**

Sending empty request will result in API returning error 1.

| field name |      type       | Description                                             |
| :--------- | :-------------: | :------------------------------------------------------ |
| ids        | list of numbers | long, ids of stock change cards that are to be deleted. |

**Request Example**

```json
{
  "action": "DELETE",
  "data": {
    "ids": [1, 2, 3]
  }
}
```
**Response**

Successful delete request will return array of order objects that were deleted. If there is any problem and response success field is false, then no orders were deleted.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name   |                      type                       | Description                 |
| :----------- | :---------------------------------------------: | :-------------------------- |
| deletedCards | array of json objects of type stock change card | Deleted stock change cards. |

**Response Example**


```json

{
    "data": {
        "cards": [
            {
                "createTime": "11.03.2021 11:04:30",
                "documentType": 1,
                "id": 1,
                "items": [
                    {
                        "accountingEntryId": "4812736b-8462-48c9-b8ba-51bf0908bd51",
                        "amount": 1,
                        "id": 1,
                        "measuringUnit": "ks",
                        "priceNet": 0,
                        "stockChangeCardId": 1,
                        "stockItemId": 1,
                        "stockItemTitle": "Kuracie mäso",
                        "vatRate": 20
                    }
                ],
                "placedTime": "11.03.2021 11:04:29",
                "storehouseId": 1,
                "supplier": {}
            },
            {
                "createTime": "11.03.2021 11:05:14",
                "documentType": 1,
                "id": 2,
                "items": [
                    {
                        "accountingEntryId": "555d9243-2285-4560-b66d-ffe931c9ddcf",
                        "amount": 1,
                        "id": 2,
                        "measuringUnit": "ks",
                        "priceNet": 0,
                        "stockChangeCardId": 2,
                        "stockItemId": 1,
                        "stockItemTitle": "Kuracie mäso",
                        "vatRate": 20
                    }
                ],
                "placedTime": "11.03.2021 11:05:13",
                "storehouseId": 1,
                "supplier": {}
            },
            {
                "createTime": "11.03.2021 14:04:49",
                "id": 3,
                "items": [
                    {
                        "amount": 100,
                        "id": 3,
                        "measuringUnit": "ks",
                        "priceNet": 0.5,
                        "stockChangeCardId": 3,
                        "stockItemId": 34,
                        "stockItemTitle": "Coca Cola 0,5L",
                        "vatRate": 20
                    },
                    {
                        "amount": 100,
                        "id": 4,
                        "measuringUnit": "kg",
                        "priceNet": 2,
                        "stockChangeCardId": 3,
                        "stockItemId": 1,
                        "stockItemTitle": "Kuracie mäso kg",
                        "vatRate": 20
                    },
                    {
                        "amount": 100,
                        "id": 5,
                        "measuringUnit": "kg",
                        "priceNet": 0.5,
                        "stockChangeCardId": 3,
                        "stockItemId": 33,
                        "stockItemTitle": "Zemiaky kg",
                        "vatRate": 20
                    }
                ],
                "placedTime": "11.03.2021 14:04:03",
                "storehouseId": 1,
                "supplier": {}
            }
        ]
    },
    "success": true
}
```

## /api/v1/inventory/item

**Request**

Fetch specific stock items or all stock items.

**Get**

| field name   |       type       | Description                                             |
| :----------- | :--------------: | :------------------------------------------------------ |
| ids          | array of numbers | Ids of requested stock items. Values are java long type |
| productCodes | array of strings | Product codes of requested items                        |

Sending empty request will return all stock items. 

**Request Example**


```json
{
  "action": "GET",
  "data": {
    "ids": [1, 2, 3],
    "productCodes": ["123", "5555"]
  }
}
```

**Response**
Response contains array of stock item objects. For the definition of stock item object refer to the Object documentation in wiki.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                   type                   | Description                                                  |
| :--------- | :--------------------------------------: | :----------------------------------------------------------- |
| items      | array of json objects of type Stock Item | List of stock items that match requested ids. If no objects match requested ids then this array will be empty. |

**Response Example**
TODO

**Endpoint: /api/v1/inventory/item**

**Request**

Create or update stock items. Note that when creating amount field can be used but when updating amount field is ignored.

Note that groups of type ALCOHOL cannot be updated or deleted.

Sending empty request will cause API to return error 1.

Sending objects without their id will cause API to return error 2.

**Update/Create**

| field name |               type                | Description                  |
| :--------- | :-------------------------------: | :--------------------------- |
| groups     | array of stock item group objects | stock item groups with items |

Stock Item Group

name field is required. If the name field is not provided request will still be processed as valid but name will be set to null.

It is not possible to type of stock item group once it's created.

It is not possible to update name of ALCOHOL type group.

It is currently not possible to delete ALCOHOL type group.(this may be bug)

Stock Item

priceNetAverage and amount fields can be used during create to set initial values but cannot be updated later on and papaya will update then when inventory changes happen. As such if you provide them to update request they will be ignored.

If priceNetAverage is not provided when creating new stock item, it will be set to zero.

If amount is not provided when creating new stock item, it will be set to zero.

All fields except priceNetAverage and amount are required. Although you can leave them out, but values will be set to null if you do, with some exceptions.

priceFixed does not need to be provided, API will use old value if that is the case.

If secondaryMeasuringUnit is not provided, API will use measuringUnit as the value.

If minimalStockAmount is not provided, API will set it to zero.

If unitCoefficient is not provided, API will set it to one.

**Request Example**


```json
{
  "action": "UPDATE",
  "data": {
    "groups": [
      {
        "id": 50,
        "name": "Toys",
        "type": "OTHER",
        "items": [
          {
            "id": 50,
            "title": "Item",
            "amount": 2,
            "priceNetAverage": 2.5,
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

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

Data field will be empty in response.

**Response Example**

```json
{
    "data": {},
    "success": true
}


```

**Method: DELETE**

**Endpoint: /api/v1/inventory/item**

**Request**

Delete stock items.

**DELETE request**

| field name |       type       | Description                                |
| :--------- | :--------------: | :----------------------------------------- |
| ids        | array of numbers | ids of stock items that are to be deleted. |

**Request Example**

```json
{
  "action": "DELETE",
  "data": {
    "ids": [2, 3, 4]
  }
}
```
**Response**

Successful delete request will return array of order objects that were deleted. If there is any problem and response success field is false, then no items were deleted.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                   type                   | Description          |
| :--------- | :--------------------------------------: | :------------------- |
| items      | array of json objects of type stock item | Deleted stock items. |

**Response Example**


```json

{
    "data": {
        "items": [
            {
                "amount": 2,
                "id": 2,
                "measuringUnit": "kg",
                "minimalStockAmount": 3,
                "priceFixed": 5,
                "priceNetAverage": 2.500001,
                "productCode": "12345",
                "secondaryMeasuringUnit": "g",
                "title": "Item",
                "unitCoefficient": 1000,
                "vatRate": 20
            }
        ]
    },
    "success": true
}
```

## /api/v1/supplier

**Request**

**Get**

Send empty data field for all suppliers or array of numbers for specific ids.

| field name |            type            | Description               |
| :--------- | :------------------------: | :------------------------ |
| ids        | optional, array of numbers | Long, requested suppliers |

Sending empty request will return all recipes

**Request Example**


```json
{
  "action": "GET",
  "data": {
    "ids": [1]
  }
}
```

**Response**
Response contains array of suppliers, either all of them or ones that exist and match ids.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                  type                  | Description                 |
| :--------- | :------------------------------------: | :-------------------------- |
| suppliers  | array of json objects of type supplier | List of requested suppliers |

**Response Example**


```json

{
    "data": {
        "suppliers": [
            {
                "address": "b",
                "city": "c",
                "contactPerson": "g",
                "country": "d",
                "email": "i",
                "icDph": "f",
                "ico": "e",
                "id": 1,
                "name": "a",
                "phone": "h",
                "registration": true,
                "registrationDate": "12.05.2022 00:00:00",
                "registrationNumber": "j"
            }
        ]
    },
    "success": true
}



```

**Request**

Create or update suppliers

**Update/Create**

| field name |           type            | Description          |
| :--------- | :-----------------------: | :------------------- |
| suppliers  | array of supplier objects | ID field is required |

If id field in any of suppliers is not provided, API will return error 2.

**Request Example**

```json
{
  "action":"UPDATE",
  "data":{
    "suppliers": [
        {
          "address": "b",
          "city": "c",
          "contactPerson": "g",
          "country": "d",
          "email": "i",
          "icDph": "f",
          "ico": "e",
          "id": 1,
          "name": "a",
          "phone": "h",
          "registration": true,
          "registrationDate": "12.05.2022 00:00:00",
          "registrationNumber": "j"
        }
    ]
  }
}
```


**Response**
Response contains array of created/updated supplier objects with new values.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:
Data object in response is empty.

**Response Example**

```json
{
    "data": {
        "suppliers": [
            {
                "address": "b",
                "city": "c",
                "contactPerson": "g",
                "country": "d",
                "email": "i",
                "icDph": "f",
                "ico": "e",
                "id": 3,
                "name": "a",
                "phone": "h",
                "registration": true,
                "registrationDate": "12.05.2022 00:00:00",
                "registrationNumber": "j"
            }
        ]
    },
    "success": true
}



{
    "error": "2: Field value required: id",
    "success": false
}
```
