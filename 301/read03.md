# React - Pasar funciones como props

En React, una práctica común es pasar funciones como props a los componentes hijos. Esto permite que los componentes hijos se comuniquen con los componentes padres y modifiquen su estado o realicen acciones específicas.

## ¿Por qué usarlo?

Cuando trabajamos con componentes reutilizables, es necesario que estos puedan interactuar con sus padres. Pasar funciones como props nos permite manejar eventos, actualizar estados y mantener una arquitectura clara y modular. Es un enfoque fundamental para lograr una comunicación eficaz entre componentes en React.

## Ejemplo básico

Supongamos que tenemos un componente padre `Contador` que maneja un estado `count`, y queremos que un componente hijo `BotonIncrementar` pueda modificar este estado:

```jsx
import { useState } from "react";

function Contador() {
  const [count, setCount] = useState(0);

  function incrementar() {
    setCount(count + 1);
  }

  return (
    <div>
      <h2>Contador: {count}</h2>
      <BotonIncrementar onIncrement={incrementar} />
    </div>
  );
}

function BotonIncrementar({ onIncrement }) {
  return <button onClick={onIncrement}>Incrementar</button>;
}

export default Contador;
```

## Explicación a profundidad

1. **Definir la función en el componente padre:** La función `incrementar` está definida en `Contador` y usa `setCount` para modificar el estado.
2. **Pasar la función como prop:** Se pasa `incrementar` como la prop `onIncrement` al componente `BotonIncrementar`.
3. **Ejecutar la función desde el hijo:** `BotonIncrementar` recibe `onIncrement` y la asigna al evento `onClick` del botón.

Este patrón facilita la comunicación entre componentes y permite reutilizar componentes hijos sin que ellos manejen lógica innecesaria.

## Mitos y verdades sobre pasar funciones como props

- **"Pasar funciones como props causa problemas de rendimiento"** → **M (Mito)**  
  React maneja eficientemente la renderización, y solo en casos de alta frecuencia de renderizado puede ser un problema. Para optimizar, se pueden usar `useCallback`.

- **"Siempre es mejor manejar la lógica en el componente hijo"** → **M (Mito)**  
  En realidad, centralizar la lógica en el componente padre suele ser una mejor práctica, ya que facilita la administración del estado y la reutilización del código.

- **"Pasar funciones como props permite la comunicación de hijo a padre"** → **V (Verdad)**  
  Es uno de los patrones más comunes en React para permitir que un hijo notifique eventos o cambios al padre.

- **"Si pasas funciones como props, el hijo se vuelve dependiente del padre"** → **I (Intermedio)**  
  Aunque el hijo depende del padre para ejecutar ciertas acciones, esto no siempre es negativo. La clave está en diseñar componentes reutilizables y desacoplados en la medida de lo posible.



