**Endpoint: /api/v1/transaction/accountingEntry**

[Get](#GET)

### Get

Request gets all bill items

* Sending `id` of opened bill to get all items that are on the bill.

**Request data**

accounting transaction (bill) `id`, other fields are ignored

| field name              |     type      | Description                         |
| :---------------------- | :-----------: | :---------------------------------- |
| accountingTransactionId | STRING (UUID) | id of accounting transaction (bill) |

**Request Example**

For getting all inventories with their items, you can send empty data.

```json
{
  "action":"GET",
  "data": {
    "accountingTransactionId":"a7e92c6d-8db5-4753-8613-de50a6b710ea"
  }
}
```



**Response**

| field name              |     type      | Description                                                  |
| :---------------------- | :-----------: | :----------------------------------------------------------- |
| id                      |     Long      | id of the accounting entry                                   |
| accountableItemId       | String (UUID) | id of the accounting transaction (bill) on which the entry is |
| title                   |    String     | title of the acccounting entry                               |
| accountingEntryState    |    String     | state of the accounting entry (only CREATED)                 |
| accountingEntryType     |    String     | type of accounting entry (only SALE)                         |
| accountingTransactionId | String (UUID) | id of accounting transaction (bill)                          |
| txAdjustCoef            |  BigDecimal   | adjusts price in the case of discount added on Transaction   |
| adjustedPrice           |  BigDecimal   | adjusted price by adjusting coeficient                       |
| priceAdjustCoef         |  BigDecimal   | adjusts price in the case of discount added on entry         |
| priceAmount             |  BigDecimal   | original price of the item                                   |
| priceCurrency           |    String     | price of the currency                                        |
| count                   |  BigDecimal   | count of this accounting entry                               |
| quantity                |  BigDecimal   | quantity of the item in unit in which item is measured (informational) |
| unit                    |    String     | title for unit in which item is measured (informational)     |
| createTime              |   Datetime    | date and time in which the entry was created                 |
| vatRateId               |      int      | Internal Vatrate Id.                                         |
| vatRateValue            |  BigDecimal   | Value of vatrate in %                                        |



**Response Example**

```json
{
    "data": {
        "accountingEntries": [
            {
                "accountableItemId": "b6aeb536-5829-41f1-ab86-aba8f69d614b",
                "accountingEntryState": "CREATED",
                "accountingEntryType": "SALE",
                "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
                "adjustedPrice": 3.5,
                "count": 1,
                "createTime": "30.09.2022 17:08:03",
                "id": "ac67a376-5c51-4b71-a6a9-2425f42bdff1",
                "priceAdjustCoef": 1,
                "priceAmount": 20,
                "priceCurrency": "EUR",
                "quantity": 0.5,
                "title": "Kvitnúci čaj",
                "txAdjustCoef": 1,
                "unit": "l",
                "vatRateId": 1,
                "vatRateValue": 20
            },
            {
                "accountableItemId": "d79e8d9e-c1f4-439b-9e6e-9deb4806a7a6",
                "accountingEntryState": "CREATED",
                "accountingEntryType": "SALE",
                "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
                "adjustedPrice": 4.0,
                "count": 1,
                "createTime": "30.09.2022 17:08:03",
                "id": "45c87416-1c1f-4d57-9453-cc7afc085f49",
                "priceAdjustCoef": 1,
                "priceAmount": 20,
                "priceCurrency": "EUR",
                "quantity": 0.5,
                "title": "Lung Ťing (Dračia studňa)",
                "txAdjustCoef": 1,
                "unit": "l",
                "vatRateId": 1,
                "vatRateValue": 20
            },
            {
                "accountableItemId": "d7553975-557f-4542-96ad-b67fc9b09b2a",
                "accountingEntryState": "CREATED",
                "accountingEntryType": "SALE",
                "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
                "adjustedPrice": 3.8,
                "count": 1,
                "createTime": "30.09.2022 17:08:03",
                "id": "58ffec43-b8f0-46e4-b950-e0e1d7a15af5",
                "priceAdjustCoef": 1,
                "priceAmount": 20,
                "priceCurrency": "EUR",
                "quantity": 0.5,
                "title": "Zelený čaj s jazmínom",
                "txAdjustCoef": 1,
                "unit": "l",
                "vatRateId": 1,
                "vatRateValue": 20
            },
            {
                "accountableItemId": "3bbd3e33-d28f-4e0d-97d7-c4b263e135c3",
                "accountingEntryState": "CREATED",
                "accountingEntryType": "SALE",
                "accountingTransactionId": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
                "adjustedPrice": 3.5,
                "count": 1,
                "createTime": "30.09.2022 17:08:04",
                "id": "84b2090a-7b92-4373-8222-fa7edee69f59",
                "priceAdjustCoef": 1,
                "priceAmount": 20,
                "priceCurrency": "EUR",
                "quantity": 0.5,
                "title": "Čaj Tuarégov zelený",
                "txAdjustCoef": 1,
                "unit": "l",
                "vatRateId": 1,
                "vatRateValue": 20
            }
        ]
    },
    "success": true
}
```

[POST](#POST)

### POST

Request post accountable item on bill. 

* Sending accountable item on the bill (accountingTransactionId).

**Request data**

add these parameters to add accountable item on accounting transaction (bill) `id`

| field name              |     type      | Description                         |
| :---------------------- | :-----------: | :---------------------------------- |
| accountingTransactionId | STRING (UUID) | id of accounting transaction (bill) |
| accountableItemId       | STRING (UUID) | id of accountable Item              |
| count                   | BigDecimal (Number) | count of items on the bill    |
| price                   | BigDecimal (Number) | id of accounting transaction (bill) |
| state                   | STRING (ENUM) | use only String CREATED             |
| type                    | STRING (ENUM) | use only String SALE                |

**Request Example**

Requets for adding accounting entry on the bill (AccountingTransaction).

```json
{
  "action":"UPDATE",
  "data": {
    "accountingTransactionId":"a7e92c6d-8db5-4753-8613-de50a6b710ea",
    "accountableItemId" : "a7e92c6d-8db5-4753-8613-de50a6b71ttt",
    "count" : 2,
    "price" : 2.50,
    "state" : "CREATED",
    "type" : "SALE"
  }
}
```


