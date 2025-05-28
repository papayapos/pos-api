**Endpoint: /api/v1/areas/rooms**

[Get](#GET)

[UPDATE](#UPDATE)

### GET ###

For getting ids of all rooms

* Send list of `id` to show only those rooms.

**Request data**

Send just empty ids list to get all rooms. Or send ids to get particular set of areas.

| field name |    type     | Description                             |
| :--------- | :---------: | :-------------------------------------- |
| ids        | Array[Long] | ids of rooms to get data about |

**Request Example**

For getting all rooms, you can send empty data.

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

**Response Example**

```json
{
    "data": {
        "areas": [
            {
                "id": 1,
                "name": "Bar"
            },
            {
                "id": 2,
                "name": "Terrace"
            },
        ]
    },
    "success": true
}
```

### UPDATE ###

Create or update a room

* requests with `id` update an existing room
* request without `id` creates a new room

**Request data**


| field name |    type     | Description                             |
| :--------- | :---------: | :-------------------------------------- |
| id         | Long        | id of room to update                   |
| name       | String      | name of room |

**Request Example**

Create new room

```json
{
  "action": "UPDATE",
  "data": {
    "name": "new room"
  }
}
```

Update existing room

```json
{
  "action": "UPDATE",
  "data": {
    "id": 2
    "name": "rename room"
  }
}
```

