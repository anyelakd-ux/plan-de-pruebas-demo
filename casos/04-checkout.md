# Casos de prueba — Checkout

**Módulo:** Checkout
**URL:** https://www.saucedemo.com/checkout-step-one.html
**Fecha de ejecución:** 5 de noviembre de 2026
**Precondición general:** sesión iniciada con `standard_user` y al menos un producto en el
carrito

---

## CP-CHK-001 · Compra completa con datos válidos

| | |
|---|---|
| **Tipo** | Funcional — camino feliz |
| **Prioridad** | Alta |

**Pasos**
1. Abrir el carrito y pulsar **Checkout**
2. Completar First Name, Last Name y Zip/Postal Code
3. Pulsar **Continue**
4. Revisar el resumen y pulsar **Finish**

**Resultado esperado**
Se muestra la confirmación *"Thank you for your order!"*. El carrito queda vacío y el
contador desaparece.

**Resultado de la ejecución:** ✅ Pasa — Confirmación *"Thank you for your order!"*; el carrito queda vacío y el contador desaparece.

---

## CP-CHK-002 · Nombre vacío

| | |
|---|---|
| **Tipo** | Negativo |
| **Prioridad** | Alta |

**Pasos**
1. En el formulario de checkout, dejar First Name vacío
2. Completar Last Name y Zip/Postal Code
3. Pulsar **Continue**

**Resultado esperado**
Se muestra *"Error: First Name is required"* y el usuario permanece en el formulario.

**Resultado de la ejecución:** ✅ Pasa — Mensaje: *"Error: First Name is required"*.

---

## CP-CHK-003 · Apellido vacío

| | |
|---|---|
| **Tipo** | Negativo |
| **Prioridad** | Media |

**Pasos**
1. Completar First Name y Zip/Postal Code
2. Dejar Last Name vacío
3. Pulsar **Continue**

**Resultado esperado**
Se muestra *"Error: Last Name is required"* y el usuario permanece en el formulario.

**Resultado de la ejecución:** ✅ Pasa — Mensaje: *"Error: Last Name is required"*.

---

## CP-CHK-004 · Código postal vacío

| | |
|---|---|
| **Tipo** | Negativo |
| **Prioridad** | Media |

**Pasos**
1. Completar First Name y Last Name
2. Dejar Zip/Postal Code vacío
3. Pulsar **Continue**

**Resultado esperado**
Se muestra *"Error: Postal Code is required"* y el usuario permanece en el formulario.

**Resultado de la ejecución:** ✅ Pasa — Mensaje: *"Error: Postal Code is required"*.

---

## CP-CHK-005 · Campos con solo espacios

| | |
|---|---|
| **Tipo** | Borde |
| **Prioridad** | Media |

**Pasos**
1. Escribir solo espacios en First Name, Last Name y Zip/Postal Code
2. Pulsar **Continue**

**Resultado esperado**
El formulario trata los espacios como campos vacíos y muestra el error correspondiente.

**Por qué importa:** si los espacios pasan la validación, se generan órdenes sin datos
reales de envío.

**Resultado de la ejecución:** ❌ Falla — Con solo espacios en los tres campos, el formulario avanza al resumen sin errores. Ver **H-002**.

---

## CP-CHK-006 · El resumen muestra los mismos productos que el carrito

| | |
|---|---|
| **Tipo** | Funcional / Regresión |
| **Prioridad** | Alta |

**Pasos**
1. Agregar dos productos y anotar nombre, cantidad y precio
2. Avanzar hasta el resumen (Checkout: Overview)
3. Comparar con lo anotado

**Resultado esperado**
El resumen lista los mismos productos, con las mismas cantidades y precios que el carrito.

**Resultado de la ejecución:** ✅ Pasa — El resumen lista los mismos productos, cantidades y precios que el carrito.

---

## CP-CHK-007 · Subtotal, impuesto y total

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Alta |

**Pasos**
1. Agregar dos productos al carrito
2. Avanzar hasta el resumen
3. Sumar a mano los precios y calcular el impuesto con la tasa del entorno

**Resultado esperado**
- **Item total** es exactamente la suma de los precios
- **Tax** corresponde a la tasa aplicada sobre el subtotal, redondeada a dos decimales
- **Total** es Item total + Tax, sin diferencias de centavos

**Por qué importa:** una diferencia de un centavo en el total es un cobro incorrecto.
Los errores de redondeo casi nunca aparecen con un solo producto; por eso el caso usa dos.

**Resultado de la ejecución:** ✅ Pasa — Con $29.99 + $9.99: Item total $39.98, Tax $3.20 (8 %), Total $43.18. Sin diferencias de centavos.

---

## CP-CHK-008 · Cancelar en el resumen conserva el carrito

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Media |

**Pasos**
1. Avanzar hasta el resumen
2. Pulsar **Cancel**
3. Abrir el carrito

**Resultado esperado**
La aplicación vuelve al catálogo sin completar la compra, y el carrito conserva los mismos
productos.

**Resultado de la ejecución:** ✅ Pasa — Cancel vuelve al catálogo y el carrito conserva los 2 productos.

---

## CP-CHK-009 · Después de comprar, el catálogo queda listo para una compra nueva

| | |
|---|---|
| **Tipo** | Regresión |
| **Prioridad** | Media |

**Pasos**
1. Completar una compra
2. En la confirmación, pulsar **Back Home**
3. Observar el catálogo y el ícono del carrito

**Resultado esperado**
El contador no aparece y todos los productos vuelven a mostrar **Add to cart**.

**Por qué importa:** si algún botón queda en *Remove* después de comprar, el usuario cree
que ya tiene productos en el carrito y la siguiente compra sale mal.

**Resultado de la ejecución:** ✅ Pasa — Back Home lleva al catálogo sin contador y con todos los botones en Add to cart.

---

## Resumen

| Prioridad | Casos |
|---|---|
| Alta | 4 |
| Media | 5 |
| **Total** | **9** |

**Ejecución del 5 de noviembre de 2026:** 8 pasan · 1 falla

Casos que fallan: CP-CHK-005. Los defectos están en [hallazgos.md](../hallazgos.md).
