# Corrutinas de Kotlin en Android
## Una corrutina es un patrón de diseño de simultaneidad que puedes usar en Android para simplificar el código que se ejecuta de forma asíncrona.
## Las corrutinas son nuestra solución recomendada para la programación asíncrona en Android. Las funciones más importantes son las siguientes:
- Ligereza 
- Menos fugas de memoria
- Compatibilidad integrada con la cancelación
- Integración con Jetpack

## Dependencias
    dependencies {
        implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.9")
    }

## 2. Componentes Principales
### a. CoroutineScope
Define un ámbito (scope) donde las corrutinas pueden ejecutarse.
Se utiliza para lanzar y gestionar corrutinas.

Ejemplo:

    val scope = CoroutineScope(Dispatchers.Main)

### b. launch y async
launch: Lanza una corrutina sin esperar su resultado (de tipo Job).
async: Lanza una corrutina que devuelve un valor de tipo Deferred, que se puede esperar con await().

    // Lanzando una corrutina
    scope.launch {
        // código a ejecutar en un hilo en segundo plano
    }

    // Lanzando una corrutina con resultado
    scope.async {
        // código que retorna un valor
        return@async "Resultado"
    }
### c. suspend
Funciones suspendidas son funciones que pueden suspenderse y reanudarse sin bloquear el hilo.
Se usan dentro de corrutinas y permiten operaciones asíncronas (por ejemplo, acceso a la red, bases de datos).

    suspend fun getData(): String {
        delay(1000)  // Simula una tarea asíncrona
        return "Datos"
    }
## 3. Tipos de Dispatcher
Los Dispatchers se utilizan para indicar en qué hilo o contexto debe ejecutarse la corrutina:

- Dispatchers.Main: Hilo principal (UI).
- Dispatchers.IO: Para operaciones de entrada/salida (red, bases de datos, archivos).
- Dispatchers.Default: Para tareas de cómputo intensivo.
- Dispatchers.Unconfined: No está confinado a ningún hilo en particular.

      CoroutineScope(Dispatchers.Main).launch {
          // Código que se ejecutará en el hilo principal
      }
      
      CoroutineScope(Dispatchers.IO).launch {
          // Operaciones de red o I/O
      }
## 4. Funciones Asíncronas Comunes
### a. delay()
Suspende la ejecución de una corrutina por un tiempo determinado (no bloquea el hilo).

    delay(2000)  // Suspende la corrutina por 2 segundos
### b. withContext()
Cambia el contexto de ejecución de una corrutina a otro Dispatcher.

    val result = withContext(Dispatchers.IO) {
        // Código de I/O que se ejecuta en un hilo distinto
        fetchDataFromServer()
    }
## 5. Gestión de Excepciones en Corrutinas
Las corrutinas permiten capturar excepciones de manera sencilla con el uso de bloques try-catch.

    scope.launch {
        try {
            val data = fetchDataFromServer()
        } catch (e: Exception) {
            // Manejo de excepciones
        }
    }
## 6. Cancelación de Corrutinas
Las corrutinas pueden ser canceladas de manera cooperativa utilizando el método cancel().

    val job = scope.launch {
        // Tarea que puede ser cancelada
        delay(5000)
    }
    
    // Cancelando la corrutina
    job.cancel()
