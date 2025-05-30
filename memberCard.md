**Endpoint: /api/v1/memberCard**

[Get](#GET)

[Create or Update](#UPDATE)

### GET ###

Returns a specific member card, or a list of all cards.

* Sending empty returns a list of all member cards (doesn't return deleted cards)
* Request with `uuid` set returns only that one member card

**Request data**

| field name              |     type      | Description                         |
| :---------------------- | :-----------: | :---------------------------------- |
| uuid      | String (UUID)| Optional UUID of member card |

**Request Examples**

Get all member cards

```json
{
  "action": "GET",
  "data": {}
}
```

Get one member card

```json
{
  "action": "GET",
  "data": {
    "uuid": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f"
  }
}
```

**Response**

| field name              |     type      | Description                                                  |
| :---------------------- | :-----------: | :----------------------------------------------------------- |
| memberCards | MemberCard[] | List of member cards

 #### Member card ####

| Field Name | Type         | Description                                                        |
|------------|--------------|--------------------------------------------------------------------|
| uuid       | String (UUID)| UUID of member card |
| firstName  | String       | first name of customer |
| lastName   | String       | last name of customer |
| email      | String       | email address of customer |
| cardId     | String       | ID of card (android app reads this via NFC) |
| discount   | BigDecimal   | discount in % that will be applied for this customer |
| deleted    | Boolean      | deleted cards are returned only when queried by UUID |

**Response Example**

```json
{
  "success": true,
  "data": {
    "memberCards": [
      {
        "uuid": "29cd0081-9e47-4cbf-b124-354f2d01fbec",
        "firstName": "John",
        "lastName": "Doe",
        "email": "john.doe@email.com"
        "cardId": "123",
        "discount": 5,
        "deleted": false
      }
    ]
  }
}
```


### UPDATE ###

Creates or updates a member card.

* Sending request without `uuid` creates a new member card with random UUID
  * `firstName` and `lastName` are required
* Sending request with `uuid` set
  * if a card with this UUID exists, it will be updated using non-null values from the request
  * if no card has this UUID, new card will be created using it (`firstName` and `lastName` required)

**Request data**

| field name              |     type      | Description                         |
| :---------------------- | :-----------: | :---------------------------------- |
| uuid      | String (UUID)| UUID of member card |
| firstName | String     | first name of customer |
| lastName  | String     | last name of customer |
| email     | String     | email address of customer |
| cardId    | String     | ID of card (android app reads this via NFC) |
| discount  | BigDecimal | discount in % that will be applied for this customer |
| deleted   | Boolean    | used to delete / restore member card info |

**Request Examples**

Create new member card

```json
{
  "action": "UPDATE",
  "data": {
    "firstName": "John",
    "lastName": "Doe",
    "cardId": "123",
    "discount": 5
  }
}
```

Update member card

```json
{
  "action": "UPDATE",
  "data": {
    "uuid": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f",
    "deleted": true
  }
}
```

**Response**

| field name              |     type      | Description                                                  |
| :---------------------- | :-----------: | :----------------------------------------------------------- |
| data | String (UUID) | UUID of the affected member card

**Response Example**

```json
{
  "success": true,
  "data": "ac9aef30-9c11-4f53-b59e-f2af98c3e13f"
}
```

