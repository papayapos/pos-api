**Endpoint: /api/v1/salePlace**

[Get](#GET)

### GET ###

Returns a specific sale place, or a list of all sale places.

* Sending empty returns a list of all sale places
* Request with `uuid` set returns only that one sale place

**Request data**

| field name              |     type      | Description                         |
| :---------------------- | :-----------: | :---------------------------------- |
| uuid      | String (UUID)| Optional UUID of sale place |

**Request Examples**

Get all sale places

```json
{
  "action": "GET",
  "data": {}
}
```

Get one sale place

```json
{
  "action": "GET",
  "data": {
    "uuid": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
  }
}
```

**Response**

| field name              |     type      | Description                                                  |
| :---------------------- | :-----------: | :----------------------------------------------------------- |
| salePlaces | SalePlace[] | List of sale places

 #### Sale Place ####

| Field Name | Type         | Description                                                        |
|------------|--------------|--------------------------------------------------------------------|
| uuid       | String (UUID)| UUID of sale place |
| name  | String       | name of sale place |

**Response Example**

```json
{
  "success": true,
  "data": {
    "salePlaces": [
      {
        "name": "Sale place",
        "uuid": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
      }
    ]
  }
}

```
