# Request Examples

## Scenario 1: Receipt with 2 items, paid by cash

### Step 1 — Create transaction

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionType": "RECEIPT",
    "areaId": 9,
    "salePlaceId": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
  }
}
```

Response returns `data.accountingTransactions[0].id` — use it as `<TX_ID>` in the steps below.

---

### Step 2a — Add first entry

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionId": "<TX_ID>",
    "accountableItemId": "b6aeb536-5829-41f1-ab86-aba8f69d614b",
    "count": 2,
    "price": 3.50,
    "state": "CREATED",
    "type": "SALE"
  }
}
```

Response returns `data.accountingEntries[0]` — save the full entry object as `<ENTRY_1>`.

---

### Step 2b — Add second entry

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionId": "<TX_ID>",
    "accountableItemId": "a1b2c3d4-0000-0000-0000-111111111111",
    "count": 1,
    "price": 5.00,
    "state": "CREATED",
    "type": "SALE"
  }
}
```

Response returns `data.accountingEntries[0]` — save the full entry object as `<ENTRY_2>`.

---

### Step 3 — Pay by cash

Use the full entry objects returned in steps 2a and 2b. The `amount` must equal the sum of all entries.

```json
{
  "action": "UPDATE",
  "data": {
    "transactionId": "<TX_ID>",
    "accountingEntries": [
      <ENTRY_1>,
      <ENTRY_2>
    ],
    "payments": [
      {
        "paymentType": "CASH",
        "amount": 12.00
      }
    ]
  }
}
```

---

## Scenario 2: Invoice with 1 item, paid by card

### Step 1 — Create transaction

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionType": "INVOICE",
    "invoiceNumber": "INV-2024-001",
    "salePlaceId": "a25805ba-dd76-4c08-8188-5e64bc2e1645"
  }
}
```

Response returns `data.accountingTransactions[0].id` — use it as `<TX_ID>`.

---

### Step 2 — Add entry

```json
{
  "action": "UPDATE",
  "data": {
    "accountingTransactionId": "<TX_ID>",
    "accountableItemId": "b6aeb536-5829-41f1-ab86-aba8f69d614b",
    "count": 1,
    "price": 20.00,
    "state": "CREATED",
    "type": "SALE"
  }
}
```

Response returns `data.accountingEntries[0]` — save the full entry object as `<ENTRY_1>`.

---

### Step 3 — Pay by card

```json
{
  "action": "UPDATE",
  "data": {
    "transactionId": "<TX_ID>",
    "invoiceId": "INV-2024-001",
    "accountingEntries": [
      <ENTRY_1>
    ],
    "payments": [
      {
        "paymentType": "CARD",
        "amount": 20.00,
        "cardData": {
          "transactionId": "<TX_ID>"
        }
      }
    ]
  }
}
```
