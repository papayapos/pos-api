**Endpoint: /api/v1/areas**

[Get](#GET)

[UPDATE](#UPDATE)

### GET ###

For getting ids of all areas (tables)

* Send list of `id` to show only those areas.

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
| roomId        |  Long  | id of room                        |

**Response Example**

```json
{
    "data": {
        "areas": [
            {
                "id": 3,
                "name": "INT 01",
                "numberOfSeats": 0,
                "roomId: 1
            },
            {
                "id": 4,
                "name": "INT 02",
                "numberOfSeats": 0,
                "roomId: 1
            },
            {
                "id": 5,
                "name": "INT 03",
                "numberOfSeats": 0,
                "roomId: 1
            },
            {
                "id": 6,
                "name": "TER 01",
                "numberOfSeats": 0,
                "roomId: 2
            },
            {
                "id": 7,
                "name": "TER 02",
                "numberOfSeats": 0,
                "roomId: 2
            },
            {
                "id": 8,
                "name": "TER 03",
                "numberOfSeats": 0,
                "roomId: 2
            }
        ]
    },
    "success": true
}
```

### UPDATE ###

Create or update an area

* requests with `id` update an existing area
* request without `id` creates a new area (`roomId` is required)

**Request data**


| field name |    type     | Description                             |
| :--------- | :---------: | :-------------------------------------- |
| id         | Long        | id of table to update                   |
| name       | String      | name of table |
| numberOfSeats| Integer   | number of seats at the table |
| roomId     | Long | id of room (parent area) |

**Request Example**

Create new table

```json
{
  "action": "UPDATE",
  "data": {
    "name": "new table",
    "roomId": 1
  }
}
```

Update existing table. `roomId` is optional - table will remain assigned to its current room if no value is sent.

```json
{
  "action": "UPDATE",
  "data": {
    "id": 4
    "name": "rename table",
    "numberOfSeats: 2
  }
}
```

