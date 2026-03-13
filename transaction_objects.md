# Transaction Object Reference

Shared object definitions used across transaction endpoints.

---

## TransactionModel

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Unique transaction ID |
| `type` | string | Transaction type. See [AccountingTransactionTypeApiEnum](#accountingtransactiontypeapienum). |
| `area` | object | Area (table) assigned to the transaction |
| `createTime` | string | Date/time the transaction was created |
| `closeTime` | string | Date/time the transaction was closed (if applicable) |
| `priceGross` | number | Total price including VAT |
| `priceNet` | number | Total price excluding VAT |
| `priceVat` | number | VAT amount (`priceGross - priceNet`) |
| `accountingTransactionState` | string | Current state. See [AccountingTransactionStateApiEnum](#accountingtransactionstateapienum). |
| `transactionPriceAdjustmentCoefficient` | number | Transaction-wide price multiplier (1 = no change, 0.8 = 20% discount) |

---

## EntryModel

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Entry ID |
| `accountingTransactionId` | string (UUID) | ID of the parent transaction |
| `accountableItemId` | string (UUID) | ID of the catalog item sold |
| `title` | string | Item name shown on the receipt |
| `fiscalNote` | string | Custom text printed by a fiscal printer |
| `bonNote` | string | Custom text printed on a kitchen ticket |
| `accountingEntryState` | string | Entry state (`CREATED`, `CANCELED`) |
| `accountingEntryType` | string | Entry type. See [AccountingEntryTypeApiEnum](#accountingentrytypeapienum). |
| `count` | number | Number of items (e.g. `2` croissants) |
| `quantity` | number | Quantity per item in the item's measuring unit (informational) |
| `unit` | string | Measuring unit (informational) |
| `priceAmount` | number | Original unit price |
| `priceAdjustCoef` | number | Entry-level discount multiplier |
| `txAdjustCoef` | number | Transaction-level discount multiplier |
| `adjustedPrice` | number | Final price after all adjustments |
| `priceCurrency` | string | Currency. See [CurrencyApiEnum](#currencyapienum). |
| `vatRateValue` | number | VAT multiplier (e.g. `0.2` means price × 1.2) |
| `createTime` | string | Date/time the entry was created |

---

## TransactionPaymentModel

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Payment ID |
| `accountingTransactionId` | string (UUID) | ID of the transaction this payment belongs to |
| `paymentType` | string | Payment method. See [PaymentTypeApiEnum](#paymenttypeapienum). |
| `amount` | number | Amount paid |
| `nominalCompensation` | number | Rounding adjustment (to nearest cent) |
| `cardDataId` | string (UUID) | ID of the associated card data (if card payment) |
| `cardData` | object | Card terminal data. See [CardDataModel](#carddatamodel). |

---

## CardDataModel

| Field | Type | Description |
|---|---|---|
| `id` | string (UUID) | Card data record ID |
| `transactionId` | string | Transaction ID from the terminal |
| `operationResult` | string | Result string from the payment terminal |
| `fullName` | string | Cardholder name |
| `cardNumber` | string | Masked card number |
| `cardType` | string | Card brand (e.g. `Visa`, `Mastercard`) |
| `cardPaymentType` | string | Payment method (e.g. `CONTACTLESS`) |
| `terminalType` | string | Terminal type |
| `terminalId` | string | Terminal ID |
| `authorizationCode` | string | Authorization code |
| `applicationId` | string | Terminal application ID and software revision |
| `panSequence` | string | Sequential card transaction identifier |
| `tvr` | string | Terminal verification result (from payment processor) |
| `tvi` | string | Additional terminal data (from payment processor) |
| `cvr` | string | Card verification result (from payment processor) |
| `iad` | string | Issuer application data (from payment processor) |
| `avc` | string | Application version check (from payment processor) |
| `act` | string | Application cryptogram type (from payment processor) |
| `arc` | string | Authorization response code (from payment processor) |
| `variableSymbol` | string | Bank transaction tracking identifier |
| `paymentReceipt` | string | Customer receipt formatted by the payment terminal |
| `paymentMerchantReceipt` | string | Merchant receipt formatted by the payment terminal |
| `hostTransactionId` | string | Transaction ID returned by the terminal host |
| `sequenceNumber` | string | Number of transactions completed by this terminal |
| `createTime` | string | Date/time the payment was created |
| `transactionCreateDate` | string | Transaction creation time returned by the terminal |

---

## AccountingTransactionStateApiEnum

| Value | Description |
|---|---|
| `OPENED` | Transaction is open and can accept entries |
| `LOCKED` | Transaction is locked |
| `NOT_PRINTED` | Transaction closed but not yet printed |
| `CANCELED` | Transaction was cancelled |
| `CLOSED` | Transaction is closed and paid |
| `EXPORTED` | Transaction has been exported |
| `CLOSED_EDIT` | Transaction is closed but still editable |

## AccountingTransactionTypeApiEnum

| Value | Description |
|---|---|
| `RECEIPT` | Fiscal receipt |
| `INVOICE` | Invoice |

## AccountingEntryTypeApiEnum

| Value | Description |
|---|---|
| `SALE` | Regular sale |
| `REFUND` | Refund of a sale |
| `DISCOUNT_FOR_ENTRY` | Discount applied to a single entry |
| `SURCHARGE_FOR_ENTRY` | Surcharge applied to a single entry |
| `DISCOUNT_FOR_TX` | Discount applied to the entire transaction |
| `SURCHARGE_FOR_TX` | Surcharge applied to the entire transaction |
| `VOUCHER` | Voucher redemption |

## PaymentTypeApiEnum

| Value |
|---|
| `CASH` |
| `CARD` |
| `GASTRO_CHECK` |
| `OPERATING_COST` |
| `HOTEL_SYSTEM` |
| `BANK_TRANSFER` |
| `CALLIO_CARD` |
| `VOUCHER` |
| `QERKO` |

## CurrencyApiEnum

| Value |
|---|
| `CZK` |
| `EUR` |
| `HUF` |
| `GBP` |
| `IDR` |
