---
id: legacy-event-pooling
title: Esemény pooling
permalink: docs/legacy-event-pooling.html
---

>Megjegyzés
>
>Ez az oldal csak a React 16 és korábbi veziókra, valamint React Native-re vonatkozik.
>
>A React 17 a weben **nem használ** esemény pooling-ot.
>
>Erről a React 17 változásról [többet itt olvashatsz](/blog/2020/08/10/react-v17-rc.html#no-event-pooling).

A [`SyntheticEvent`](/docs/events.html) objektumok egy közös készletben vannak. Ez azt jelenti, hogy a `SyntheticEvent` objektum újrafelhasználható és minden tulajdonság ki lesz nullázva az esemény callbackjének meghívása után. Ez például nem fog működni:

```javascript
function handleChange(e) {
  // Ez nem fog műküdni, mert az esemény objektumok újra fel lesznek használva.
  setTimeout(() => {
    console.log(e.target.value); // Túl késő!
  }, 100);
}
```

Ha szeretnél egy esemény tulajdonságaihoz azután hozzáférni hogy az eseménykezelő lefutott, meg kell hogy hívd az `e.persist()` metódust:

```javascript
function handleChange(e) {
  // Megakadályozza a Reactet a tulajdonságok visszaállításában:
  e.persist();

  setTimeout(() => {
    console.log(e.target.value); // Ez működik
  }, 100);
}
```
