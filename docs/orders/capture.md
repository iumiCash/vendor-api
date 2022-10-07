# Capture order

If the vendor missed capture from iumiCash for some reason, 
then he can independently ask iumiCash about the status of the order.

iumiCash understands that it was a certain vendor who asked and marks this order as `notified` and stops trying to reach [`callback_url`](callback.md).

## Capture order API

Capture an order

`POST /api/v1/orders/<order_id>/capture/`


### Path parameters

???+ info "Path parameters"

    `order_id` *[ID][identifier]*
    :    iumiCash order identifier.


### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    To make REST API calls, include the bearer token in this header with the `Bearer` authentication scheme. The value is `Bearer <Access-Token>`

    `Content-Type` *string* **required**
    :    The media type. Required for operations with a request body. The value is `application/<format>`, where format is `json`.


### Response

???+ success "Response"

    `id` *UUID* **unique**
    
    :    iumiCash identifier. Using to unique identify resourses (in this example, order etc.)

    `external_id` *string* **unique**

    :    Unique identifier of your system. 

    `created_at` *datetime*
    
    :    Created time of the order in ISO format.

    `updated_at` *datetime*
    
    :    Updated time of the order in ISO format.

    `status` *enum* 
    
    :    Order status. Can be one of follows:
    
         * `created`
         * `approved`
         * `captured`
         * `cancelled`
         * `completed`

    `items` *list of [*item object*](#item)*

    :    Order items in the order.

    `application_context` [*object*](#application_context)

    :    Callback/redirect URIs that used during payment process.

    `links` *list of [*HATEOAS links*](#hateoas)*
    
    :    An array of request-related HATEOAS links. For example, 
         to complete payer approval, use `approve` link to redirect the payer.


### Examples

???+ example "Examples"

    === "Request"

        Example request with cURL. You can make this request in any programming language.

        ```bash
        curl -v -X POST https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/capture/ \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer <Access-Token>"'
        ```

    === "Response"

        A successful request returns the `HTTP 201 Created` status code and a JSON response body.

        ```bash
        {
          "id": "42481508-af81-43b9-82dd-d47d9e040ece",
          "external_id": "123456",
          "created_at": "2022-09-12T13:27:09.860466",
          "updated_at": "2022-09-12T13:27:09.860466",
          "status": "created",
          "items": [
            {
              "name": "T-shirt",
              "description": "Brand new T-shirt",
              "amount": {
                "value": 100,
                "currency_code": "USD"
              },
              "count": 1
            }
          ],
          "application_context": {
            "callback_url": "https://starlink.com/api/v1/iumiCash/callback/"
            "success_url": "https://starlink.com/success.html",
            "error_url": "https://starlink.com/error.html"
          },
          "links": [
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "self",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/checkout/42481508-af81-43b9-82dd-d47d9e040ece/",
              "rel": "approve",
              "method": "get"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/capture/",
              "rel": "capture",
              "method": "post"
            },
            {
              "link": "https://api.iumi.cash/api/v1/orders/42481508-af81-43b9-82dd-d47d9e040ece/cancel/",
              "rel": "cancel",
              "method": "post"
            }
          ]
        }
        ```


[idempotency]: ../idempotency.md
[client secret]: ../authentication/vendor_registration.md
[callback data]: ../orders/callback.md
[identifier]: ../types.md#iumicash-identifier