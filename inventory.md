**Endpoint: /api/v1/inventory**

[Get](GET)

[Create or Update](#UPDATE)

[Delete](#DELETE)

Inventories allow users to track amounts of [items](item.md). Furthermore, users can create multiple inventories and track amount of items in each of them separately. Amount of item in each inventory is calculated from [cards](card.md), initially starting on zero. 

### GET ###

For getting current state of inventories we have multiple options, from getting everything as well as allowing you to select specific inventories and items.

* Sending empty `data` field will make API return all inventories with all items.
* Sending array of query objects.
    * Allows you to specify which inventories you want to get by their `id`.
    * Allows you to specify which items you want to get by their `id`.
    * In case you do not specify items, all items will be get for that inventory.

> In case you send non-existent item or inventory ids, they will be ignored and only existing ones will be fetched.

**Request data**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| inventories  | array of inventory query objects | Each object contains inventory id and may contain item ids. |

**Inventory query**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| inventoryId  | number | id of inventory to fetch |
| itemIds| array of numbers | Long, ids of items to fetch for inventory | 

> `inventoryId` required
> 
> `itemIds` are optional and can be used to limit what should be fetched from API. In case you only want specific items, this field can be used to reduce calculation time as well as payload of response.

**Request Examples**

For getting all inventories with their items, you can send empty data.

```json
{
  "action": "GET",
  "data": {
  }
}
```

For specific inventory with all items in it, you can send inventory query with just `inventoryId`. You can of course send multiple queries in single request. For simplicity of example there is only one query in example.

```json
{
  "action": "GET",
  "data": {
    "inventories": [
      {
        "inventoryId": 1
      }
    ]     
  }
}
```

For cases when we want specific inventory but do not require all items we can specify which items we want to get as seen below, which requests state of items with `id` 1,2 and 3 in inventory with `id` 2. You can of course send multiple queries in single request. For simplicity of example there is only one query in example.

```json
{
  "action": "GET",
  "data": {
    "inventories": [
       {
           "inventoryId": 2,
           "itemIds": [1,2,3]
       }
    ]
  }
}
```

Of course it is also possible to combine both types of queries. In example below, first query requests all items for inventory with `id` 1, and items with `id` 1, 2 and 3 for inventory with `id` 2.

```json
{
  "action": "GET",
  "data": {
    "inventories": [
       {
           "inventoryId": 1
       },
       {
          "inventoryId": 2,
          "itemIds": [1,2,3]
       }
    ]
  }
}
```

**Response**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
|data| json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
|success | boolean | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error | string | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

Response contains inventory objects with current state of items in it, items are stored in group objects. For the definition of objects refer to the [Object documentation](storehouse%20objects.md).

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| inventories | array of inventory objects | Inventories that match requested ids. Field will contain empty array if there are no matches.

**Response Example**

```json

{
    "data": {
        "inventories": [
            {
                "id": 1,
                "name": "Main inventory"
                "description": "Description of main inventory",
                "groups": [
                    {
                        "id": 1,
                        "name": "Fruit",
                        "items": [
                            {
                                "id": 1,
                                "externalId": "1234567879",
                                "title": "Apple",
                                "productCode": "12345",
                                "amount": 10,
                                "minimalStockAmount": 3,
                                "measuringUnit": "kg",
                                "secondaryMeasuringUnit": "g",
                                "unitCoefficient": 1000,
                                "priceFixed": 5,
                                "priceNetAverage": 5,
                                "vatRate": 20
                            }
                        ]
                    }

                ]
            }
        ]
    },
    "success": true
}

```

### UPDATE ###

For creating inventories you have these options.

* Sending inventory **without** `id`. API will create new inventory and generate `id` for it.
* Sending inventory **with** `id`.
    * In case inventory with such `id` already exists, it will be updated.
    * In case inventory does not exist, API will create new inventory and use provided `id`.

> You can send empty inventory object and you will get back inventory without `name` or `description` with only `id`.
>
> Updating deleted inventory will restore it, including cards.

You might expect that updating amounts would also be part of inventory api. That is not the case as there is a lot of information that Papaya allows you to record in inventory movements. If you want to update amounts or average price of items, you should check [card](card.md) endpoint. As for updating other item data you should check [item](item.md) endpoint.

**Request data**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| inventories  | array of inventory objects | Inventories to create or update |

> `groups` field in inventory object is only relevant for [get](#GET) and [delete](#DELETE) requests and will be ignored in update requests.

If you provide inventory without *id*, API will return error 2.

**Request Example**
For simplicity of example, there is only one inventory in array. Of course you can send multiple object in one request.

```json
{
  "action": "UPDATE",
  "data": {
    "inventories": [
      {
        "id": 1,
        "name": "Name of inventory",
        "description": "Description of this inventory"
      }
    ]
  }
}
```

**Response**

Successful update request will return array of inventory objects that were created or updated. If there is any problem and response `success` field is false, then there will be no changes to data and you need to resend request.

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
|data| json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
|success | boolean | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error | string | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

Data contains array of inventory objects. For more detailed definition of objects refer to [Object documentation](storehouse%20objects) in wiki.

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| inventores | array of inventory objects | Created or updated inventories |

**Response Example**

```json
{
    "data": {
        "inventories": [
            {
              "id": 1,
              "name": "Name of inventory",
              "description": "Description of this inventory"
            }
        ]
    },
    "success": true
}

```
### DELETE ###

For deleting inventories, you need to send their `id` in request. 

API keeps track of deleted inventories. In case you want to restore them, you can send update request with that inventory `id`.

> Non-existent `ids` will be ignored and response will contain only deleted inventories.
>
> It is not possible to delete inventory with `id` 1. Attempting to do it will return error.

**Request data**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| ids | array of numbers | ids of inventories to be deleted.|

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

Successful delete request will return array of inventory objects that were deleted. If there is any problem and response *success* field is false, then there were no changes to data and you need to resend request.

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
|data| json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
|success | boolean | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error | string | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

Data contains array of inventory objects. For more detailed definition of objects refer to [Object documentation](storehouse%20objects) in wiki.

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| inventories | array of inventory objects | Deleted inventories. Note that field *groups* will not be set even if inventory is not empty. |

> `groups` field will not be set in response.

**Response Example**

```json

{
  "success": true,
  "data" {
    "inventories": [
      {
        "id": 2,
        "name": "Fruit",
        "description": "Fruit and other related vegetables"
      },
      {
        "id": 3,
        "name": "Water"
      },
    ]
  }
}
```

When attempting to delete default inventory, with `id` 1

```json
{
    "error": "4000: Default Inventory cannot be deleted",
    "success": false
}
```
