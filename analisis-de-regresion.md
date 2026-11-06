# Análisis de regresión

La pregunta que me hago antes de probar cualquier cambio no es *"¿funciona lo que
tocaron?"*, sino **"¿qué más se puede haber roto?"**.

Este documento muestra ese razonamiento sobre la tienda de demostración.

---

## Mapa de dependencias

Qué alimenta a qué. Si se modifica lo de la izquierda, hay que volver a probar lo de la
derecha aunque nadie lo haya tocado.

| Si se modifica… | Hay que volver a probar… | Por qué |
|---|---|---|
| Cálculo del total en el carrito | Checkout · Resumen de la orden · Impuesto | El total viaja por las tres pantallas; un cambio de redondeo se arrastra |
| Autenticación | Carrito · Catálogo · Acceso directo por URL | El carrito se asocia a la sesión: si cambia cómo se identifica al usuario, puede perderse o mezclarse |
| Ordenamiento del catálogo | Carrito · Precios mostrados | Si el orden reasigna índices, se puede agregar un producto distinto al que se eligió |
| Agregar / quitar del carrito | Contador del ícono · Checkout · Botón Remove | El contador es un estado aparte que se desincroniza con facilidad |
| Formulario de checkout | Resumen · Confirmación | Los datos ingresados se muestran después; un cambio de validación puede dejar pasar datos vacíos |

---

## Ejemplo aplicado

**Cambio propuesto:** *"Agregar un descuento del 10% para compras mayores a $50."*

Parece un cambio acotado al carrito. No lo es.

### Lo que hay que probar

**En el carrito (lo obvio)**
- Compra de $49.99 → sin descuento
- Compra de $50.00 → ¿aplica o no? El límite exacto siempre es ambiguo, hay que confirmarlo con negocio
- Compra de $50.01 → con descuento

**En el checkout (lo que se olvida)**
- El subtotal, el descuento y el impuesto se muestran por separado
- El impuesto, ¿se calcula antes o después del descuento? Cambia el total final
- El total del checkout coincide exactamente con el del carrito

**En la confirmación**
- El monto confirmado es el mismo que se mostró antes de pagar

**Casos borde**
- Compra que supera los $50 y luego se quita un producto y baja: ¿se retira el descuento?
- Compra con descuento donde se vacía el carrito completo y se vuelve a llenar

**Regresión pura**
- Compras menores a $50 siguen funcionando igual que antes
- El contador del carrito no se ve afectado

### Conclusión

Un cambio descrito en una línea genera **doce casos de prueba**, de los cuales solo tres
están en la pantalla que se modificó. Los otros nueve son los que encuentran los defectos
que llegan a producción.

---

## El criterio de fondo

> Un cambio nunca está contenido en la pantalla donde se hizo.

Antes de dar un desarrollo por verificado, me pregunto tres cosas:

1. **¿Qué otras pantallas consumen este dato?**
2. **¿Qué pasa en el límite exacto de la condición?** — el `>` contra el `>=` es de los
   defectos más frecuentes y más caros.
3. **¿Lo que funcionaba antes sigue funcionando?** — porque el usuario no perdona que se
   rompa algo que ya usaba.
