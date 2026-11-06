# Casos de prueba — Catálogo y ordenamiento

**Módulo:** Catálogo
**URL:** https://www.saucedemo.com/inventory.html
**Fecha de ejecución:** 5 de noviembre de 2026
**Precondición general:** sesión iniciada con `standard_user`

---

## CP-CAT-001 · El catálogo muestra los productos completos

| | |
|---|---|
| **Tipo** | Funcional — camino feliz |
| **Prioridad** | Alta |

**Pasos**
1. Iniciar sesión con `standard_user`
2. Observar el catálogo

**Resultado esperado**
Se muestran 6 productos. Cada uno tiene imagen, nombre, descripción, precio con formato
`$00.00` y el botón **Add to cart**.

**Por qué importa:** un producto sin precio o sin imagen no se vende, aunque la compra
funcione.

**Resultado de la ejecución:** ✅ Pasa — 6 productos, todos con imagen, nombre, descripción, precio `$00.00` y **Add to cart**.

---

## CP-CAT-002 · Ordenar por precio, de menor a mayor

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Alta |

**Pasos**
1. Abrir el selector de orden
2. Elegir **Price (low to high)**

**Resultado esperado**
Los precios quedan en orden ascendente: el primer producto es el más barato y el último el
más caro. Ningún producto desaparece ni se duplica.

**Resultado de la ejecución:** ✅ Pasa — Orden ascendente: primero Sauce Labs Onesie ($7.99), último Sauce Labs Fleece Jacket ($49.99).

---

## CP-CAT-003 · Ordenar por precio, de mayor a menor

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Media |

**Pasos**
1. Abrir el selector de orden
2. Elegir **Price (high to low)**

**Resultado esperado**
Los precios quedan en orden descendente. Si dos productos tienen el mismo precio, ambos
aparecen y su orden entre sí es estable.

**Por qué importa:** los empates de precio son el caso borde que más fácil se rompe en un
ordenamiento.

**Resultado de la ejecución:** ✅ Pasa — Orden descendente. Los dos productos de $15.99 aparecen ambos, en orden estable.

---

## CP-CAT-004 · Ordenar por nombre, de Z a A y de vuelta a A–Z

| | |
|---|---|
| **Tipo** | Funcional |
| **Prioridad** | Media |

**Pasos**
1. Elegir **Name (Z to A)**
2. Verificar el orden
3. Elegir **Name (A to Z)**

**Resultado esperado**
En el paso 2 los nombres quedan en orden alfabético inverso. En el paso 3 el catálogo vuelve
exactamente al orden inicial.

**Resultado de la ejecución:** ✅ Pasa — Z–A invierte el orden; al volver a A–Z el catálogo queda igual al inicial.

---

## CP-CAT-005 · La ficha del producto coincide con el catálogo

| | |
|---|---|
| **Tipo** | Funcional / Regresión |
| **Prioridad** | Alta |

**Pasos**
1. Anotar nombre, descripción y precio de un producto en el catálogo
2. Hacer clic en el nombre del producto
3. Comparar la ficha con lo anotado
4. Pulsar **Back to products**

**Resultado esperado**
La ficha muestra el mismo nombre, descripción, precio e imagen que el catálogo. El botón
**Back to products** regresa al catálogo.

**Por qué importa:** si la ficha y el catálogo muestran precios distintos, el cliente
reclama en el momento de pagar.

**Resultado de la ejecución:** ✅ Pasa — Nombre, descripción, precio e imagen coinciden; Back to products vuelve al catálogo.

---

## CP-CAT-006 · El orden elegido se mantiene al volver de la ficha

| | |
|---|---|
| **Tipo** | Regresión |
| **Prioridad** | Media |

**Pasos**
1. Elegir **Price (low to high)**
2. Abrir la ficha de cualquier producto
3. Pulsar **Back to products**

**Resultado esperado**
El catálogo conserva el orden por precio que el usuario eligió.

**Por qué importa:** si el orden se pierde, el usuario tiene que volver a ordenar cada vez
que mira un producto. No impide comprar, pero molesta en cada visita.

**Resultado de la ejecución:** ❌ Falla — Al volver de la ficha el selector regresa a **Name (A to Z)** y se pierde el orden por precio. Ver **H-005**.

---

## Resumen

| Prioridad | Casos |
|---|---|
| Alta | 3 |
| Media | 3 |
| **Total** | **6** |

**Ejecución del 5 de noviembre de 2026:** 5 pasan · 1 falla

Casos que fallan: CP-CAT-006. Los defectos están en [hallazgos.md](../hallazgos.md).
