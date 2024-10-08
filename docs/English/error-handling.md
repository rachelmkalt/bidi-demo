---
title: Error Handling
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
## setApiKey

In the case of the *setApiKey* method, it is a promise and it can fail if the API key is not valid. Therefore, it is necessary to control the flow.

```js JavaScript
try {
  await Hotelpay.Checkout.setApiKey("...");
} catch (err) {
  console.error("Invalid API key!");
}
```

## render

In the case of the *render* method, it has a direct dependency on the *setApiKey* method, in the case that the validation of the API key is not completed, this method will wait for it to finish or fail in the case that *setApiKey* also failed.

```js JavaScript
try {
  await Hotelpay.Checkout.render("#container");
} catch (err) {
  console.error(err);
}
```

> ❗️ Invalid apikey

> ❗️ render
>
> *render* can also fail because the container selector is not valid and you will receive an error like the following.

> ❗️ Invalid container selector

> ❗️ pay
>
> *pay* is a promise and it must validate the required fields of the *setDetails* and *setTotal* methods.

> ❗️ Items was not provided please use [setDetails]

> ❗️ Total was not provided please use [setTotal]

> ❗️ Apikey was not provided please use [setApiKey]

> ❗️ Apikey is not valid