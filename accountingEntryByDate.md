**Endpoint: /api/v1/transaction/accountingEntry/date**

[Get](#GET)

### Get

Request gets all items from a date range

* Sending `fromDate` and `toDate` to get all items from that date range.

**Request data**

| field name              |     type      | Description                         |
| :---------------------- | :-----------: | :---------------------------------- |
| fromDate | Long | UNIX timestamp in miliseconds |
| toDate | Long | UNIX timestamp in miliseconds |

**Request Example**

```json
{
  "action": "GET",
  "data": {
    "fromDate": 1713873874129,
    "toDate": 1713873934000
  }
}
```



**Response**

| field name              |     type      | Description                                                  |
| :---------------------- | :-----------: | :----------------------------------------------------------- |
| menuId | String (UUID) | id of menu of the entry |
| txUuid | String (UUID) | id of the transaction |
| createDate | Long | unix timestamp (in milliseconds) of entry creation |
| id | String (UUID) | id of the entry |
| title" | String | title of the entry |
| plu | String | PLU code of the entry |
| menuTitle | String | title of the menu of the entry |
| count | BigDecimal | count of the entry |
| basePrice | BigDecimal | base price of the entry|
| unitPrice | BigDecimal | price per unit |
| priceAdjustCoef | BigDecimal | price adjustment coefficient |
| totalPrice | BigDecimal | total price without vat |
| totalWithVat | BigDecimal | total price (including adjustment and count) |
| vatRate | BigDecimal | vat rate in % |
| type | String | entry type (SALE, REFUND, VOUCHER) |
| user | String | name of user who created the entry |
| revenueCenterName | String | name of revenue center |
| note | String | entry note |
| salePlaceName | String | name of sale place |






**Response Example**

```json
{
  "success": true,
  "data": [
    {
      "menuId": "00000000-0000-0000-0000-000000000007",
      "txUuid": "82046ac6-69ef-46f1-8616-8bf832c8a146",
      "createDate": 1713873874129,
      "id": "f9c7ecca-552a-4f27-8fec-d78beaa34285",
      "title": "Zlatý bažant radler - citrón",
      "plu": "0719",
      "menuTitle": "Pivo",
      "count": 1,
      "basePrice": 1.40,
      "unitPrice": 1.40,
      "priceAdjustCoef": 1,
      "totalPrice": 1.1667,
      "totalWithVat": 1.40,
      "vatRate": 20,
      "type": "SALE",
      "user": "Administrátor ",
      "salePlaceName": "Pokladňa"
    }
  ]
}
```

