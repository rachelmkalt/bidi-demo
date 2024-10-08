---
title: Integrate Webhook
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The system will send a webhook with detailed information about the transaction once the payment is completed. This information is crucial for updating the reservation status in your system.

Below, we will explain some integration examples for testing or, alternatively, for connecting with a platform like Pipedream.

# Webhook JSON Structure

This document provides a detailed description of the JSON for a webhook event.

## Example JSON

```json Structure
{
  "uuid": "string",
  "reference": "string",
  "booking_number": "string | null",
  "amount": "number",
  "fee": "number",
  "currency": "string",
  "checkin": "string (format: DD/MM/YYYY)",
  "checkout": "string (format: DD/MM/YYYY)",
  "description": "string",
  "customer_info": {
    "first_name": "string",
    "last_name": "string",
    "email": "string",
    "ip": "string",
    "phone": "string",
    "user_agent": "string",
    "browser_info": {
      "language": "string",
      "color_depth": "string",
      "java_enabled": "boolean",
      "screen_width": "number",
      "accept_header": "string",
      "screen_height": "number",
      "java_script_enabled": "boolean"
    }
  },
  "items": [
    {
      "price": "number",
      "quantity": "number",
      "description": "string"
    }
  ],
  "payment_mode": "string",
  "order_status": "string",
  "payment_status": "string",
  "created_at": "string (format: DD/MM/YYYY HH:MM:SS)",
  "merchant": {
    "name": "string",
    "slug": "string",
    "logo": "string (HTML)",
    "billing_email": "string | null",
    "reservation_email": "string",
    "reservation_email_bcc": "string | null",
    "bg_color": "string (hex color)",
    "text_color": "string (hex color)",
    "address": "string",
    "address_phone": "string | null",
    "contact_info": "string (HTML)",
    "pictures": "array"
  },
  "transactions": [
    {
      "priority": "string",
      "kind": "string",
      "amount": "number",
      "card_address": {
        "address": "string",
        "city": "string",
        "country": "string",
        "country_code": "string",
        "zip": "string",
        "address2": "string | null",
        "state": "string"
      },
      "card": {
        "bin": "string",
        "type": "string",
        "brand": "string",
        "bank_name": "string",
        "last_4_digits": "string",
        "country_code": "string",
        "url_brand": "string (URL)"
      },
      "card_info": {
        "holder_name": "string",
        "expiration": "string (format: MM/YYYY)"
      },
      "installment": "string",
      "monthly": "string",
      "descriptor": "string"
    }
  ],
  "transaction_template": [
    {
      "BRAND": "string",
      "CARD_ICON": "string (URL)",
      "TYPE": "string",
      "LAST_4_DIGITS": "string",
      "HOLDER_NAME": "string",
      "MERCHANT_DESCRIPTOR": "string",
      "INSTALLMENT": "string",
      "MONTHLY": "string"
    }
  ],
  "holder_name_diff": "boolean",
  "payment_adds": {
    "date": "string (format: DD/MM/YYYY HH:MM)",
    "amount": "string"
  }
}
```
```json Example
{
  "uuid": "f5b07c64-a98a-4b9e-8fbc-9cef25cc7659",
  "reference": "f80f1431-1d94-44d0-ab30-b3a6b7d19d4e",
  "booking_number": null,
  "amount": 1000,
  "fee": 0,
  "currency": "MXN",
  "checkin": "05/07/2024",
  "checkout": "06/07/2024",
  "description": "Reserva de hotel",
  "customer_info": {
    "first_name": "Alex",
    "last_name": "Galindo",
    "email": "pruebas@retrypay.com",
    "ip": "177.249.170.25",
    "phone": "3311224455",
    "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0",
    "browser_info": {
      "language": "en-US",
      "color_depth": "1",
      "java_enabled": true,
      "screen_width": 564,
      "accept_header": "*/",
      "screen_height": 200,
      "java_script_enabled": true
    }
  },
  "items": [
    {
      "price": 1000,
      "quantity": 1,
      "description": "Reservation of hotel"
    }
  ],
  "payment_mode": "single",
  "order_status": "inactive",
  "payment_status": "paid",
  "created_at": "01/07/2024 20:56:04",
  "merchant": {
    "name": "Merchant Test",
    "slug": "merchant-test",
    "logo": "<img src=\"https://hotelpay-merchants.s3.us-east-2.amazonaws.com/logos/66673da2078ae\" alt=\"\" height=\"120\">",
    "billing_email": null,
    "reservation_email": "pagos@retrypay.com",
    "reservation_email_bcc": null,
    "bg_color": "#d16363",
    "text_color": "#ba6a6a",
    "address": "Tiburon, -, -, Guadalajara, Jalisco, CP 32400",
    "address_phone": null,
    "contact_info": "<p>Llámanos al</p>",
    "pictures": []
  },
  "transactions": [
    {
      "priority": "primary",
      "kind": "card",
      "amount": 1000,
      "card_address": {
        "address": "Av Hidalgo 1995",
        "city": "Guadalajara",
        "country": "México",
        "country_code": "MX",
        "zip": "123456",
        "address2": null,
        "state": "Jalisco"
      },
      "card": {
        "bin": "400000",
        "type": "credit",
        "brand": "visa",
        "bank_name": "Intl Hdqtrs-Center Owned",
        "last_4_digits": "0051",
        "country_code": "US",
        "url_brand": "https://testing.hotelpay.net/storage/img/brands/flats/visa.svg"
      },
      "card_info": {
        "holder_name": "Alex",
        "expiration": "12/2030"
      },
      "installment": "1",
      "monthly": "$1,000.00 MXN",
      "status": "capture",
      "descriptor": "HOTELPAY"
    }
  ],
  "transaction_template": [
    {
      "BRAND": "visa",
      "CARD_ICON": "https://testing.hotelpay.net/storage/img/brands/flats/visa.svg",
      "TYPE": "credit",
      "LAST_4_DIGITS": "0051",
      "HOLDER_NAME": "Alex",
      "MERCHANT_DESCRIPTOR": "HOTELPAY",
      "INSTALLMENT": "1",
      "MONTHLY": "$1,000.00 MXN"
    }
  ],
  "holder_name_diff": true,
  "payment_adds": {
    "date": "01/07/2024 20:56",
    "amount": "$1,000.00 MXN"
  }
}
```

## JSON Structure

***

| Field                 | Description                                                | Data Type     | Example                                           |
| --------------------- | ---------------------------------------------------------- | ------------- | ------------------------------------------------- |
| uuid                  | Universal unique identifier for the reservation.           | String        | f5b07c64-a98a-4b9e-8fbc-9cef25cc7659              |
| reference             | Internal reference for the reservation.                    | String        | f80f1431-1d94-44d0-ab30-b3a6b7d19d4e              |
| booking\_number       | Booking number (not available in this case).               | String        | null                                              |
| amount                | Total amount of the reservation in the specified currency. | Number        | 1000                                              |
| fee                   | Commission applied to the transaction.                     | Number        | 0                                                 |
| currency              | Currency used (Mexican Pesos).                             | String        | MXN                                               |
| checkin               | Hotel check-in date.                                       | String        | 05/07/2024                                        |
| checkout              | Hotel check-out date.                                      | String        | 06/07/2024                                        |
| description           | Description of the reservation.                            | String        | Hotel reservation                                 |
| customer\_info        | Customer information                                       | json          | [Customer Information](#customer-information)     |
| items                 | Room details                                               | array of json | Room details                                      |
| payment\_mode         | Payment mode (single in this case).                        | String        | single                                            |
| order\_status         | Order status (inactive).                                   | String        | inactive                                          |
| payment\_status       | Payment status (paid).                                     | String        | [Payment status](#payment-status)                 |
| created\_at           | Date and time of reservation creation.                     | String        | 01/07/2024 20:56:04                               |
| merchant              | Merchant information                                       | json          | [Merchant Information](#merchant-information)     |
| transactions          | Transactions                                               | array of json | [Transactions](#transactions)                     |
| transaction\_template | Transaction Template (Sendgrid)                            | array of json | [Transaction Template](#transaction-template)     |
| holder\_name\_diff    | Indicates if the cardholder's name is different.           | Boolean       | true                                              |
| payment\_adds         | Additional Information                                     | json          | [Additional Information](#additional-information) |

### Customer Information

| Field         | Description                                          | Data Type | Example                                                                                                                               |
| ------------- | ---------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| first\_name   | Customer's first name.                               | String    | Alex                                                                                                                                  |
| last\_name    | Customer's last name.                                | String    | Galindo                                                                                                                               |
| email         | Customer's email address.                            | String    | correo....                                                                                                                            |
| ip            | Customer's IP address.                               | String    | 177.249.170.25                                                                                                                        |
| phone         | Customer's phone number.                             | String    | 3311224455                                                                                                                            |
| user\_agent   | Customer's browser and operating system information. | String    | Mozilla/5.0 (Macintosh; Intel Mac OS X 10\_15\_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36 Edg/126.0.0.0 |
| browser\_info | Browser Information                                  | json      | [Customer Browser Information](#customer-browser-information)                                                                         |

### Customer Browser Information

<Table>
  <thead>
    <tr>
      <th>
        Field
      </th>

      <th>
        Description
      </th>

      <th>
        Data Type
      </th>

      <th>
        Example
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        language
      </td>

      <td>
        Browser language.
      </td>

      <td>
        String
      </td>

      <td>
        en-US
      </td>
    </tr>

    <tr>
      <td>
        color_depth
      </td>

      <td>
        Screen color depth.
      </td>

      <td>
        String
      </td>

      <td>
        1
      </td>
    </tr>

    <tr>
      <td>
        java_enabled
      </td>

      <td>
        Indicates if Java is enabled.
      </td>

      <td>
        Boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        screen_width
      </td>

      <td>
        Screen width in pixels.
      </td>

      <td>
        Number
      </td>

      <td>
        564
      </td>
    </tr>

    <tr>
      <td>
        accept_header
      </td>

      <td>
        Browser accept header.
      </td>

      <td>
        String
      </td>

      <td>
        \*

        /
      </td>
    </tr>

    <tr>
      <td>
        screen_height
      </td>

      <td>
        Screen height in pixels.
      </td>

      <td>
        Number
      </td>

      <td>
        200
      </td>
    </tr>

    <tr>
      <td>
        java_script_enabled
      </td>

      <td>
        Indicates if JavaScript is enabled.
      </td>

      <td>
        Boolean
      </td>

      <td>
        true
      </td>
    </tr>
  </tbody>
</Table>

### Room Details

| Field       | Description        | Data Type | Example           |
| ----------- | ------------------ | --------- | ----------------- |
| price       | Item price.        | Number    | 1000              |
| quantity    | Quantity of items. | Number    | 1                 |
| description | Item description.  | String    | Hotel reservation |

### Merchant Information

<Table>
  <thead>
    <tr>
      <th>
        Field
      </th>

      <th>
        Description
      </th>

      <th>
        Data Type
      </th>

      <th>
        Example
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        name
      </td>

      <td>
        Merchant's name.
      </td>

      <td>
        String
      </td>

      <td>
        Merchant Test
      </td>
    </tr>

    <tr>
      <td>
        slug
      </td>

      <td>
        Unique identifier for the merchant.
      </td>

      <td>
        String
      </td>

      <td>
        merchant-test
      </td>
    </tr>

    <tr>
      <td>
        logo
      </td>

      <td>
        Merchant's logo in HTML format.
      </td>

      <td>
        String
      </td>

      <td>
        <img src="&#x3C;https://hotelpay-merchants.s3.us-east-2.amazonaws.com/logos/66673da2078ae>" alt="" height="120" />
      </td>
    </tr>

    <tr>
      <td>
        billing_email
      </td>

      <td>
        Billing email address (may not be available).
      </td>

      <td>
        String
      </td>

      <td>
        email...
      </td>
    </tr>

    <tr>
      <td>
        reservation_email
      </td>

      <td>
        Email address for reservations.
      </td>

      <td>
        String
      </td>

      <td>
        email...
      </td>
    </tr>

    <tr>
      <td>
        reservation_email_bcc
      </td>

      <td>
        BCC email address for reservations (may not be available).
      </td>

      <td>
        String
      </td>

      <td>
        emails...
      </td>
    </tr>

    <tr>
      <td>
        bg_color
      </td>

      <td>
        Background color.
      </td>

      <td>
        String
      </td>

      <td>
        \# d16363
      </td>
    </tr>

    <tr>
      <td>
        text_color
      </td>

      <td>
        Text color.
      </td>

      <td>
        String
      </td>

      <td>
        \# ba6a6a
      </td>
    </tr>

    <tr>
      <td>
        address
      </td>

      <td>
        Merchant's address.
      </td>

      <td>
        String
      </td>

      <td>
        Tiburon, -, -, Guadalajara, Jalisco, CP 32400
      </td>
    </tr>

    <tr>
      <td>
        address_phone
      </td>

      <td>
        Address phone number (not available).
      </td>

      <td>
        String
      </td>

      <td>
        null
      </td>
    </tr>

    <tr>
      <td>
        contact_info
      </td>

      <td>
        Contact information in HTML format.
      </td>

      <td>
        String
      </td>

      <td>
        <p />

        Call us at

        <html xmlns="http://www.w3.org/1999/xhtml"><head /><body /></html>
      </td>
    </tr>

    <tr>
      <td>
        pictures
      </td>

      <td>
        Associated images (none available).
      </td>

      <td>
        Array
      </td>

      <td>
        \[

        ]
      </td>
    </tr>
  </tbody>
</Table>

### Transactions

| Field         | Description                              | Data Type | Example                                           |
| ------------- | ---------------------------------------- | --------- | ------------------------------------------------- |
| priority      | Transaction priority.                    | String    | primary                                           |
| kind          | Transaction type (card).                 | String    | card                                              |
| amount        | Transaction amount.                      | Number    | 1000                                              |
| card\_address | Card Address                             | json      | [Card Address](#card-address)                     |
| card          | Card Information                         | json      | [Card Information](#card-information)             |
| card\_info    | Cardholder Information                   | json      | [Cardholder Information](#cardholder-information) |
| installment   | Number of installments.                  | String    | 1                                                 |
| monthly       | Monthly payment.                         | String    | $1,000.00 MXN                                     |
| status        | Transaction status.                      | String    | capture                                           |
| descriptor    | Merchant description in the transaction. | String    | HOTELPAY                                          |

### Card Address

| Field         | Description                          | Data Type | Example         |
| ------------- | ------------------------------------ | --------- | --------------- |
| address       | Card address.                        | String    | Av Hidalgo 1995 |
| city          | Card city.                           | String    | Guadalajara     |
| country       | Card country.                        | String    | Mexico          |
| country\_code | Card country code.                   | String    | MX              |
| zip           | Card zip code.                       | String    | 123456          |
| address2      | Second address line (not available). | Null      | null            |
| state         | Card state.                          | String    | Jalisco         |

### Card Information

<Table>
  <thead>
    <tr>
      <th>
        Field
      </th>

      <th>
        Description
      </th>

      <th>
        Data Type
      </th>

      <th>
        Example
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        bin
      </td>

      <td>
        Card bin.
      </td>

      <td>
        String
      </td>

      <td>
        400000
      </td>
    </tr>

    <tr>
      <td>
        type
      </td>

      <td>
        Card type (credit).
      </td>

      <td>
        String
      </td>

      <td>
        credit
      </td>
    </tr>

    <tr>
      <td>
        brand
      </td>

      <td>
        Card brand.
      </td>

      <td>
        String
      </td>

      <td>
        visa
      </td>
    </tr>

    <tr>
      <td>
        bank_name
      </td>

      <td>
        Bank name.
      </td>

      <td>
        String
      </td>

      <td>
        \`

        Intl Hdqtrs-C
      </td>
    </tr>
  </tbody>
</Table>

### Cardholder Information

| Field        | Description           | Data Type | Example |
| ------------ | --------------------- | --------- | ------- |
| holder\_name | Cardholder name.      | String    | Alex    |
| expiration   | Card expiration date. | String    | 12/2030 |

### Transaction Template

| Field                | Description                | Data Type | Example                                                                                                                          |
| -------------------- | -------------------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| BRAND                | Card brand.                | String    | visa                                                                                                                             |
| CARD\_ICON           | Card icon.                 | String    | [https://testing.hotelpay.net/storage/img/brands/flats/visa.svg](https://testing.hotelpay.net/storage/img/brands/flats/visa.svg) |
| TYPE                 | Card type.                 | String    | credit                                                                                                                           |
| LAST\_4\_DIGITS      | Last 4 digits of the card. | String    | 0051                                                                                                                             |
| HOLDER\_NAME         | Cardholder's name.         | String    | Alex                                                                                                                             |
| MERCHANT\_DESCRIPTOR | Merchant description.      | String    | UNLIMIT                                                                                                                          |
| INSTALLMENT          | Number of installments.    | String    | 1                                                                                                                                |
| MONTHLY              | Monthly payment.           | String    | $1,000.00 MXN                                                                                                                    |

### Additional Information

| Field                | Description     | Data Type | Example          |
| -------------------- | --------------- | --------- | ---------------- |
| payment\_adds.date   | Payment date.   | String    | 01/07/2024 20:56 |
| payment\_adds.amount | Payment amount. | String    | $1,000.00 MXN    |

### Payment status

| Value      | Description                                                    |
| ---------- | -------------------------------------------------------------- |
| `pending`  | The payment order is pending processing.                       |
| `process`  | The payment order is in the process of being completed.        |
| `paid`     | The payment order has been completed and confirmed.            |
| `canceled` | The payment order has been canceled and will not be completed. |
| `error`    | An error occurred during the payment order process.            |

These linked tables allow for a structured and clear presentation of the information contained in the JSON, facilitating its understanding and use.

***

## Additional Notes

* The JSON structure is flexible and can be extended according to the specific needs of the system and business requirements.

This JSON provides a solid foundation for information exchange between reservation and payment systems, facilitating automation and efficiency in the process of managing reservations and payments for accommodations.

## Webhook.site

Webhook.site is a tool that allows you to generate unique URLs to receive and observe HTTP requests, including JSONs sent by webhooks. It is useful for testing and debugging.

#### Steps to use Webhook.site

1. **Generate a Webhook URL**:

   * Go to [Webhook.site](https://webhook.site/).
   * A unique URL will be automatically generated for you.

     <Image alt="Example of the URL" align="center" src="https://files.readme.io/1fa69c2-image.png">
       Example of the URL
     </Image>

2. **Configure the Webhook URL**: &#x20;

   * Go to the hotelpay backend in the Hotels section.
   * Specify the URL that will act as the endpoint to receive the JSON and, if necessary, indicate a token.

     ![](https://files.readme.io/2a66e14-image.png)

3. **Observe the Received Data**:

   * Return to the Webhook.site page.
   * You will see the HTTP requests received in real-time, including the JSON that has been sent.

   <Image alt="Example of a webhook request for a reservation" align="center" src="https://files.readme.io/039c16f-image.png">
     Example of a webhook request for a reservation
   </Image>

## Pipedream.com

Pipedream is an integration platform that allows you to automate workflows by connecting different services and applications. You can use Pipedream to receive webhooks and process the data.

#### Steps to use Pipedream.com

1. **Create an Account and a New Workflow**:
   * Sign up or log in to [Pipedream](https://pipedream.com/).
   * Create a new Workflow.

2. **Configure the Webhook Trigger**:

   * Select "HTTP / Webhook" as the trigger.

     <Image alt="We must select new HTTP" align="center" src="https://files.readme.io/9c88cce-image.png">
       We must select new HTTP
     </Image>
   * Pipedream will generate a unique URL that will act as your endpoint to receive webhooks.

     <Image alt="We copy the URL" align="center" src="https://files.readme.io/4c3ec87-image.png">
       We copy the URL
     </Image>

3. **Configure the Webhook Sender**:
   * Use the generated URL in the service that will send the webhook.
   * Go to the hotelpay backend in the Hotels section.
   * Specify the URL that will act as the endpoint to receive the JSON and, if necessary, indicate a token.\
     ![](https://files.readme.io/2a66e14-image.png)

4. **Add Steps to Process the JSON**:

   * Once the trigger is configured, add a step to process the data. You can use Node.js, Python, or any other language supported by Pipedream. In this case, we will use Node.js.

     <Image alt="We select Node for this example." align="center" src="https://files.readme.io/95b3435-image.png">
       We select Node for this example.
     </Image>

     <Image alt="We select the first option" align="center" src="https://files.readme.io/319de77-image.png">
       We select the first option
     </Image>
   * We copy the code and paste it into the code section in Pipedream

   ```javascript
   export default defineComponent({
     async run({ steps, $ }) {
       const data = steps.trigger.event.body;

       console.log('Received data:', data);

       const { uuid, reference, amount, currency, customer_info, items, transactions } = data;

       console.log(`Reservation UUID: ${uuid}`);
       console.log(`Reference: ${reference}`);
       console.log(`Amount: ${amount} ${currency}`);
       console.log(`Customer: ${customer_info.first_name} ${customer_info.last_name}`);
       console.log(`Items:`);
       items.forEach(item => {
         console.log(`- Description: ${item.description}, Price: ${item.price}, Quantity: ${item.quantity}`);
       });
       console.log(`Transactions:`);
       transactions.forEach(transaction => {
         console.log(`- Amount: ${transaction.amount}, Status: ${transaction.status}`);
       });

       return data;
     }
   });
   ```

5. **Deploy and Test**:
   * Save and deploy the workflow.

6. **Observe and Process the Data**:

   * In the Pipedream dashboard, you can see the execution of each workflow, including the received and processed data.

     <Image alt="Example of each webhook request in an implemented process" align="center" src="https://files.readme.io/0cc921b-image.png">
       Example of each webhook request in an implemented process
     </Image>
   * Pipedream also allows you to easily integrate with other services and perform additional actions, such as sending emails, storing data in databases, etc.

### Additional Considerations

* **Security**: Ensure that only trusted sources send data to your webhook. Pipedream allows you to configure authentication and other security mechanisms.
* **Errors and Retries**: Handle errors appropriately and consider implementing a retry system in case of failures.
* **Logs and Monitoring**: Both Webhook.site and Pipedream provide tools to monitor and log received requests, which is useful for debugging and analysis.

Using these tools, you can receive, observe, and process the data sent through the hotelpay webhook.