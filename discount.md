**Endpoint: /api/v1/transaction/discount**

[UPDATE](#UPDATE)

### UPDATE

For updating the discount ratios.

| Field Name              | Type       | Description                                                           |
|-------------------------|------------|-----------------------------------------------------------------------|
| accountingEntryId       | UUID       | The id of the accounting entry you would like to discount             |
| accountingTransactionId | UUID       | The id of the transaction you would like to discount                  |
| discountRate            | BigDecimal | A 0-1 value for discounting a price in the manner of 0.8 = price*0.8  |

**Example**

---Warning---

Do not fill both accountingTransaction and accountingEntry ids at once, as the response will only contain the entry.

Accounting Entry Query
```json
{
  "action":"UPDATE",
  "data": {
    "accountingEntryId": "6b2b82c2-3423-4905-966a-d66811e0b88a",
    "discountRate": 0.8
  }
}
```

Accounting Transaction Query
```json
{
  "data": {
    "discountRate": 0.8,
    "accountingTransactionId": "d18926c3-c4f1-4e1e-94a6-e47c2e5afa0d"
  },
  "action": "UPDATE"
}
```
**Response**

| Field Name                    | Type                                        | Description                                        |
|-------------------------------|---------------------------------------------|----------------------------------------------------|
| adjustedAccountingEntry       | [EntryModel](transaction_objects.md#)       | An entry model with the price changes applied      |
| adjustedAccountingTransaction | [TransactionModel](transaction_objects.md#) | A transaction model with the price changes applied |


**Example**

Accounting Entry Response

```json
{
  "success": true,
  "data": {
    "adjustedAccountingEntry": {
      "id": "6b2b82c2-3423-4905-966a-d66811e0b88a",
      "accountingTransactionId": "42d6b8b2-da39-4113-9b84-7d28c466184a",
      "count": 2,
      "quantity": 1,
      "unit": "ks",
      "accountingEntryState": "CREATED",
      "accountingEntryType": "SALE",
      "priceAmount": 2.50,
      "priceAdjustCoef": 0.2,
      "txAdjustCoef": 1,
      "priceCurrency": "EUR",
      "title": "Croissant plnený šunkou a syrom",
      "accountableItemId": "cbceb73a-25e7-4b83-a15d-c3cef5ef5f32",
      "createTime": "11.10.2022 14:50:17",
      "seat": 1
    }
  }
}
```

Accounting Transaction Response

```json
{
  "success": true,
  "data": {
    "adjustedAccountingTransaction": {
      "id": "d18926c3-c4f1-4e1e-94a6-e47c2e5afa0d",
      "area": {
        "id": 3,
        "numberOfSeats": 0
      },
      "type": "INVOICE",
      "createTime": "12.10.2022 11:03:30",
      "priceGross": 0,
      "accountingTransactionState": "OPENED",
      "transactionPriceAdjustmentCoefficient": 0.2
    }
  }
}
```