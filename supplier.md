**Endpoint: /api/v1/supplier**

[Get](#markdown-header-get)

[Create or Update](#markdown-header-update)

[Delete](#markdown-header-delete)

Supplier endpoint allows user to store information about suppliers that are frequently used, saving time and work when creating new inventory cards. For usage of suppliers refer to [card](https://bitbucket.org/papayapos/papayapos/wiki/Card) endpoint.

### Get ###

For getting suppliers you have following options.

* Sending empty `data` field will make API return all suppliers.
* Sending `ids` will make API return all suppliers that match provided `ids`.

> Non-existent `ids` will be ignored

**Request data**

| field name |       type       | Description               |
| :--------- | :--------------: | :------------------------ |
| ids        | array of numbers | Long, requested suppliers |

**Request Example**

Request below shows how to get all suppliers.

```json
{
  "action": "GET",
  "data": {
  }
}
```

Request below shows how to use `ids` to get specific suppliers.

```json
{
  "action": "GET",
  "data": {
    "ids": [1]
  }
}
```

**Response**

Response contains array of supplier objects. For the detailed definition of objects refer to the [Object documentation](https://bitbucket.org/papayapos/papayapos/wiki/storehouse%20objects) in wiki.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |           type            | Description                                |
| :--------- | :-----------------------: | :----------------------------------------- |
| suppliers  | array of supplier objects | List of suppliers that match request data. |

**Response Example**


```json

{
    "data": {
        "suppliers": [
            {
                "id": 1,
                "address": "Street no. 1",
                "city": "Bratislava",
                "contactPerson": "Elena",
                "country": "Slovakia",
                "email": "abc@cde.efg",
                "icDph": "12345",
                "ico": "44444444",
                "name": "Silvia",
                "phone": "1234567890",
                "registration": true,
                "registrationDate": "12.05.2022 00:00:00",
                "registrationNumber": "55"
            }
        ]
    },
    "success": true
}

```

### Update ###

Creating and updating can be done simultaneously in one request. Depending on data provided, API will create or update new suppliers. 

* Sending supplier **without** `id`. API will create new supplier and generate `id` for it.
* Sending supplier **with** `id`.
    * In case supplier already exists, its data will be overwritten.
    * In case supplier does not exist, API will create new supplier and use provided `id`.

> Updating supplier overwrites all its fields with provided information. Even if you want to update only one  field, you still need to send all data. 
>
> All fields are optional and as such it is permitted to send empty supplier and let API generate `id` for it. Usefulness of such supplier is questionable though.

**Request data**

| field name |           type            | Description                       |
| :--------- | :-----------------------: | :-------------------------------- |
| suppliers  | array of supplier objects | Supplier to be created or updated |

**Request Example**

```json
{
  "action":"UPDATE",
  "data":{
    "suppliers": [
            {
                "id": 1,
                "address": "Street no. 1",
                "city": "Bratislava",
                "contactPerson": "Elena",
                "country": "Slovakia",
                "email": "abc@cde.efg",
                "icDph": "12345",
                "ico": "44444444",
                "name": "Silvia",
                "phone": "1234567890",
                "registration": true,
                "registrationDate": "12.05.2022 00:00:00",
                "registrationNumber": "55"
            }
    ]
  }
}
```

**Response**

Response contains array of suppliers that were created or updated. For the detailed definition of objects refer to the [Object documentation](https://bitbucket.org/papayapos/papayapos/wiki/storehouse%20objects) in wiki.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

**Response Example**

```json
{
    "data": {
        "suppliers": [
            {
                "address": "Street no. 1",
                "city": "Bratislava",
                "contactPerson": "Julia",
                "country": "Slovakia",
                "email": "abc@cde.efg",
                "icDph": "12345",
                "ico": "44444444",
                "id": 1,
                "name": "Silvia",
                "phone": "1234567890",
                "registration": true,
                "registrationDate": "12.05.2022 00:00:00",
                "registrationNumber": "55"
            }
        ]
    },
    "success": true
}

```

### Delete ###

Deleting supplier is done via their `ids`. API keeps supplier data in cards separately so even if you delete supplier, existing cards will not be affected.

> API will ignore non-existent `ids`.

**Request data**

| field name |       type       | Description                     |
| :--------- | :--------------: | :------------------------------ |
| ids        | array of numbers | ids of suppliers to be deleted. |

**Request Example**

```json
{
  "action": "DELETE",
  "data": {
    "ids": [1, 2, 3, 4]
  }
}
```
**Response**

Successful delete request will return array of supplier objects that were deleted. If there is any problem and response success field is false, then no suppliers were deleted. For the detailed definition of objects refer to the [Object documentation](https://bitbucket.org/papayapos/papayapos/wiki/storehouse%20objects) in wiki.

| field name |    type     | Description                                                  |
| :--------- | :---------: | :----------------------------------------------------------- |
| data       | json object | All successful responses contain data field that contains json with requested data. This field is only set if success is true. |
| success    |   boolean   | Indicates whether request was successfully processed or not, if its false there is error message in response as well. |
| error      |   string    | Error message with cause of failure. This field is only set if success is false. |

Contents of data for this response:

| field name |           type            | Description       |
| :--------- | :-----------------------: | :---------------- |
| suppliers  | array of supplier objects | Deleted suppliers |

**Response Example**


```json

{
    "data": {
        "suppliers": [
            {
                "address": "Street no. 1",
                "city": "Bratislava",
                "contactPerson": "Julia",
                "country": "Slovakia",
                "email": "abc@cde.efg",
                "icDph": "12345",
                "ico": "44444444",
                "id": 1,
                "name": "Silvia",
                "phone": "1234567890",
                "registration": true,
                "registrationDate": "12.05.2022 00:00:00",
                "registrationNumber": "55"
            }
        ]
    },
    "success": true
}
```