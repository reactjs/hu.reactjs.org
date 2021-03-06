---
id: shallow-renderer
title: Sekély renderelő
permalink: docs/shallow-renderer.html
layout: docs
category: Reference
---

**Importálás**

```javascript
import ShallowRenderer from 'react-test-renderer/shallow'; // ES6
var ShallowRenderer = require('react-test-renderer/shallow'); // ES5 npm-mel
```

## Áttekintés {#overview}

Amikor egységteszteket írsz Reacthez, a sekély renderelés hasznos lehet. A sekély renderelés lehetővé teszi egy komponens renderelését "egy szint mélységig", és tényeket tudsz állítani arról, hogy a render metódus mit ad vissza anélkül, hogy a gyermekkomponensek viselkedésétől kéne aggódnod, mivel ezek nem lesznek renderelve. Ez nem igényel DOM-ot.

Például, ha van ez a komponensed:

```javascript
function MyComponent() {
  return (
    <div>
      <span className="heading">Cím</span>
      <Subcomponent foo="bar" />
    </div>
  );
}
```

Akkor az alábbit állíthatod:

```javascript
import ShallowRenderer from 'react-test-renderer/shallow';

// a tesztedben:
const renderer = new ShallowRenderer();
renderer.render(<MyComponent />);
const result = renderer.getRenderOutput();

expect(result.type).toBe('div');
expect(result.props.children).toEqual([
  <span className="heading">Cím</span>,
  <Subcomponent foo="bar" />
]);
```

A sekély renderelésnek jelenleg vannak korlátai, ugyanis nem támogatja a ref-eket.

> Megjegyzés:
>
> Ajánljuk továbbá, hogy nézz rá az Enzyme [Shallow Rendering API](https://airbnb.io/enzyme/docs/api/shallow.html)-jára. Ez szebb, felsőbbszintű API-ket szolgáltat ugyanazzal a funkcionalitással.

## Referencia {#reference}

### `shallowRenderer.render()` {#shallowrendererrender}

Gondolhatsz úgy a shallowRenderer-re, mint egy "helyre", ahova a tesztelt komponenst renderelheted és ahonnan ki tudod vonni a komponens kimenetét.

A `shallowRenderer.render()` hasonló a [`ReactDOM.render()`](/docs/react-dom.html#render)-hez, de nincs szüksége DOM-ra, és csak egy szint mélységig renderel. Ez azt jelenti, hogy a komponenseket a gyermekeik implementációjától elzártan tudod tesztelni.

### `shallowRenderer.getRenderOutput()` {#shallowrenderergetrenderoutput}

A `shallowRenderer.render()` meghívása után használhatod a `shallowRenderer.getRenderOutput()` metódust, hogy megkapd a sekélyen renderelt kimenetet.

Ezután nekiállhatsz tényeket állítani a kimenetről.
