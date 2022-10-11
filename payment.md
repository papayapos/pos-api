**Endpoint: /api/v1/transaction/payment**

[UPDATE](#UPDATE)

### UPDATE

For updating payment status, contents, and distribution.

**Request data**

| Field Name        | Type                               | Description                                                                        | Required |
|-------------------|------------------------------------|------------------------------------------------------------------------------------|----------|
| transactionId     | UUID                               | Unique ID of the Transaction that the payment is going to be created for           | Yes      |
| accountingEntries | List\<[EntryModel](transaction_objects.md#)>    | The entries that will compose the contents of the payment                          | Yes      |
| payments          | List\<[TransactionPaymentModel](transaction_objects.md#)> | The series of payments which as a whole will cover the bill created by the entries | Yes      |
| invoiceId         | String                             | The id printed on the invoice                                                      | No       |

**Example**

Card payment with the API
```json
{
  "action": "UPDATE",
  "data": {
    "accountingEntries": [
      {
        "priceAdjustCoef": 1,
        "quantity": 1,
        "accountingEntryState": "CREATED",
        "count": 12,
        "txAdjustCoef": 1,
        "title": "Croissant plnený šunkou a syrom",
        "accountingTransactionId": "dcfb7004-9649-4d92-bab1-261062993381",
        "seat": 1,
        "unit": "ks",
        "priceCurrency": "EUR",
        "accountableItemId": "cbceb73a-25e7-4b83-a15d-c3cef5ef5f32",
        "createTime": "11.10.2022 11:58:46",
        "id": "7e74dbf6-df47-4858-830a-a0d5be1f09a0",
        "priceAmount": 2.5,
        "accountingEntryType": "SALE"
      }
    ],
    "payments": [
      {
        "cardData": {
          "transactionId": "dcfb7004-9649-4d92-bab1-261062993381"
        },
        "amount": 30,
        "paymentType": "CARD"
      }
    ],
    "transactionId": "dcfb7004-9649-4d92-bab1-261062993381"
  }
}
```
Cash payment with API

```json
{
  "action": "UPDATE",
  "data": {
    "accountingEntries": [
      {
        "priceAdjustCoef": 1,
        "quantity": 1,
        "accountingEntryState": "CREATED",
        "count": 12,
        "txAdjustCoef": 1,
        "title": "Croissant plnený šunkou a syrom",
        "accountingTransactionId": "3d25d1e3-1da3-41cc-a373-129d7af28d30",
        "seat": 1,
        "unit": "ks",
        "priceCurrency": "EUR",
        "accountableItemId": "cbceb73a-25e7-4b83-a15d-c3cef5ef5f32",
        "createTime": "11.10.2022 12:25:40",
        "id": "8673a8ad-c4ae-40e8-ba44-c402298cab03",
        "priceAmount": 2.5,
        "accountingEntryType": "SALE"
      }
    ],
    "payments": [
      {
        "amount": 30,
        "paymentType": "CASH"
      }
    ],
    "transactionId": "3d25d1e3-1da3-41cc-a373-129d7af28d30"
  }
}
```

**Response**

| Field Name     | Type                                                                      | Description                                                                     |
|----------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| newTransaction | List\<[TransactionModel](transaction_objects.md#)>        | A list of transaction data provided proportionally to the received entry models |
| payments       | List\<[TransactionPaymentModel](transaction_objects.md#)> | A list of payment data provided proportionally to the received payment models   |

**Example**
Cash response

```json
{
  "success": true,
  "data": {
    "newTransaction": {
      "id": "acfcad66-489c-4d31-92a7-0e39e627e2c9",
      "type": "INVOICE",
      "createTime": "11.10.2022 12:31:49",
      "priceGross": 30.00,
      "accountingTransactionState": "OPENED",
      "createdByUserId": 1,
      "transactionPriceAdjustmentCoefficient": 1
    },
    "payments": [
      {
        "accountingTransactionId": "acfcad66-489c-4d31-92a7-0e39e627e2c9",
        "paymentType": "CASH",
        "amount": 30
      }
    ]
  }
}
```
Card response

```json
{
  "success": true,
  "data": {
    "newTransaction": {
      "id": "40d0746b-b490-4630-bd42-30d8904176f0",
      "type": "INVOICE",
      "createTime": "11.10.2022 12:33:25",
      "priceGross": 30.00,
      "accountingTransactionState": "OPENED",
      "createdByUserId": 1,
      "transactionPriceAdjustmentCoefficient": 1
    },
    "payments": [
      {
        "accountingTransactionId": "40d0746b-b490-4630-bd42-30d8904176f0",
        "paymentType": "CARD",
        "amount": 30
      }
    ]
  }
}
```



