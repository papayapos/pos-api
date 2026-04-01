# POST /api/v1/transaction/payment

Close a transaction by submitting the entries to be paid and the payment method(s) covering them.

**Auth:** `Authorization: Bearer <token>`

Sections: [UPDATE](#update)

---

## UPDATE

Pay for a transaction. Provide the entries that make up the bill and the payments that cover it. The sum of `payments[].amount` should equal the total of the provided entries.

### Request fields

| Field | Type | Required | Description |
|---|---|:---:|---|
| `transactionId` | string (UUID) | yes | ID of the transaction to pay |
| `accountingEntries` | object[] | yes | Entries (line items) that form the bill. Use the entry objects from [GET /accountingEntry](accountingEntry.md). **Important:** the API returns `createTime` as a Unix timestamp in milliseconds, but this endpoint expects it as a formatted string `"dd.MM.yyyy HH:mm:ss"`. Convert before sending. |
| `payments` | object[] | yes | One or more payments covering the total. See [TransactionPaymentModel](transaction_objects.md). |
| `invoiceId` | string | no | Invoice number to print on the invoice |

**Payment object:**

| Field | Type | Required | Description |
|---|---|:---:|---|
| `paymentType` | string | yes | Payment method. See [PaymentTypeApiEnum](transaction_objects.md). |
| `amount` | number | yes | Amount to pay with this payment method |
| `cardData` | object | no | Card terminal data. Required for `CARD` payments. See [CardDataModel](transaction_objects.md). |

### Request examples

Cash payment:

```json
{
  "action": "UPDATE",
  "data": {
    "transactionId": "3d25d1e3-1da3-41cc-a373-129d7af28d30",
    "accountingEntries": [
      {
        "id": "8673a8ad-c4ae-40e8-ba44-c402298cab03",
        "accountingTransactionId": "3d25d1e3-1da3-41cc-a373-129d7af28d30",
        "accountableItemId": "cbceb73a-25e7-4b83-a15d-c3cef5ef5f32",
        "accountingEntryState": "CREATED",
        "accountingEntryType": "SALE",
        "title": "Croissant plnený šunkou a syrom",
        "count": 12,
        "quantity": 1,
        "unit": "ks",
        "priceAmount": 2.5,
        "priceAdjustCoef": 1,
        "txAdjustCoef": 1,
        "priceCurrency": "EUR",
        "createTime": "11.10.2022 12:25:40"
      }
    ],
    "payments": [
      {
        "paymentType": "CASH",
        "amount": 30
      }
    ]
  }
}
```

Card payment:

```json
{
  "action": "UPDATE",
  "data": {
    "transactionId": "dcfb7004-9649-4d92-bab1-261062993381",
    "accountingEntries": [
      {
        "id": "7e74dbf6-df47-4858-830a-a0d5be1f09a0",
        "accountingTransactionId": "dcfb7004-9649-4d92-bab1-261062993381",
        "accountableItemId": "cbceb73a-25e7-4b83-a15d-c3cef5ef5f32",
        "accountingEntryState": "CREATED",
        "accountingEntryType": "SALE",
        "title": "Croissant plnený šunkou a syrom",
        "count": 12,
        "quantity": 1,
        "unit": "ks",
        "priceAmount": 2.5,
        "priceAdjustCoef": 1,
        "txAdjustCoef": 1,
        "priceCurrency": "EUR",
        "createTime": "11.10.2022 11:58:46"
      }
    ],
    "payments": [
      {
        "paymentType": "CARD",
        "amount": 30,
        "cardData": {
          "transactionId": "dcfb7004-9649-4d92-bab1-261062993381"
        }
      }
    ]
  }
}
```

### Response

`data.newTransaction` — the closed/updated transaction object. See [TransactionModel](transaction_objects.md).

`data.payments` — array of payment records. See [TransactionPaymentModel](transaction_objects.md).

### Code examples

HTTPie — cash payment:
```bash
http POST $BASE_URL/api/v1/transaction/payment \
  "Authorization:Bearer $TOKEN" \
  action=UPDATE \
  data:='{"transactionId":"3d25d1e3-1da3-41cc-a373-129d7af28d30","accountingEntries":[{"id":"8673a8ad-c4ae-40e8-ba44-c402298cab03","accountingTransactionId":"3d25d1e3-1da3-41cc-a373-129d7af28d30","accountableItemId":"cbceb73a-25e7-4b83-a15d-c3cef5ef5f32","accountingEntryState":"CREATED","accountingEntryType":"SALE","title":"Croissant","count":12,"quantity":1,"unit":"ks","priceAmount":2.5,"priceAdjustCoef":1,"txAdjustCoef":1,"priceCurrency":"EUR","createTime":"11.10.2022 12:25:40"}],"payments":[{"paymentType":"CASH","amount":30}]}'
```

Kotlin — cash payment:
```kotlin
val body = """
    {
        "action": "UPDATE",
        "data": {
            "transactionId": "3d25d1e3-1da3-41cc-a373-129d7af28d30",
            "accountingEntries": [{
                "id": "8673a8ad-c4ae-40e8-ba44-c402298cab03",
                "accountingTransactionId": "3d25d1e3-1da3-41cc-a373-129d7af28d30",
                "accountableItemId": "cbceb73a-25e7-4b83-a15d-c3cef5ef5f32",
                "accountingEntryState": "CREATED",
                "accountingEntryType": "SALE",
                "title": "Croissant",
                "count": 12,
                "quantity": 1,
                "unit": "ks",
                "priceAmount": 2.5,
                "priceAdjustCoef": 1,
                "txAdjustCoef": 1,
                "priceCurrency": "EUR",
                "createTime": "11.10.2022 12:25:40"
            }],
            "payments": [{
                "paymentType": "CASH",
                "amount": 30
            }]
        }
    }
""".trimIndent().toRequestBody("application/json".toMediaType())

OkHttpClient().newCall(
    Request.Builder()
        .url("$BASE_URL/api/v1/transaction/payment")
        .addHeader("Authorization", "Bearer $TOKEN")
        .post(body)
        .build()
).execute().body?.string()
```

### Response examples

Cash payment response:

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

Card payment response:

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
