---
title: SDK Checkout
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
# Reference

```javascript
Hotelpay: {
  Checkout: {
    // Generales
    setApiKey: (key: string) => Promise<void>;
    setCurrency: (currency: "MXN" | "USD") => void;
    setTotal: (total: number) => void;
    setDetails: (data: Details) => void;
    setMode: (mode: "testing" | "production") => void;
    setLanguage: (lang: "es" | "en") => void;

    // Overlay
    onSuccess: (callback: () => void) => void;
    onCancel: (callback: () => void) => void;
    onFail: (callback: () => void) => void;
    pay: async () => Promise<void>;
    render: async (selector: string, onSubmit?: () => void) => Promise<void>;

    // Iframe
    on: (eventName: string, cb: Function) => void;
    binFocus: () => void;
    binBlur: () => void;
    cvvFocus: () => void;
    cvvBlur: () => void;
    continueCheckout: () => void;
    setAddress: (address: Address) => void;
    renderIframe: async (selector: string) => Promise<void>;
  }
}
```

# Environments

## Testing

Environment configured to simulate the operating conditions of an application or system, allowing developers and testers to verify functionality, look for errors, and ensure quality before the product is deployed in the production environment. This environment is crucial for identifying and solving problems without affecting normal operation or the end user experience.

```javascript
Hotelpay.Checkout.setMode(Hotelpay.TESTING);
```

## Production

It is the final environment in which an application or software system runs and is accessible to end users. It is the live environment where the application is used to perform real operations and fulfill its designed purpose. Since any error or failure can have direct impacts on business processes, user experience and, in some cases, may carry financial or security risks. Therefore, before an application is moved to the production environment, it must go through rigorous testing.

```javascript
Hotelpay.Checkout.setMode(Hotelpay.PRODUCTION);
```

# General Methods

<br />

## setApiKey

This method is used to set the API key necessary to authenticate requests made from the SDK to the service.

```js JavaScript
Hotelpay.Checkout.setApiKey("pk_1237932718");
```

## setDetails

This method is used to set what are the details of the reservation, as well as internal id of the reservation manager, items, customer data and reservation dates.

```js JavaScript
Hotelpay.Checkout.setDetails({
  /**
   * Optional.
   * Send internal identifier of the reservation engine
   */
  referenceId: "my-internal-id",

  /* Required */
  reserve_data: {
    booking_number: "00001",
    checkin: "2024-12-01",
    checkout: "2024-12-31",
    guests_adults: 2,
    guests_children: 1
  },

  /* Required */
  items: [
    {
      description: "Habitación doble vista al mar",
      price: 56.79,
      quantity: 1,
    },
    {
      description: "Habitación sencilla",
      price: 34.99,
      quantity: 1,
    },
  ],

  /* Required */
  customer_info: {
    first_name: "Victor",
    last_name: "Alvarez",
    phone: "7777777777",
    email: "victor@hotelpay.com",
  },

  /* Optional */
  card_holder: {
    name: "Victor Alvarez",
    phone: "7777777777",
    email: "victor@hotelpay.com",
  },
});
```

<br />

## setTotal

This method is used to set the total purchase amount, 2 decimals are allowed.

> ❗️ Maximum 2 decimals

```js JavaScript
Hotelpay.Checkout.setTotal(444.44);
```

## setMode

This method is used to set the environment in which the Checkout will be executed, it can be a test or production environment.

```js JavaScript
Hotelpay.Checkout.setMode(Hotelpay.PRODUCTION);

Hotelpay.Checkout.setMode(Hotelpay.TESTING);
```

## setCurrency

This method is used to set the currency for transactions.

```js JavaScript
Hotelpay.Checkout.setCurrency("MXN");

Hotelpay.Checkout.setCurrency("USD");
```

## setLanguage

This method is used to set the language.

```js JavaScript
Hotelpay.Checkout.setLanguage("es");

Hotelpay.Checkout.setLanguage("en");
```

# Overlay

## onSuccess

Callback that is executed when the transaction has been successfully completed.

```js JavaScript
Hotelpay.Checkout.onSuccess(() => {
  console.log("Successful payment");
});
```

## onCancel

Callback that is executed when the transaction is canceled.

```js JavaScript
Hotelpay.Checkout.onCancel(() => {
  console.log("Checkout canceled");
});
```

## onFail

Callback that is executed when the transaction fails.

```js JavaScript
Hotelpay.Checkout.onFail(() => {
  console.log("Unsatisfactory payment");
});
```

## pay

Method to open the checkout overlay.

```js JavaScript
Hotelpay.Checkout.pay();
```

## render

Method to render the pay button.

This button

```js JavaScript
const onSubmit = () => {
  /**
   * add custom behavior
   **/

  // Open checkout overlay
  Hotelpay.Checkout.pay();
};

Hotelpay.Checkout.render("#container", onSubmit);
```

# Iframe

## renderIframe

Method to render the iframe version form.

```js JavaScript
Hotelpay.Checkout.renderIframe("#container");
```

## setAddress

Method to send the necessary information for the transaction, user address and email

```js JavaScript
const user = {
  email: "unknown@hotelpay.com",
};
const address = {
  country: "México",
  state: "Jalisco",
  city: "Guadalajara",
  street: "Unknown",
  zip: "45000",
};
Hotelpay.Checkout.setAddress(user, address);
```

## on(eventName)

Method to listen to events that happen inside the iframe.

```js Javascript
Hotelpay.Checkout.on("complete-form-card", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-start", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-end", (e) => console.log(e));

Hotelpay.Checkout.on("msi", (e) => console.log(e));

Hotelpay.Checkout.on("3ds-start", (e) => console.log(e));

Hotelpay.Checkout.on("3ds-end", (e) => console.log(e));

Hotelpay.Checkout.on("transaction-completed", (e) => console.log(e));

Hotelpay.Checkout.on("transaction-failed", (e) => console.log(e));

/**
 * Split payment only
 **/

Hotelpay.Checkout.on("toggle-split", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-2-start", (e) => console.log(e));

Hotelpay.Checkout.on("adding-card-2-end", (e) => console.log(e));
```

Events:

* **complete-form-card**: this event is executed when the status between valid and invalid card data changes

When the form starts, it is executed as `is_valid = false` when all the data is captured correctly it is executed as `is_valid = true`. If at any time the data changes again, it is possible to receive the event `is_valid = false` indicating that a data was modified and must be corrected

```json
{
  is_valid: true | false
}
```

* **adding-card-start**: this event is executed after `Hotelpay.Checkout.continueCheckout()` indicates that the process of adding a card has started

* **adding-card-end**: this event is executed after the process of adding a card has finished and returns as information if the card could be added correctly

```json
{
  is_valid: true | false
}
```

* **msi**: this event is executed after `adding-card-end` if the card was added correctly returns as information the available terms and the minimum amounts to be able to apply

```json
{
  available: true | false,
  installments: {
    months: 3,
    minimum: 1000
  }[]
}
```

* **3ds-start**: this event is executed when it is necessary to execute a 3ds challenge

* **3ds-end**: this event is executed when the 3ds challenge ends

* **transaction-completed**: this event is executed when the transaction has finished with a positive result

* **transaction-failed**: this event is executed when the transaction has finished with a negative result

Split payment

* **toggle-split**: this event is executed when the user changes from single payment mode or split payment mode

```json
{
  is_split: true | false
}
```

* **adding-card-2-start**: this event is executed after `Hotelpay.Checkout.continueCheckout()` indicates that the process of adding card 2 has started

* **adding-card-2-end**: this event is executed after the process of adding a card has finished and returns as information if card 2 could be added correctly

```json
{
  is_valid: true | false
}
```

## continueCheckout

Method to execute the transaction after having filled in the user information

```js JavaScript
Hotelpay.Checkout.continueCheckout();
```