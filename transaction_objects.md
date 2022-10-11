
**Card Data Model**

| Field Name             | Type    | Description                                                    |
|------------------------|---------|----------------------------------------------------------------|
| id                     | UUID    | The Id of the card data model                                  |
| operationResult        | String  | The report string for a successful or unsuccessful payment     |
| fullName               | String  | The full name of the customer                                  |
| cardNumber             | String  | The number of the credit or debit card being used              |
| cardType               | String  | The type of card being used (Visa,Mastercard,etc.)             |
| cardPaymentType        | String  | The method of payment being used (CONTACTLESS,etc.)            |
| createTime             | Date    | The date when the payment was created by the user              |
| terminalType           | String  | The type of terminal the payment was made from                 |
| terminalId             | String  | The id of the specific terminal creating the payment           |
| authorizationCode      | String  | A verification string                                          |
| applicationId          | String  | The Id that identifies the terminal and it's software revision |
| panSequence            | String  | Sequential identifier for the card transaction.                |
| tvr                    | String  | Extra data given by the payment processor                      |
| tvi                    | String  | Extra data given by the payment processor                      |
| cvr                    | String  | Extra data given by the payment processor                      |
| iad                    | String  | Extra data given by the payment processor                      |
| avc                    | String  | Extra data given by the payment processor                      |
| act                    | String  | Extra data given by the payment processor                      |
| arc                    | String  | Extra data given by the payment processor                      |
| transactionId          | String  | The Id of the accounting transaction                           |
| variableSymbol         | String  | Bank transaction tracking identifier                           |
| paymentReceipt         | String  | The receipt formatted by the payment terminal for the customer | 
| paymentMerchantReceipt | String  | The receipt formatted by the payment terminal for the merchant |
| hostTransactionId      | String  | The Id returned by the terminal for the payment                |
| sequenceNumber         | String  | The number of transactions completed by the terminal           |
| transactionCreateDate  | Date    | The returned terminal creation time for the payment            |

**TransactionPaymentModel**

| Field Name              | Type               | Description                                                    |
|-------------------------|--------------------|----------------------------------------------------------------|
| id                      | UUID               | The Id of the payment                                          |
| accountingTransactionId | UUID               | The Id of the transaction which is associated with the payment |
| paymentType             | PaymentTypeApiEnum | The payment method used by the customer (CASH,CARD,etc.)       |
| amount                  | BigDecimal         | The amount of money to be paid                                 |
| nominalCompensation     | BigDecimal         | Rounding to the nearest X cent                                 |
| cardDataId              | UUID               | Id of the card data being submitted                            |
| cardData                | CardDataModel      | Debit or credit card information                               |

**PaymentTypeApiEnum**

| Payment Methods  |
|------------------|
| CASH             |
| CARD             |
| GASTRO_CHECK     |
| OPERATING_COST   |
| HOTEL_SYSTEM     |
| BANK_TRANSFER    |
| CALLIO_CARD      |
| VOUCHER          |

**Entry Model**

| Field Name              | Type                         | Description                                                                         |
|-------------------------|------------------------------|-------------------------------------------------------------------------------------|
| id                      | UUID                         | Entry model Id                                                                      |
| accountingTransactionId | UUID                         | The Id of the accounting transaction that the model is supposed to be associated to |
| count                   | BigDecimal                   | The amount of items requested                                                       |
| quantity                | BigDecimal                   | The quantity amount provided per item                                               |
| unit                    | String                       | The quantity measure for the item (g,kg,l,etc.)                                     |
| accountingEntryState    | AccountingEntryStateApiEnum  | The state of the accounting entry (CREATED,CANCELED)                                |
| accountingEntryType     | String                       | The type of transaction being created for processing (INVOICE,SALE)                 |
| vatRateId               | Long                         | ID of the vat multiplier                                                            |
| vatRateValue            | BigDecimal                   | Multiplier to increase the price by a certain percentage (0.2 = price*1.2)          |
| priceAmount             | BigDecimal                   | Total price of the transaction                                                      |
| priceAdjustCoef         | BigDecimal                   | Discount multiplier for this specific item (0.8 = price*0.8)                        |
| txAdjustCoef            | BigDecimal                   | Discount multiplier for the whole transaction (0.8 = price*0.8)                     |
| adjustedPrice           | BigDecimal                   | The resulting price from the discount adjustments                                   |
| priceCurrency           | String                       | Currency used for the transaction                                                   |
| title                   | String                       | The name that is shown on the receipt                                               |
| fiscalNote              | String                       | The custom text printed on the receipt by a fiscal printer                          |
| bonNote                 | String                       | The custom text printed on the receipt                                              |
| accountableItemId       | UUID                         | The Id of item that is being sold                                                   |
| createTime              | Date                         | The date that the transaction was created                                           |

**TransactionModel**

| Field Name                            | Type                              | Description                                                        |
|---------------------------------------|-----------------------------------|--------------------------------------------------------------------|
| id                                    | UUID                              | Unique accounting transaction Id                                   |
| area                                  | AreaModel                         | The model to describe the area (table) assigned to the transaction |
| type                                  | AccountingTransactionTypeApiEnum  | The type of transaction (?)                                        |
| createTime                            | Date                              | The date the transaction was created                               |
| closeTime                             | Date                              | The date the transaction was closed                                |
| priceGross                            | BigDecimal                        | Price with vat                                                     |
| priceNet                              | BigDecimal                        | Price without vat                                                  |
| priceVat                              | BigDecimal                        | The vat itself (priceGross - priceNet)                             |
| accountingTransactionState            | AccountingTransactionStateApiEnum | The current state of the transaction                               |                                                          |
| transactionPriceAdjustmentCoefficient | BigDecimal                        | The coefficient that the transaction was discounted by             |