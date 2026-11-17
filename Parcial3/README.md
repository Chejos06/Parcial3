# Sistema de Inventario de Zapatos — SQLAlchemy ORM

## 1. Información del proyecto

| Información | Descripción |
|---|---|
| Estudiante | Chenier Jose Ojeda Gomez|
| Asignatura | Desarrollo de aplicaciones con acceso de datos |
| Docente | Eduardo Pimienta |
| Fecha | 17/11/2026 |
| Lenguaje de programación | Python |
| ORM utilizado | SQLAlchemy |
| Base de datos | SQLite |

## 2. Descripción del proyecto

**¿En qué consiste el programa?**

En un pograma que administra el inventario de una pequeña tienda de zapatos

**¿Qué funcionalidades tiene?**

Describe brevemente las operaciones que permite realizar el sistema:

- Registrar zapatos.
Registra el zapato con su modelo, talla y precio
- Consultar el inventario.
Consulta y muestra los zapatos que han sido registrados
- Buscar zapatos por ID.
Busca el zapato registrado mediante su ID
- Actualizar precios.
Modifica datos de un zapato ya registrado
- Eliminar zapatos.
Permite eliminar un zapato

## 3. Estructura del proyecto

Presenta la estructura de archivos de tu aplicación.

```text
tienda_zapatos/
├── main.py
├── datos.py
├── requirements.txt
├── README.md
└── evidencias/
```

**Explicación de los archivos:**

| Archivo | ¿Cuál es su responsabilidad? |
|---|---|
| `main.py` | Menu e interracion con el usuario |
| `datos.py` | Funciones |
| `requirements.txt` | libreria y version a utlizar |
| `zapateria.db` | hace que funciones como registrar permita que los datos se aguarden y se matengan, hace que no se pierda la informacion sino que permanezca,se genera a travez de sqlalchemy importando el create_engine |

## 4. Instalación y ejecución

Explica los pasos para ejecutar el proyecto en otro computador.

**Paso 1. Requisitos**
[Indica los programas y herramientas necesarios.]
main.py
datos.py
Python
Visual Studio Code
SQLAlchemy 2.0
SQLite
Pip
Terminal o consola

**Paso 2. Instalación de dependencias**

```bash
python -m pip install -r requirements.txt
```

**Paso 3. Ejecución**

```bash
python main.py
```

**Paso 4. Resultado esperado**

Que aparezca el menu con las funciones y que cada una funcione correctamente  y sobretodo se guarde la informacion.

---

# 5. Documentación técnica — Preguntas obligatorias

Responde las siguientes preguntas utilizando tu implementación. Cuando sea necesario, incluye fragmentos de tu código para explicar el funcionamiento.

### Pregunta 1. Modelo y tabla

¿Qué clase de Python representa la tabla de la base de datos? Explica cómo SQLAlchemy identifica el nombre de la tabla y sus columnas.

**Respuesta:**

Utiliza la clase Zapato como representación de la tabla zapatos donde cada atributo de esa clase representa una columna de la tabla, permitiendo trabajar con los registros como objetos de Python sin necesidad de escribir sentencias SQL manualmente.

**Fragmento de código relacionado:**

class Zapato(Base):
  __tablename__ = "zapatos"

  id: Mapped[int] = mapped_column(primary_key=True, autoincrement=True)
  modelo: Mapped[str] = mapped_column(String(50), nullablel=False)
  talla: Mapped[int] = mapped_column(nullablel=False)
  precio: Mapped[float] = mapped_column(nullablel=False)

class que vendria siendo la clase Zapato, donde creamos la tabla llamada zapatos con el __tablename__ y de ahi definimos sus atributos

### Pregunta 2. Creación de la base de datos

¿Qué ocurre cuando ejecutas el programa por primera vez? Explica cómo se crea la base de datos `zapateria.db` y la tabla `zapatos`.

**Respuesta:**

Cuando ejecuto el programa por primera vez, se inicializa la conexion con la base de datos SQLite mediante create_engine().

**Fragmento de código relacionado:**

from sqlalchemy import create_engine
engine = create_engine("sqlite:///zapateria.db", echo=True)
...
Base.metadata.create_all(engine)

Esta instruccion comprueba si existen las tablas definidas en los modelos de SQLAlchemy, si no existen, las crea automaticamente, se crea el archivo zapateria.db y dentro de el la tabla zapatos, con sus respectivas columnas.


### Pregunta 3. Registro de zapatos

Cuando seleccionas la opción de registrar un zapato, ¿qué recorrido siguen los datos desde el menú hasta quedar almacenados en SQLite? Identifica las funciones utilizadas.

Cuando selecciono la opción de registrar un zapato en el menú de main.py, el programa solicita los datos del producto: modelo, talla y precio.

Estos datos se envian a la funcion crear_zapato() ubicada en datos.py.

Se abre una sesion de SQLAlchemy mediante Session(engine).
Se crea un objeto de la clase Zapato con los datos recibidos.
Se utiliza session.add() para agregar el objeto a la sesion.
Se utiliza session.commit() para confirmar la transaccion y guardar los datos permanentemente en SQLite.
La funcion devuelve True si el registro se realizo correctamente.

**Fragmento de código relacionado:**

if opcion == "1":
                modelo = input("Modelo: ").strip()
                talla = int(input("Talla: "))
                precio = float(input("Precio: "))

                if not modelo or talla <= 0 or precio <= 0:
                    print("Datos no válidos.")
                    continue

                if datos.crear_zapato(modelo, talla, precio):
                    print("Zapato registrado correctamente.")
                else:
                    print("No se pudo registrar.")

### Pregunta 4. Sesiones y transacciones

¿Para qué se utiliza una sesión de SQLAlchemy? ¿Qué ocurriría si agregas o modificas un objeto, pero no confirmas la transacción con `commit()`?

**Respuesta:**

Una sesion de SQLAlchemy se utiliza para establecer una comunicacion entre el programa y la base de datos, permitiendo realizar operaciones como insertar, consultar, modificar y eliminar registros. Si agrego o modifico un objeto, pero no ejecuto commit(), los cambios no quedan confirmados ni guardados permanentemente en la base de datos.

### Pregunta 5. Búsqueda por ID

¿Cómo funciona la búsqueda de zapatos por ID? Explica qué devuelve tu función cuando encuentra el registro y qué devuelve cuando no existe.

**Respuesta:**

utiliza el metodo session.get() de SQLAlchemy.

**Fragmento de código relacionado:**

zapato = session.get(Zapato, id_zapato)

Este metodo busca un registro utilizando su clave primaria que es el atributo id, Si encuentra el registro,devuelve el objeto Zapato con toda su informacion. Si no encuentra ningun registro con ese ID, devuelve None.

### Pregunta 6. Actualización de precios

¿Cómo se actualiza el precio de un zapato mediante el ORM? Explica cómo se obtiene el objeto, cómo se modifica su atributo y cómo se guarda el cambio.

**Respuesta:**

La actualizacion se realiza mediante la funcion actualizar_precio(id_zapato, nuevo_precio).

**Fragmento de código relacionado:**

zapato.precio = nuevo_precio

session.commit()

Esta instruccion confirma los cambios y guarda el nuevo precio en SQLite. Si el zapato no existe, la funcion devuelve False. Si se actualiza correctamente, devuelve True.

### Pregunta 7. Eliminación de registros

¿Qué ocurre cuando eliminas un zapato? Explica cómo compruebas que el registro ya no existe en la base de datos.

**Respuesta:**

Cuando elimino un zapato, el programa utiliza la funcion eliminar_zapato(id_zapato).

**Fragmento de código relacionado:**

session.delete(zapato)
session.commit()

La primera instruccion marca el objeto para su eliminacion y la segunda confirma la transaccion, eliminando el registro de la base de datos.

### Pregunta 8. Persistencia de datos

¿Qué sucede con los zapatos registrados cuando cierras y vuelves a ejecutar el programa? Explica por qué permanecen almacenados.

**Respuesta:**

Cuando cierro el programa y vuelvo a ejecutarlo, los zapatos que registre anteriormente permanecen almacenados en la base de datos. Esto ocurre porque estoy utilizando SQLite, que almacena la informacion de manera persistente en un archivo llamado zapateria.db, lo que hace que permanezcan guardados en el archivo de la base de datos.

---

## 6. Pruebas de funcionamiento y evidencias

Debes ejecutar el programa y demostrar el funcionamiento de cada una de las operaciones.

### Prueba 1. Registrar zapatos

- **Acción realizada:** registrar dos zapatos.
- **Datos utilizados:** [Indicar modelo, talla y precio].
- **Resultado obtenido:** [Explicar qué ocurrió].

**Evidencia:**

![Registro de zapatos](evidencias/registro.png)

### Prueba 2. Listar inventario

- **Acción realizada:** consultar todos los zapatos.
- **Resultado obtenido:** [Explicar qué mostró el programa].

**Evidencia:**

![Listado de zapatos](evidencias/listado.png)

### Prueba 3. Buscar por ID

- **ID utilizado:** [Indicar ID].
- **Resultado obtenido:** [Explicar qué ocurrió].

**Evidencia:**

![Búsqueda de zapatos](evidencias/busqueda.png)

### Prueba 4. Actualizar precio

- **Zapato seleccionado:** [Indicar].
- **Precio anterior:** [Indicar].
- **Precio nuevo:** [Indicar].
- **Resultado obtenido:** [Explicar qué ocurrió].

**Evidencia:**

![Actualización de precio](evidencias/actualizacion.png)

### Prueba 5. Eliminar zapato

- **ID eliminado:** [Indicar ID].
- **Resultado obtenido:** [Explicar cómo verificaste la eliminación].

**Evidencia:**

![Eliminación de zapato](evidencias/eliminacion.png)

### Prueba 6. Comprobar persistencia

- **Acción realizada:** cerrar y ejecutar nuevamente el programa.
- **Resultado obtenido:** [Explicar cuáles registros permanecieron guardados].

**Evidencia:**

![Persistencia de datos](evidencias/persistencia.png)

### Pruebas con registros inexistentes

Ejecuta las operaciones de buscar, actualizar y eliminar utilizando un ID que no exista.

| Operación | ¿Qué sucedió? |
|---|---|
| Buscar ID inexistente | aparece un mensaje Zapato no encontrado |
| Actualizar ID inexistente | sale Zapato no encontrado.|
| Eliminar ID inexistente | aparece Zapato no encontrado. |

## 7. Conclusiones

Responde brevemente:

1. ¿Qué aprendiste al implementar las operaciones CRUD utilizando SQLAlchemy?
Aprendi a utilizar SQLAlchemy ORM para gestionar una base de datos desde Python mediante las operaciones CRUD: crear, consultar, actualizar y eliminar registros. A definir modelos que representan tablas, utilizar sesiones para interactuar con la base de datos y confirmar los cambios mediante commit().
2. ¿Qué diferencia identificaste entre trabajar con objetos Python y escribir sentencias SQL directamente?
Que trabajar con objetos Python mediante SQLAlchemy es mas sencillo y practico, porque permite representar los registros de la base de datos como objetos y manipular sus atributos directamente. En cambio, al utilizar SQL directamente, es necesario escribir consultas y sentencias especificas para cada operacion. Considero que trabajar con ORM permite tener un codigo mas organizado, legible y facil de mantener.
3. ¿Cuál fue la principal dificultad durante el desarrollo y cómo la solucionaste?
La principal dificultad fue comprender como utilizar las sesiones de SQLAlchemy y como realizar correctamente las operaciones sobre los registros de la base de datos.

## Indicaciones de documentación

- Responde las ocho preguntas con tus propias palabras.
- Relaciona tus explicaciones con el código que implementaste.
- Incluye fragmentos de código únicamente cuando ayuden a explicar tu respuesta.
- Todas las evidencias deben corresponder a ejecuciones reales del programa.
- Guarda las capturas dentro de la carpeta `evidencias/` utilizando los nombres indicados o actualiza las rutas del README.
- Verifica que las imágenes se visualicen correctamente en GitHub.
- Elimina los textos entre corchetes y reemplázalos con tus respuestas antes de entregar.
- La documentación debe permitir que otra persona comprenda cómo instalar, ejecutar y utilizar tu aplicación.