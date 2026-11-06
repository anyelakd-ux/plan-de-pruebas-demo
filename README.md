# Plan de pruebas — Tienda en línea (Sauce Demo)

Plan de pruebas completo sobre [Sauce Demo](https://www.saucedemo.com), una tienda de
comercio electrónico pública creada para practicar testing.

Este repositorio muestra **cómo trabajo**: cómo analizo una funcionalidad, cómo escribo
un caso de prueba, qué priorizo y cómo documento lo que encuentro.

> **Nota.** Todo lo que hay aquí está construido sobre un producto público de
> demostración. El trabajo que realizo para mis empleadores es confidencial y no se
> publica.

---

## Qué se probó

| Módulo | Casos | Pasan | Fallan | Documento |
|---|---|---|---|---|
| Inicio de sesión | 8 | 7 | 1 | [casos/01-inicio-de-sesion.md](casos/01-inicio-de-sesion.md) |
| Catálogo y ordenamiento | 6 | 5 | 1 | [casos/02-catalogo.md](casos/02-catalogo.md) |
| Carrito de compras | 7 | 6 | 1 | [casos/03-carrito.md](casos/03-carrito.md) |
| Checkout | 9 | 8 | 1 | [casos/04-checkout.md](casos/04-checkout.md) |
| **Total** | **30** | **26** | **4** | |

**30 casos** — camino feliz, negativos, bordes, validación visual y regresión.
Ejecutados el 5 de noviembre de 2026 con el usuario `standard_user`. Cada caso incluye su
resultado de ejecución.

---

## Cómo está organizado cada caso

Cada caso sigue la misma estructura, porque un caso que no se puede reproducir no sirve:

```
ID           Identificador único, para poder referenciarlo
Módulo       Dónde vive la funcionalidad
Título       Qué se verifica, en una frase
Tipo         Funcional / Negativo / Borde / Visual / Regresión
Prioridad    Alta / Media / Baja, según impacto en el negocio
Precondición Qué debe existir antes de empezar
Pasos        Numerados, reproducibles por alguien que no conoce el sistema
Resultado    Qué debe pasar exactamente
Evidencia    Captura o referencia
```

---

## Criterio de priorización

No todos los casos valen lo mismo. Priorizo según **qué pasa si esto falla en producción**:

| Prioridad | Criterio | Ejemplo |
|---|---|---|
| **Alta** | Impide comprar, cobra mal o expone datos | El total del carrito no coincide con la suma de los productos |
| **Media** | Molesta al usuario pero existe alternativa | El ordenamiento por precio no persiste al volver atrás |
| **Baja** | Estético, sin impacto funcional | Un texto desalineado en pantallas pequeñas |

---

## Hallazgos

La ejecución encontró **6 hallazgos** (5 de severidad media y 1 baja), documentados en
[hallazgos.md](hallazgos.md) con el formato que uso a diario: **qué está pasando, qué lo
provoca y qué puede provocar** si no se atiende.

El más importante: con el carrito vacío se puede completar una compra de $0.00.

---

## Análisis de regresión

Antes de probar cualquier cambio, analizo qué más puede romperse. El razonamiento
completo está en [analisis-de-regresion.md](analisis-de-regresion.md).

Ejemplo del criterio: si se modifica el cálculo del total en el carrito, hay que volver a
probar el checkout, el resumen de la orden y el impuesto — aunque nadie haya tocado esas
pantallas.

---

## Contacto

Anyela Dávila · Quality Assurance Engineer
📧 anyela.kd@gmail.com · 💼 [LinkedIn](https://www.linkedin.com/in/anyela-davila-9795a0105/)
