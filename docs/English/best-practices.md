---
title: Best Practices
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
## Set API key

Set the API key at the beginning of the application. The *setApiKey* method is a promise that can fail if the key is invalid.

```js JavaScript
Hotelpay.Checkout.setApiKey("pk_1237932718")
  .then(() => {
    console.log("API key is valid!");
    // After validating that the API key is valid, we are sure that the render will work
    Hotelpay.Checkout.render("#container");
  })
  .catch(() => {
    // Handle the problem
    console.warning("API key is invalid!");
  });
```