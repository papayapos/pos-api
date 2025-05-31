**Endpoint: /api/v1/catalog/item**

[Get](#GET)

### GET ###

For getting data from item catalog (pricelist).

* Send list of `UUID` to show only items which has the UUID from the list.

**Request data**

Send just empty ids list to get all items. Or send ids to get particular set of areas.

| field name        |    type       | Description                       |
| :------------     | :----:        | :-------------------------------- |
| item              | UUID          | id of the area (table)            |
| items             | List<UUID>    | name of the area                  |
| category          | UUID          | number of seats that the area has |
| categories        | List<UUID>    | id of room                        |

**Request Example**

For getting all items, you can send empty data.

```json
{
  "action": "GET",
  "data": {
    "ids": []
  }
}
```

**Response**
...

**Response Example**

```json
{
  "success": true,
  "data": {
    "categories": [
      {
        "id": "5de57c08-6b12-4de5-8d6f-edccaf82cd2d",
        "title": "bbb",
        "position": 0,
        "color": "RED_2",
        "items": [
          {
            "id": "360ded1b-8dcb-410e-9f0e-0aa80bc1720f",
            "title": "abcde",
            "price": 123,
            "vatRate": 20,
            "color": "VIOLET_1",
            "isFavorite": false,
            "isHidden": false,
            "isOutOfStock": false,
            "position": 0
          },
          {
            "id": "7e8a6ea0-2832-4c15-b686-2b9a80a7b573",
            "title": "asdasdas",
            "price": 12,
            "vatRate": 20,
            "color": "ORANGE_1",
            "isFavorite": false,
            "isHidden": false,
            "isOutOfStock": false,
            "position": 1
          }
        ]
      },
      {
        "id": "e33a3bf6-d6c2-43c9-a4e5-ad1fae30fd2f",
        "title": "aaa",
        "position": 1,
        "color": "ORANGE_1",
        "items": [
          {
            "id": "104f4c2b-c51f-4619-8777-52b1629f7ece",
            "title": "pole",
            "price": 2,
            "vatRate": 20,
            "color": "VIOLET_3",
            "isFavorite": false,
            "isHidden": false,
            "isOutOfStock": false,
            "position": 0
          }
        ]
      }
    ]
  }
}
```
