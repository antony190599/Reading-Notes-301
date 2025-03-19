# Formularios en React: Controlados vs No Controlados

## Formularios controlados vs no controlados

Los formularios controlados y no controlados tienen distintas ventajas y desventajas según la aplicación:

### Ventajas de los formularios controlados:
- ✅ **Sincronización con el estado**: Permite actualizaciones en tiempo real y mayor control sobre los datos.
- ✅ **Validación inmediata**: Facilita la validación mientras el usuario escribe.
- ✅ **Mayor previsibilidad**: El estado del formulario siempre está en React, lo que ayuda en debugging.

### Desventajas de los formularios controlados:
- ❌ **Mayor re-renderizado**: Cada cambio en un input dispara una actualización del estado.
- ❌ **Más código**: Se requiere más boilerplate para manejar `useState` y eventos.

### Ventajas de los formularios no controlados:
- ✅ **Menor carga de re-renderizado**: React no necesita actualizar el estado en cada cambio.
- ✅ **Simplicidad**: Útil para formularios pequeños donde no se necesita control sobre cada input.

### Desventajas de los formularios no controlados:
- ❌ **Menos flexibilidad**: Acceder a los valores requiere `useRef` o manipulación del DOM.
- ❌ **Validaciones menos dinámicas**: La validación suele ocurrir solo al enviar el formulario.

**¿Cuándo usar cada uno?**
- **Formularios controlados**: Cuando necesitas validaciones en tiempo real, control del estado o integración con otros estados globales.
- **Formularios no controlados**: Para formularios simples y cuando el rendimiento es una preocupación.

---

## Reactividad y sincronización

React enfatiza la sincronización entre el estado y los inputs porque:
- Garantiza **un flujo de datos unidireccional**, lo que hace que el comportamiento del formulario sea predecible.
- Facilita el **debugging** porque los datos están en el estado y no en el DOM.
- Permite validar y modificar la información en tiempo real sin depender del DOM nativo.

Esto mejora la **predictibilidad** porque el estado del formulario está siempre reflejado en la UI y facilita encontrar errores en la lógica.

---

## Validación temprana

Comparando **onChange** vs **onSubmit**:

### Validar en `onChange`
- ✅ Proporciona **feedback inmediato** al usuario.
- ✅ Mejora la UX al evitar frustraciones con datos incorrectos.
- ❌ Puede ser costoso en rendimiento si hay muchas validaciones.

### Validar en `onSubmit`
- ✅ Reduce la cantidad de validaciones ejecutadas, mejorando rendimiento.
- ✅ Útil cuando la validación no es crítica en cada cambio.
- ❌ Puede frustrar al usuario al recibir todos los errores solo al final.

**¿Cuál prefiero?**  
Depende del contexto, pero en la mayoría de los casos, **validar en `onChange`** con un ligero retraso (debouncing) mejora la experiencia del usuario.

---

## Inmutabilidad y Spread Operator

En React, **la inmutabilidad es clave** porque:
- **Evita mutaciones accidentales** que pueden causar re-renderizados inesperados.
- **Facilita el uso de `useEffect` y comparación de estados**.
- **Mejora la depuración** al asegurarse de que los datos antiguos no cambien.

### Uso del Spread Operator:
```js
const nuevoContacto = { id: 3, nombre: "Ana" };
setContactos([...contactos, nuevoContacto]);
```

## Inmutabilidad y Spread Operator

Esto crea un nuevo array sin modificar el original.

### Riesgos de mutar el array original:
- ❌ Puede **romper el historial de cambios**, afectando optimizaciones como `React.memo()`.
- ❌ Puede hacer que **React no detecte cambios** y no re-renderice correctamente.

---

## Manejo de errores UX-friendly

Un buen sistema de validación mejora la UX mediante:

- **Mensajes de error claros** cerca del input incorrecto.
- **Bordes o resaltados de color** para guiar visualmente.
- **Deshabilitar el botón de enviar** hasta que el formulario sea válido.
- **Sugerencias en tiempo real** para corregir errores.

### Ejemplo:
```
<input 
  className={error ? "border-red-500" : "border-gray-300"} 
  value={inputValue} 
/>
{error && <p className="text-red-500">Campo obligatorio</p>}
```

## Escalabilidad

Cuando un formulario crece hasta tener **20+ campos y lógica condicional**, es clave:

- **Dividirlo en secciones y componentes reutilizables**.
- **Usar un manejador de estado como `useReducer`** para formularios complejos.
- **Implementar librerías como `Formik` o `React Hook Form`** que simplifican la gestión del estado.

### Ejemplo de uso de `useReducer`:
```
const [state, dispatch] = useReducer(reducer, initialState);
```

# Desacoplamiento de validaciones  

## ¿Dónde deberían vivir las validaciones?  
❌ **Dentro del componente del formulario** → No escalable, difícil de mantener.  
✅ **En un módulo separado** → Facilita la reutilización y el mantenimiento.  

### Ejemplo:  

```
// validations.js
export const validarEmail = (email) => /\S+@\S+\.\S+/.test(email);
```

```
import { validarEmail } from "./validations";
```

## Mitos y verdades  

| Enunciado | Clasificación |
|-----------|--------------|
| "Los formularios controlados siempre son mejores" | 🟡 Intermedio (Depende del caso) |
| "Usar formularios controlados mejora el rendimiento" | 🔴 Mito (Puede generar más renders) |
| "Validar en `onChange` siempre es mejor que en `onSubmit`" | 🟡 Intermedio (Mejora UX, pero puede ser costoso) |
| "Mutar el estado directamente en React puede causar bugs" | 🟢 Verdad (React no detecta cambios correctamente) |
| "Extraer validaciones a un módulo mejora la reutilización" | 🟢 Verdad (Sigue buenas prácticas de código limpio) |


