**Endpoint: /api/v1/memberCard**

[Create or Update](#UPDATE)

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

