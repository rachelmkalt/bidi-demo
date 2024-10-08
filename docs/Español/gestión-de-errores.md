---
title: Gestión de Errores
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

En el caso del método *setApiKey* es una promesa y este puede fallar en el caso que sea una apikey no válida. De modo que es necesario controlar el flujo.

```js JavaScript
try {
  await Hotelpay.Checkout.setApiKey("...");
} catch (err) {
  console.error("Apikey Inválida!");
}
```

<h4>render</h4>

En el caso del método *render* tiene una dependencia directa al método *setApiKey* en el caso que la validación del apikey no esté terminada este método esperará a que termine o fallar en el caso de que *setApiKey* también fallé.

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
> *render* tambien puede fallar porque el selector del contenedor no es válido y recibiras un error como el siguiente.

> ❗️ Invalid container selector

> ❗️ pay
>
> *pay* es una promesa debe validar los campos requeridos del método *setDetails* y *setTotal*

> ❗️ Items was not provided please use [setDetails]

> ❗️ Total was not provided please use [setTotal]

> ❗️ Apikey was not provided please use [setApiKey]

> ❗️ Apikey is not valid