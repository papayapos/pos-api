**Endpoint: /api/v1/areas**

[Get](#GET)

### Get

For getting ids of all areas (tables)

* Sending `id` of area to get list of opened accounting transactions.

**Request data**

Send just empty ids list to get all areas. Or send ids to get particular set of areas.

| field name |    type     | Description                             |
| :--------- | :---------: | :-------------------------------------- |
| ids        | Array[Long] | ids of areas (tables) to get data about |

**Request Example**

For getting all areas, you can send empty data.

```json
{
  "action": "GET",
  "data": {
    "ids": []
  }
}
```

**Response**

| field name    |  type  | Description                       |
| :------------ | :----: | :-------------------------------- |
| id            |  Long  | id of the area (table)            |
| name          | String | name of the area                  |
| numberOfSeats |  Long  | number of seats that the area has |

**Response Example**

```json
{
    "data": {
        "areas": [
            {
                "id": 3,
                "name": "INT 01",
                "numberOfSeats": 0
            },
            {
                "id": 4,
                "name": "INT 02",
                "numberOfSeats": 0
            },
            {
                "id": 5,
                "name": "INT 03",
                "numberOfSeats": 0
            },
            {
                "id": 6,
                "name": "TER 01",
                "numberOfSeats": 0
            },
            {
                "id": 7,
                "name": "TER 02",
                "numberOfSeats": 0
            },
            {
                "id": 8,
                "name": "TER 03",
                "numberOfSeats": 0
            }
        ]
    },
    "success": true
}
```

