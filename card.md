**Endpoint: /api/v1/inventory/card**

[Get](#GET)

[Create or Update](#UPDATE)

[Delete](#DELETE)

Cards are used to record movement of items in inventories. Allowing user to record detailed information about supplier, type of movement, item, prices and more. Each card contains array of items that describe changes to [stock items](item.md).

### Get

For getting cards you have following options.

* Sending `ids` of cards you want to retrieve.
* Sending date range `from` and `to` to get all cards that were created between dates, additional options are available to date range.
    * Optional `inventoryId`, API will only return cards for inventory.
    * Optional `documentType`, API will only return cards of provided document type.
    * Optional `supplierName`, API will only return cards from supplier. 

> Note that there is no option to retrieve everything. This limitation is due to fact that number of cards grow over time ad infinitum. Over time would responses to such requests grow to unreasonable sizes.
>
> If you request ids that are not in database they will be ignored and only found cards will be returned.
>
> `ids` and `from`/`to` range cannot be combined in single request, if both options are provided `ids` will take priority and data range will be ignored.

**Request data**

Option 1: `ids`, other fields are ignored

| field name |       type       | Description                               |
| :--------- | :--------------: | :---------------------------------------- |
| ids        | array of numbers | long, id of stock taking card in database |

Option 2: `from`, `to` range and optional parameters

| field name   |  type  | Description                                                |
| :----------- | :----: | :--------------------------------------------------------- |
| from         | string | start of interval that is to be retrieved                  |
| to           | string | optional, end of interval that is to be retrieved          |
| inventoryId  | number | long, optional                                             |
| documentType | number | long, optional, 1 = fiscal receipt, 2 = invoice, 3 = other |
| supplierName | string | optional, name of supplier                                 |

**Request Example**

For getting cards by `ids`:

```json
{
  "action": "GET",
  "data": {
    "ids": [1, 2, 3, 4]
  }
}
```

For getting cards that are between `from` and `to`:

```json
{
  "action": "GET",
  "data": {
    "from": "11.03.2020 01:01:03",
    "to": "11.03.2022 01:01:03"
  }
}
```

For getting cards that are between `from` and `to`, with additional parameters:

```json
{
  "action": "GET",
  "data": {
    "from": "11.03.2020 01:01:03",
    "to": "11.03.2022 01:01:03",
    "inventoryId": 1,
    "documentType": 1,
    "supplierName": "Silvia"
  }
}
```

**Response**

Response contains array of card objects. For the detailed definition of objects refer to the [Object documentation](storehouse%20objects.md) in wiki.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |         type          | Description                                                  |
| :--------- | :-------------------: | :----------------------------------------------------------- |
| cards      | array of card objects | List of requested cards. If no cards were found this field will contain empty array. |

**Response Example**

```json
{
    "data": {
        "cards": [
            {
                "id": 1,
                "createTime": "11.03.2021 11:04:30",
                "documentType": 1,
                "placedTime": "11.03.2021 11:04:29",
                "inventoryId": 1,
                "supplier": {
                    "id": 1,
                    "address": "Street no. 1",
                    "city": "Bratislava",
                    "contactPerson": "Julia",
                    "country": "Slovakia",
                    "email": "abc@cde.efg",
                    "icDph": "12345",
                    "ico": "44444444",
                    "name": "Silvia",
                    "phone": "1234567890",
                    "registration": true,
                    "registrationDate": "12.05.2022 00:00:00",
                    "registrationNumber": "55"
                },
                "items": [
                    {
                        "id": 1,
                        "stockChangeCardId": 1,
                        "stockItemId": 1,
                        "stockItemTitle": "Apple",
                        "measuringUnit": "piece",
                        "amount": 10,
                        "priceNet": 2,
                        "vatRate": 20
                    }
                ]
            }
            
        ]
    },
    "success": true
}

```

### Update

There are multiple ways to create or update cards. We will first cover cards and after that supplier and finally card items. While they must be created or updated in same request as card they belong to, there are important things to note about them.

For cards these are options for creating and updating:

* Sending request **without** `id`, API will create new card and generate `id` for it.
* Sending request **with** `id`.
    * `id` does not exists, API will create new card and use `id` provided in request.
    * `id` already exists, API will update existing card.

For supplier this applies:

* Providing supplier **with** `id` will use stored data. If you provide additional supplier data apart from `id`, then they will be used instead of stored supplier data. For fields not provided in request, API will use data from stored object. Only supplier data in this card will be altered. Original stored supplier object nor any other card will not change.
* Providing supplier **without** `id. Provided data will be stored in card. No default data will be fetched as `id` is not provided. This will not create new supplier and if you want to create new supplier you should do so in [supplier](https://bitbucket.org/papayapos/papayapos/wiki/Supplier) endpoint.

And finally these are options and conditions for card items:

* When creating new card, card item `id` will be ignored, API will generate new `id` that will be returned in response. This is to prevent items from sharing cards as well as accidental 'stealing' of items between cards.
* When updating existing card.
    * If card item with provided `id` exists **and** belongs to **updated** card. Then item will be overwritten with item from request.
    * If card item with provided `id` exists **but** belongs to **other** card. Then provided `id` will be ignored, API will generate new `id` and create new card item for updated card. This is to prevent accidental 'stealing of items between cards.

API always deletes or overwrites previous card items. Because of this it is viable approach to never provide item `id`s and let API handle them. 

**Request data**

| field name |                type                | Description                         |
| :--------- | :--------------------------------: | :---------------------------------- |
| cards      | array of stock change card objects | Stock change cards with their items |

**Request Example**


```json
{ 
  "action": "UPDATE",
  "data": {
    "cards": [
      {
        "id": 1,
        "inventoryId": 1,
        "createTime": "17.02.2022 14:04:03",
        "placedTime": "17.02.2022 14:04:03",
        "type": "RECEIPT_CARD",
        "documentId": "1234",
        "documentType": 1,
        "supplier": {
          "id": 1,
          "address": "Street no. 1",
          "city": "Bratislava",
          "contactPerson": "Julia",
          "country": "Slovakia",
          "mail": "abc@cde.efg",
          "icDph": "12345",
          "ico": "44444444",
          "name": "Silvia",
          "phone": "1234567890",
          "registration": true,
          "registrationDate": "12.05.2022 00:00:00",
          "registrationNumber": "55"
        },
        "orderNumber": "12",
        "deliveryNote": "delivery number (number of delivery confirmation document)",
        "note": "Note",
        "items": [
          {
            "amount": 100,
            "measuringUnit": "ks",
            "priceNet": 0.5,
            "stockItemId": 1,
            "stockItemTitle": "Apple",
            "vatRate": 20
          }
        ]
      }
      ]
  }
}
```


**Response**

Response contains array of card objects with their supplier and items. If you are planning to use item `id`s you should store them now.  For the detailed definition of objects refer to the [Object documentation](storehouse%20objects.md) in wiki.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                type                | Description                         |
| :--------- | :--------------------------------: | :---------------------------------- |
| cards      | array of stock change card objects | Stock change cards with their items |


**Response Example**

```json
{
    "data": {
        
    "cards": [
      {
        "id": 1,
        "inventoryId": 1,
        "createTime": "17.02.2022 14:04:03",
        "placedTime": "17.02.2022 14:04:03",
        "type": "RECEIPT_CARD",
        "documentId": "1234",
        "documentType": 1,
        "supplier": {
          "id": 1,
          "address": "Street no. 1",
          "city": "Bratislava",
          "contactPerson": "Julia",
          "country": "Slovakia",
          "mail": "abc@cde.efg",
          "icDph": "12345",
          "ico": "44444444",
          "name": "Silvia",
          "phone": "1234567890",
          "registration": true,
          "registrationDate": "12.05.2022 00:00:00",
          "registrationNumber": "55"
        },
        "orderNumber": "12",
        "deliveryNote": "delivery note",
        "note": "Note",
        "items": [
          {
            "id": 1,
            "amount": 100,
            "measuringUnit": "ks",
            "priceNet": 0.5,
            "stockChangeCardId": 3,
            "stockItemId": 1,
            "stockItemTitle": "Apple",
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

For deleting cards only option is to use their `ids`. It is important to note that card items cannot exist without card and will be deleted along with the card. Deleting card does not affect supplier in case it was stored using [supplier](supplier.md) endpoint.

**Request data**

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

Successful delete request will return array of card objects that were deleted. If there are any problems and response success field is false, then there will be no changes to stored data.  For the detailed definition of objects refer to the [Object documentation](storehouse%20objects.md) in wiki.


| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |                type                | Description                 |
| :--------- | :--------------------------------: | :-------------------------- |
| cards      | array of stock change card objects | Deleted stock change cards. |

**Response Example**


```json
{
    "data": {
        
    "cards": [
      {
        "id": 1,
        "inventoryId": 1,
        "createTime": "17.02.2022 14:04:03",
        "placedTime": "17.02.2022 14:04:03",
        "type": "RECEIPT_CARD",
        "documentId": "1234",
        "documentType": 1,
        "supplier": {
          "id": 1,
          "address": "Street no. 1",
          "city": "Bratislava",
          "contactPerson": "Julia",
          "country": "Slovakia",
          "mail": "abc@cde.efg",
          "icDph": "12345",
          "ico": "44444444",
          "name": "Silvia",
          "phone": "1234567890",
          "registration": true,
          "registrationDate": "12.05.2022 00:00:00",
          "registrationNumber": "55"
        },
        "orderNumber": "12",
        "deliveryNote": "delivery note",
        "note": "Note",
        "items": [
          {
            "id": 1,
            "amount": 100,
            "measuringUnit": "ks",
            "priceNet": 0.5,
            "stockChangeCardId": 3,
            "stockItemId": 1,
            "stockItemTitle": "Apple",
            "vatRate": 20
          }
        ]
      }
      ]
    },
    "success": true
}
```
