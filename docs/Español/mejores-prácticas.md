---
title: Mejores Prácticas
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
## Indicar API key

Indicar API key desde el inicio de la aplicación, el método *setApiKey* es una promesa que puede fallar en el caso de ser inválida

```js JavaScript
Hotelpay.Checkout.setApiKey("pk_1237932718")
  .then(() => {
    console.log("Apikey es válida!");
    // Luego de validar que la apikey es válida estamos seguros que el render funcionará
    Hotelpay.Checkout.render("#container");
  })
  .catch(() => {
    // Controlar el problema
    console.warning("Apikey es inválida!");
  });
```