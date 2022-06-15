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
