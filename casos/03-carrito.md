# Casos de prueba — Carrito de compras

**Módulo:** Carrito
**URL:** https://www.saucedemo.com/cart.html
**Fecha de ejecución:** 5 de noviembre de 2026
**Precondición general:** sesión iniciada con `standard_user` y carrito vacío

---

## CP-CAR-001 · Agregar un producto desde el catálogo

| | |
|---|---|
| **Tipo** | Funcional — camino feliz |
| **Prioridad** | Alta |

**Pasos**
1. En el catálogo, pulsar **Add to cart** en un producto
2. Observar el botón y el ícono del carrito

**Resultado esperado**
El botón cambia a **Remove**. El ícono del carrito muestra el contador en **1**.

**Resultado de la ejecución:** ✅ Pasa — El botón cambia a Remove y el contador muestra 1.

---

## CP-CAR-002 · Agregar varios productos y verificar el contador

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Alta |

**Pasos**
1. Agregar tres productos distintos
2. Observar el contador
3. Abrir el carrito

**Resultado esperado**
El contador muestra **3**. El carrito lista los tres productos, cada uno con cantidad 1,
nombre, descripción y precio.

**Resultado de la ejecución:** ✅ Pasa — Contador en 3; el carrito lista los 3 productos con cantidad 1, descripción y precio.

---

## CP-CAR-003 · El precio en el carrito coincide con el catálogo

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Alta |

**Pasos**
1. Anotar el precio de dos productos en el catálogo
2. Agregarlos al carrito
3. Abrir el carrito y comparar

**Resultado esperado**
Cada producto muestra en el carrito exactamente el mismo precio que en el catálogo.

**Por qué importa:** una diferencia de precio entre pantallas es un cobro incorrecto, y es
lo primero que el cliente reclama.

**Resultado de la ejecución:** ✅ Pasa — Los precios del carrito coinciden con el catálogo ($29.99, $9.99, $7.99).

---

## CP-CAR-004 · Quitar un producto desde el carrito

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Alta |

**Pasos**
1. Agregar dos productos
2. Abrir el carrito y pulsar **Remove** en uno de ellos
3. Volver al catálogo con **Continue Shopping**

**Resultado esperado**
El producto desaparece del carrito y el contador baja a **1**. En el catálogo, ese producto
vuelve a mostrar **Add to cart**.

**Resultado de la ejecución:** ✅ Pasa — El producto sale del carrito, el contador baja y el catálogo vuelve a mostrar Add to cart.

---

## CP-CAR-005 · Quitar desde el catálogo se refleja en el carrito

| | |
|---|---|
| **Tipo** | Regresión |
| **Prioridad** | Media |

**Pasos**
1. Agregar dos productos
2. En el catálogo, pulsar **Remove** en uno de ellos
3. Abrir el carrito

**Resultado esperado**
El carrito muestra solo el producto que sigue agregado y el contador coincide.

**Por qué importa:** el contador y la lista del carrito son dos estados distintos; cuando
se quita desde otra pantalla es cuando más fácil se desincronizan.

**Resultado de la ejecución:** ✅ Pasa — El carrito muestra solo el producto que sigue agregado y el contador coincide.

---

## CP-CAR-006 · El carrito se conserva al navegar

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Media |

**Pasos**
1. Agregar dos productos
2. Abrir la ficha de otro producto y volver
3. Abrir el carrito y pulsar **Continue Shopping**

**Resultado esperado**
En todo momento el contador muestra **2** y el carrito conserva los mismos productos.

**Resultado de la ejecución:** ✅ Pasa — El contador se mantiene en 2 en todas las pantallas y el carrito conserva los productos.

---

## CP-CAR-007 · No se puede iniciar el checkout con el carrito vacío

| | |
|---|---|
| **Tipo** | Negativo / Borde |
| **Prioridad** | Media |

**Pasos**
1. Con el carrito vacío, abrir el carrito
2. Pulsar **Checkout**

**Resultado esperado**
El sistema no permite iniciar una compra sin productos: el botón está deshabilitado o se
muestra un mensaje que lo explica.

**Por qué importa:** si deja avanzar, se pueden generar órdenes vacías que después alguien
tiene que limpiar a mano.

**Resultado de la ejecución:** ❌ Falla — Con el carrito vacío, **Checkout** está habilitado y la compra se completa: resumen en $0.00 y confirmación *"Thank you for your order!"*. Ver **H-001**.

---

## Resumen

| Prioridad | Casos |
|---|---|
| Alta | 4 |
| Media | 3 |
| **Total** | **7** |

**Ejecución del 5 de noviembre de 2026:** 6 pasan · 1 falla

Casos que fallan: CP-CAR-007. Los defectos están en [hallazgos.md](../hallazgos.md).
