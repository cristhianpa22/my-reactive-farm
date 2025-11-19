# Reactive Farm Deep Dive

## 1. Análisis del repositorio

El propósito general del proyecto es tener un registro de los animales por medio de tarjetas. Cada animal tiene su propia tarjeta, que contiene el nombre, la edad, la especie y el peso, lo que nos permite tener monitorizados a todos los animales. También nos permite crear nuevos animales si tenemos animales nuevos, para así tener un control óptimo de todos ellos.

<hr>

## 2. Cuestionario de React
1. ¿Cuál es la diferencia entre un componente presentacional y un componente de página en React? Da ejemplos usando archivos del proyecto.

Los componentes de presentación son los que se centran únicamente en la estética y no se encargan de ninguna funcionalidad, solo de recibir datos y funciones a través de los props y mostrarlos, y se reutilizan en muchos proyectos. En el proyecto lo podemos ver en AnimalCard, ya que este componente solo se encarga de recibir datos por props y mostrarlos; también el componente loader.jsx, y también layout.jsx.

Y los componentes de página en React son aquellos en los que ya tienen una funcionalidad y una lógica presente, como obtener datos, manejar estados, pasar los datos por props. En el proyecto podemos ver AnimalForm.jsx, que se encarga de toda la lógica de los formularios, de validar y etcétera; animalsApi.js, que se encarga de obtener los datos de la API.

<hr>

2. ¿Para qué se utiliza useState en el proyecto? Menciona dos estados distintos y su función.

Se utiliza para agregar estados a los componentes de función, permitiendo que los estados recuerden y puedan cambiar con el tiempo, haciendo que React vuelva a renderizar el componente y le asigne un nuevo valor.

const [values, setValues] = useState(initialValues): Sirve para almacenar los valores actuales de todos los campos de entrada del formulario. Contiene un objeto con cada campo del formulario, y setValues es la que actualiza el estado values cada vez que el usuario modifica un campo cuando se escucha el evento handleChange.

const [submitting, setSubmitting] = useState(false): Sirve para ver si el formulario se está enviando. Se controla por medio de un booleano con true al inicio de handleSubmit y false en el finally cuando la operación de envío finaliza. Se utiliza para deshabilitar el botón de envío para no enviar duplicados.

<hr>

3. ¿Cómo se usa useEffect para cargar datos desde MockAPI al inicio? Explica el flujo.

Se usa para que solo se ejecute después de que se monte o se renderice el componente, y nunca más. Se hace utilizando los arreglos vacíos. Lo primero que hace es mostrar un indicador de carga, que es el setLoading, después limpia cualquier error. Utiliza el try y llama a la API, pausando con el await hasta que la petición se complete, y cuando la promesa es exitosa, guarda los datos en el estado del componente. Si la llamada falla, catch muestra un error al usuario. Y así sea exitosa o errónea la petición, ejecuta finally que oculta la carga cuando haya finalizado.

<hr>

4. ¿Cómo maneja el proyecto los estados de loading, error y lista vacía? ¿Qué se muestra al usuario en cada caso?

loading: Indica si el componente está esperando una respuesta de la API para obtener la lista de animales. Se encuentra activo (true) al inicio para indicar que está cargando y pasa a false en el bloque finally cuando la operación ha terminado para ocultar el mensaje de carga.

El usuario ve un spinner de carga y un mensaje que dice "Fetching animals from the farm…" que lo renderiza el componente loader.

error: Los errores ocurren al traer los datos. Se establece un mensaje dentro de catch dentro del useEffect. Si la llamada falla, loadError contiene un mensaje y este se lo pasa al componente hijo Alert que lo muestra en la pantalla. O al intentar crear un nuevo animal, si la llamada falla, no interrumpe la visualización de la lista, solo pasa el estado por la prop a su componente hijo, mostrando un alert con el mensaje encima del formulario.

Lista vacía: Se maneja desde AnimalList, quien determina si la lista tiene 0 objetos. Si tiene, se renderizan las tarjetas, pero si está vacía, lo que hace es mostrar un mensaje de "No animals found. Add a new one to get started 🐾".

<hr>

5.¿Qué significa que un formulario sea controlado en React? Relaciónalo con el formulario del proyecto.

Significa que el estado de todos los campos de entrada del formulario son gestionados y dirigidos por el estado de React y no por el DOM. Cada campo tiene un value que lee el valor y está vinculado con un estado de React, y el onChange lo que hace es disparar un evento cada vez que se cambia el value y cambia el estado por el nuevo value. En el proyecto lo podemos ver, también todos los campos del formulario (name, type, age, weight, status) están vinculados al estado values. Cada campo tiene el atributo value que está conectado con el estado values que contiene lo que se está escribiendo o seleccionando.

<hr>

6. ¿Por qué es buena práctica separar la lógica de datos en archivos como animalsApi.js en vez de hacer peticiones dentro de los componentes?

Porque se puede reutilizar código. Si se hace dentro de un componente y se necesita obtener la misma lista en otro componente, tocaría copiar y pegar código de la petición. También para que funciones como getAnimal o createAnimal se puedan importar y reutilizar en cualquier componente. También si se quiere corregir o actualizar la URL, al estar todo junto toca modificar petición por petición; en cambio, con la separación solo se modifica el animalsApi.js.
   
7. ¿Qué hace que AnimalCard sea un componente reutilizable? ¿Cómo se podría usar una tarjeta similar en otro contexto?

Que solo es un componente estético que solo recibe datos y los pinta en la página, y no tiene ninguna funcionalidad, y puede ser utilizado múltiples veces, como se ve en el proyecto, porque mantiene el mismo diseño, solo cambian los datos que recibe y los muestra sin ninguna dificultad. Se puede utilizar en otras cosas como un card de productos. Deja la misma estructura, solo se le cambia el texto del strong, como por ejemplo por title, category, Price, stock, etcétera, y se cambian los estatus por unos que se adapten al contexto, y ya se podría reutilizar en múltiples contextos.

<hr>
   
8. ¿Qué elementos del proyecto contribuyen a la accesibilidad? Menciona tres y explica su importancia.

-Los label están conectados a los campos de entrada de datos, lo que permite a los usuarios de lectores de pantalla escuchar la descripción que tiene el label al enfocarse en el campo.

-aria-invalid: Ayuda a los usuarios de lector de pantalla, ya que le dice al usuario que el valor no cumple con la validación del dato, y ayuda a que las personas con baja visión sepan que deben revisar ese campo, ya que hay un error y debe ser corregido.

-focus tailwind: Permite a los usuarios de teclado moverse por toda la página e interactuar con ella por medio del tab, mostrando un anillo o borde alrededor de él.

<hr>

9. Antes de agregar una funcionalidad nueva, ¿qué pasos debes pensar según la filosofía de React? (ej.: qué datos, qué estado, dónde vive la lógica)

10. Descomponer la UI en una Jerarquía de Componentes El primer paso es mirar la nueva funcionalidad (o la vista completa) y dividirla en una jerarquía de componentes individuales y reutilizables.

-Identificación: Dibuja la UI y encierra cada parte que sea independiente y potencialmente reutilizable (botones, listas, formularios, etc.) con una caja.

-Jerarquía: Determina qué componentes viven dentro de otros.

-Clasificación: Decide si cada componente será Presentacional (solo muestra datos/estilos) o Contenedor (gestiona la lógica/estado).

2. Definir los Datos Una vez que tienes la estructura, identifica exactamente qué información necesita la funcionalidad.

-Datos Necesarios: ¿Qué datos estáticos o dinámicos se requieren para renderizar la UI?

-Fuente de Datos: ¿De dónde vienen estos datos?

  Props: Datos que vienen del componente padre.

  Estado Local (useState): Datos que cambian con la interacción del usuario (ej. valor de un     campo, estado de un dropdown).

  Estado Global (APIs / Context / Redux): Datos que se obtienen de una API o que son necesarios en muchos lugares.

3. Definir el Estado Mínimo y Completo Identifica el estado mínimo necesario para que la UI funcione.

-Principio de Eliminación: El estado es todo lo que la funcionalidad necesita "recordar" que puede cambiar con el tiempo. Si un dato puede ser calculado a partir de props o de otro estado (estado derivado), no debe ser parte del estado.

-Ejemplo: No guardas el nombre y el apellido por separado si puedes derivar el nombre completo.

4. Determinar Dónde Debe Vivir el Estado 📍 Este paso, llamado "State Colocation" o "Levantamiento de Estado (Lifting State Up)", es crucial para la eficiencia.

-Regla: El estado debe vivir en el componente más bajo posible en la jerarquía que necesita ese estado o necesita pasar ese estado a sus hijos.

-Lifting State Up: Si dos componentes hermanos necesitan compartir o modificar el mismo estado, el estado debe moverse hacia arriba, a su ancestro común más cercano (el componente padre).

-Añadir la Lógica de Flujo de Datos Finalmente, implementa la forma en que los componentes presentacionales modifican el estado del componente contenedor.

5. Paso de Handlers: Pasa las funciones de actualización de estado (set... handlers) como props a los componentes hijos (Data Down, Actions Up).

<hr>

10. ¿Qué conceptos de React aprendidos en este proyecto podrías reutilizar en otro tipo de aplicación? Da al menos cuatro ejemplos.

-En cómo debe ser la estructura del proyecto, como que la petición de datos de la API debe ser separada y no en un componente.

-En cómo se hace la validación de los formularios para que informe al usuario de los errores al ingresar datos en los campos y los elementos accesibles que debe contener un formulario.

-El manejo de errores para los múltiples errores que se presenten en la aplicación y las cargas mientras que se cargan los datos.

-La utilización de useEffect para la carga inicial de datos de la API.

## 3. Actividades prácticas

## Actividad 1: Modificar y explicar un estado

El estado que modifiqué fue que filtra el tipo de animal.

```bash
const [typeFilter, setTypeFilter] = useState("sheep");
```
Lo cambié para que, en vez de que al inicio se muestren todos los tipos de animales, solo se muestre la oveja. Lo que sucede en la interfaz es que, al entrar, en vez de mostrar todos los animales o tarjetas, solo me filtra o me muestra la oveja. Solo se renderizan las cards que cumplan con el tipo, que es la oveja.

Al finalizar la petición de la API con éxito, se llama a data, que tiene los objetos, y se filtra. Se da un true para los animales que cumplen con el typeFilter "sheep".

<img width="1820" height="916" alt="image" src="https://github.com/user-attachments/assets/82e10578-8006-4bf6-b6a3-922f10d622e2" />

<hr>

## Actividad 2: Agregar un filtro nuevo

El estado se agregó con los demás estados de filtros de la siguiente manera:

```bash
const [weightFilter, setWeightFilter] = useState("");
```

Lo que hace es almacenar el texto del input que el usuario escribe en el filtro con el peso mínimo. Se inicializa como una cadena vacía, lo que hace que, por defecto, no se aplique ningún filtro.

Lógica : 
-weightFilter es una cadena de texto. Para tener un valor numérico, lo que se hace es convertir weightFilter a Number en la constante minWeight. Si el campo está vacío o no es válido, el valor se establece como un cero (0).

```bash
const minWeight = Number(weightFilter) || 0;
```

- La condición de filtrado es: si minWeight es 0, sería true, haciendo que byWeight sea true para todos los animales y se salte el filtro porque todos los animales cumplen con la condición. Ahora, si minWeight es $> 0$, se pasa a la segunda parte, haciendo que sea true solo para los animales cuyo peso sea mayor o igual al peso mínimo ingresado por el usuario. 

```bash
  const byWeight = minWeight === 0 || a.weight >= minWeight;
```

Lista:
La lista se ve afectada cada vez que el usuario escribe un número en el input de "Min. Weight". La función setWeightFilter se llama, lo que provoca que se actualice el estado y se renderice. React, al detectar el cambio y volver a ejecutar la función de filtrado, hace que el componente AnimalList reciba la nueva lista filteredAnimals y se la pase al componente hijo AnimalCard, y este las renderice.
  
  <img width="1831" height="921" alt="image" src="https://github.com/user-attachments/assets/66f4019e-42b1-43a3-9acc-f02f312a39dc" />
  
<hr>

## Actividad 3: Mejorar el formulario

-required: Se cambió el span del label: en vez de solo tener un asterisco para decirle al usuario que ese campo es obligatorio, se le agregó la palabra "required" a cada campo obligatorio. Esto mejora la experiencia del usuario en el sentido de que los usuarios que no entiendan el contexto o no estén familiarizados con el asterisco se les dice que este campo es obligatorio, lo que hace que el usuario entienda desde el principio que el campo es obligatorio y lo debe llenar.

-autofocus: Se le puso un autofocus al campo de name para que, al entrar, sea lo primero que se selecciona. Esto ayuda mucho a usuarios con lectores de pantalla o de teclado para que no tengan que presionar el tab para comenzar a agregar un nuevo animal y facilitarles la vida, y también que los usuarios de teclado ubiquen dónde está ubicado el cursor al entrar.

<img width="1828" height="913" alt="image" src="https://github.com/user-attachments/assets/4c391c3b-5e48-46c5-9604-584d3c9bd0f0" />


  





