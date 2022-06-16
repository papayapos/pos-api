**Endpoint Summary**




| Endpoint                                 | Description                                     |
| ---------------------------------------- | ----------------------------------------------- |
| [/api/v1/inventory](inventory.md#)       | CRUD operations for inventories                 |
| [/api/v1/inventory/card](card.md#)       | Receive or issue the stock with inventory cards |
| [/api/v1/inventory/item](item.md#)       | Work with stock items                           |
| /api/v1/inventory/recipe                 | Create and assign recipes to menu items         |
| /api/v1/inventory/movement (todo define) | Read inventory movements not grouped in cards   |
| [/api/v1/supplier](supplier.md#)         | CRUD for supplier                               |
| /api/v1/order                            | Create order to POS                             |
| /api/v1/                                 |                                                 |


**Sending request**

Currently only way to send request is to use Http POST request on correct endpoint with body containing `action` and `data`.

**Request**

All requests must contain `action` and `request`. Currently there are 3 defined actions for get, create or update, and delete.

In case you want to send empty `data`, you need to put in empty json. In case when `data` is missing in request, API will respond with error and request will be ignored.

**Actions**

| action | description |
| :---   | :---          |
| GET | Fetch data, may use data in request to narrow results |
| UPDATE | Create or update with data provided in request |
| DELETE | Delete objects using data provided in request |

**Example**

You can find examples for each endpoint and action in wiki. This is example of empty `data` with GET `action`.

```json

{
  "action": "GET",
  "data": {}
}
```
