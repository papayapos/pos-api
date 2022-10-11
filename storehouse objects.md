**stock item group**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| id | number | long, id in database |
| name | string | name |
| type | number | Type of the group, currently supported values are 1 = alcohol, 2 = other |
| items | array of stock item objects | items belonging to group |

**Stock item**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| id | number | long, Id of stock item in Papaya.|
| title | string | Name of item |
| amount | number | Amount of item in storehouse |
| priceNetAverage | number | Average net price of item, calculated from stock change cards |
| priceFixed | number | price of item, set by user |
| vatRate | number | vat rate of item |
| measuringUnit | string | measuring unit of item |
| secondaryMeasuringUnit | string | secondary measuring unit |
| unitCoefficient | number | conversion coefficient between primary and secondary unit |
| productCode | string | product code |
| minimalStockAmount | number | minimal stock amount, used in reports |

**recipe**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| accountableItemId | string | UUID of accountable item |
| items | array of recipe item objects | items of recipe |


**recipe item** 

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| stockItem | stock item object | stock item object associated with recipe |
| amountUsed | number | amount of item used in recipe |
| unit | number | unit used in recipe, 1 = primary, 2 = secondary | 

**Stock change card** 

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| id | number | long, id of card in database |
| storehouseId | number | long, storehouse id |
| createTime | string | create time, |
| placedTime | string | placed time, plays role in stock taking process which is currently not part of API, use same time as create time for now |
| accType | number | type of stock change card, currently 1 = Operating costs, 2 = SALE, 3 = Refund of operating costs, 4 = Refund of sale, 5 = Stock taking receipt, 6 = Stock taking invoice, stock taking is currently not part of API|
| documentType | number | type of bill associented with card, 1 = Fiscal receipt, 2 = invoice, 3 = other |
| documentId | string | id of document associated with card, for example accounting transaction |
| type | string | type of change, RECEIPT_CARD = when goods are added to storehouse, ISSUE_CARD = when goods are removed from storehouse |
| supplier | supplier object| supplier|
| orderNumber | string | order number|
| deliveryNote | string | delivery note |
| note | string | another note |

**supplier**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| name | string | name |
| address| string | address|
| city | string | city|
| country | string | country |
| ico | string | ico |
| dic | string | dic  |
| icDph | string | icdph |
| contactPerson | string | name of someone responsible |
| phone | string | phone | 
| email | string | email | 
| registration| string | registration |
| registrationNumber | string | registration number |
| registrationDate | string | registration date |

**Stock change card item**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| id | number | long, Id of stock change card item in Papaya.|
| amount | number | amount added/subtracted |
| priceNet | number | net price |
| vatRate | number | vat rate |
| stockItemId | number | long, stock item id |
| stockItemTitle | string | title of item |
| measuringUnit | string | measuring unit |
| stockChangeCardId | number | long, id of stock change card this item belongs to |
| entryId | string | id of accounting entry, UUID |

**Storehouse**

| field name  | type        | Description   |
| :---        |    :----:   | :---          |
| id | number | long, id of storehouse in database |
| name | string | name of the storehouse|
| description | string | description of the storehouse |
| groups | array of stock item group objects | Current state of storehouse. This object is only useful for get requests. In post and delete requests this field is ignored. Each group contains stock items. |


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
