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
```json
{
  "action":"UPDATE",
  "data": {
    "accountingEntryId": "6b2b82c2-3423-4905-966a-d66811e0b88a",
    "discountRate": 0.8
  }
}
```
**Response**

| Field Name                    | Type             | Description                                        |
|-------------------------------|------------------|----------------------------------------------------|
| adjustedAccountingEntry       | EntryModel       | An entry model with the price changes applied      |
| adjustedAccountingTransaction | TransactionModel | A transaction model with the price changes applied |


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