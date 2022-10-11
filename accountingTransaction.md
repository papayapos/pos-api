**Endpoint: /api/v1/transaction/AccountingTransaction**

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
                "transactionPriceAdjustmentCoefficient": 1
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
                "transactionPriceAdjustmentCoefficient": 1
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
                "transactionPriceAdjustmentCoefficient": 1
            }
        ]
    },
    "success": true
}
```
