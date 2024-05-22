**Endpoint: /api/v1/transaction/accountingTransaction**

[Get](#GET)

Purpose is to get opened bills for the table.

### Get

For getting Receipts you have following options.

* Sending `id` of area to get list of opened accounting transactions.

**Request data**

area `id`, other fields are ignored

| field name | type | Description                                     |
| :--------- | :--: | :---------------------------------------------- |
| areaId     | Long | id of area (table) from which you get the bills |

**Request Example**

For getting all inventories with their items, you can send empty data.

```json
{
  "action": "GET",
  "data": {
    "areaId" : 2
  }
}
```

**Response**

list of opened accounting transactions that belongs to the area

| field name                            |    type    | Description                                                  |
| :------------------------------------ | :--------: | :----------------------------------------------------------- |
| accountingTransactionState            |   STRING   | state of the accounting transaction (only OPENED)            |
| area                                  |   Object   | same object as in area endpoint                              |
| createTime                            |  Datetime  | date and time in which the bill was created                  |
| createdByUser                         |    int     | Id of user that created the bill                             |
| priceGross                            | BigDecimal | gross price of the bill                                      |
| transactionPriceAdjustmentCoefficient | BigDecimal | adjusts price for every item on the bill (range 0-1 is discount, higher is surcharge) |
| customerUuid                          |    UUID    | internal UUID of member card                                 |
| memberCardId                          |   String   | physical card ID (scanned by android app)                    |



**Response Example**

```json
{
    "data": {
        "accountingTransactions": [
            {
                "accountingTransactionState": "OPENED",
                "area": {
                    "id": 2,
                    "numberOfSeats": 0
                },
                "createTime": "30.09.2022 17:07:48",
                "createdByUserId": 1,
                "id": "78c06986-3a94-4993-8b3b-09bbb73265da",
                "priceGross": 12.2,
                "humanId":"100",
                "transactionPriceAdjustmentCoefficient": 1,
                "customerUuid": "29cd0081-9e47-4cbf-b124-354f2d01fbec",
                "memberCardId": "123456"
            },
            {
                "accountingTransactionState": "OPENED",
                "area": {
                    "id": 2,
                    "numberOfSeats": 0
                },
                "createTime": "30.09.2022 17:07:58",
                "createdByUserId": 1,
                "id": "b93045af-3fd5-4bc5-92a4-91f346e1cc5e",
                "priceGross": 1.3,
                "humanId":"101",
                "transactionPriceAdjustmentCoefficient": 1,
                "customerUuid": "0bee35d7-c993-425d-8de8-716d5fa30c8a",
                "memberCardId": "341"
            },
            {
                "accountingTransactionState": "OPENED",
                "area": {
                    "id": 2,
                    "numberOfSeats": 0
                },
                "createTime": "30.09.2022 17:08:01",
                "createdByUserId": 1,
                "id": "a7e92c6d-8db5-4753-8613-de50a6b710ea",
                "priceGross": 14.8,
                "humanId":"102",
                "transactionPriceAdjustmentCoefficient": 1
            }
        ]
    },
    "success": true
}
```

