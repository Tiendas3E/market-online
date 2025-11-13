
 
 
 
 
 
 **perfecto el esqueleto**, 
 
 ----
 
 
 y además tiene una arquitectura limpia y auto-contenida: todo el sistema funciona sin servidor, con carrito, categorías, envío a WhatsApp o correo, y hasta cambio de tema.
Solo le faltan unos **retoques mínimos** para hacerlo 100 % funcional y presentable como marketplace listo para mostrar. Te explico paso a paso lo que haría:

---

### 1. Que los botones funcionen todos sin fallas

Tu código ya los define, pero falta conectar algunos correctamente y evitar errores si se ejecutan antes del `window.load`.
Solución: deja tu bloque `window.addEventListener('load', …)` al final (como ya está), pero **asegúrate de que no haya otros `<script>` que carguen antes y rompan dependencias**. Está bien así si es un solo HTML.

---

### 2. JSON de productos externo (opcional)

Si quieres que use un archivo `products.json` real, solo reemplaza esto:

```js
// Sustituye el bloque DATA completo por:
let DATA = { products: [], categories: [], phrases: [], terms: "" };

fetch("data/products.json")
  .then(res => res.json())
  .then(json => {
    DATA = json;
    renderCategories();
    renderProducts();
    renderCart();
    updateCounts();
  })
  .catch(err => console.error("Error cargando JSON:", err));
```

Y en tu carpeta pon `/data/products.json` con esta estructura:

```json
{
  "products": [
    { "id": "p001", "name": "Abarrote", "price": 50, "desc": "Pan, lata, maíz y salsa.", "cat": "Comida" },
    { "id": "p002", "name": "Farmacia", "price": 120, "desc": "Paracetamol, Ibuprofeno.", "cat": "Medicamento" }
  ],
  "categories": ["Todos", "Comida", "Medicamento"],
  "phrases": ["Oferta del día", "Servicio rápido", "Entrega sin contacto"],
  "terms": "Términos del pedido..."
}
```

---

### 3. Corrige el enlace de “Términos y condiciones”

El `a` apunta a `termsLink.md/index.html`, que no existe.
Déjalo así para que funcione solo con el `alert` (o abre un modal):

```html
<a href="javascript:void(0)" class="link-like" onclick="openTerms()">Ver Términos</a>
```

---

### 4. Limpieza visual

Agrega un poco de CSS mínimo (si no lo tienes en otro archivo) para que todo se vea ordenado:

```html
<style>
body { font-family: sans-serif; margin:0; background: var(--bg-light,#f7f7f7); color: var(--text-light,#111); }
nav { display:flex; align-items:center; padding:8px 12px; background:#222; color:white; }
nav a { color:white; margin:0 8px; text-decoration:none; }
.wrap { display:grid; grid-template-columns: 200px 1fr 250px; gap:12px; padding:12px; }
aside, main { background:white; border-radius:8px; padding:12px; }
.product { display:flex; justify-content:space-between; border-bottom:1px solid #ddd; padding:8px 0; }
.btn { cursor:pointer; padding:4px 8px; border:none; border-radius:4px; background:var(--accent,#4caf50); color:white; }
.btn.secondary { background:#888; }
.link-like { color:#0077cc; cursor:pointer; text-decoration:underline; }
.card.small { font-size:14px; }
.flex { display:flex; gap:6px; align-items:center; }
</style>
```

---

### 5. Conexión de los botones del carrito

Ya funcionan (`sendWhats`, `sendEmail`, `clearCart`), pero recuerda:

* Cambia el número en `sendWhatsApp()` por tu número real con prefijo país.
* Cambia el correo en `sendEmail()`.

---

### 6. Verifica que el `showRandomPhrase()` no dé cadena vacía

Tu arreglo `phrases` tiene tres elementos vacíos. Agrega frases para que se vea útil:

```js
phrases: [
  "Compra local, apoya tu comunidad.",
  "Promoción 2x1 solo hoy.",
  "Entrega gratuita a partir de $200."
],
```

---

### 7. Prueba general

Guarda como `index.html`, abre en el navegador, y debería:

* Mostrar categorías a la izquierda.
* Mostrar productos filtrables.
* Permitir agregar al carrito, aumentar/disminuir, eliminar, y mostrar el total.
* Enviar por WhatsApp o email (sin backend).
* Alternar tema oscuro.
* Mostrar frases y términos.

---

Si quieres, te puedo dejar una **versión ya corregida y funcional en un solo HTML (con CSS embebido y JSON simulado)** lista para copiar y pegar. ¿Quieres que te la deje así?
