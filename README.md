# Simulador-de-Aeropuerto-Proyecto-Estructuras-de-Datos
El simulador permite gestionar aviones en un aeropuerto ficticio, priorizando aterrizajes de emergencia, manejando despegues en orden FIFO, y asociando equipaje a aviones específicos. Las maletas se generan con ID únicos y se gestionan con reglas lógicas para simular operaciones reales de un aeropuerto.

Características

  - Gestión de Aterrizajes : Utiliza una cola de prioridad para aviones de emergencia (prioridad alta) y normales.
  - Gestión de Despegues : Emplea una cola FIFO para aviones en espera de despegue.
  - Equipaje : Implementa una pila LIFO asociada a cada avión, con IDs generados aleatoriamente.
  - Aviones Aterrizados : Usa un vector para acceso directo a aviones ya aterrizados, permitiendo retiro de equipaje y preparación para despegue.
  - Interfaz Interactiva : Ofrece un menú en consola para simular operaciones en tiempo real.


Instalación
  Requisitos
    1. Un compilador C++ compatible con C++ 11 o superior (por ejemplo, g++).
    2. Un sistema operativo con soporte para terminal (como Linux, macOS o Windows con MinGW).
  Pasos
    Clona el repositorio en tu máquina local:
      1.git clone https://github.com/tu-usuario/simulador-aeropuerto.git
      2. cd simulador-aeropuerto
    Compila el código fuente:
      1. g++ main.cpp -std=c++11 -o simulador
    Ejecuta el simulador:
      1. ./simulador

      
Manual de usuario
  El simulador se ejecuta en la consola y presenta un menú interactivo. A continuación, se proporciona una guía paso a paso para usar el programa:

  Inicio : Al ejecutar el programa, verás el mensaje "SIMULADOR DE AEROPUERTO" seguido del menú principal.

  Agregar Aviones a Aterrizaje :

  Opción 1: Agregar aviones normales (prioridad 2). El usuario debe ingresar el número de aviones a agregar.
  Opción 2: Agregar aviones de emergencia (prioridad 1). El usuario debe ingresar el número de  aviones de emergencia a agregar.

  Procesar Aterrizaje :

  Opción 3: Procesa el aterrizaje del avión de mayor prioridad (emergencia primero). El avión se mueve automáticamente a la lista de aviones aterrizados.

  Agregar Aviones a Despegue :

  Opción 4: Agrega aviones nuevos a la cola de despegue. El usuario debe ingresar el número de aviones a agregar.

  Procesar Despegue :

  Opción 5: Procesa el despegue del avión que está al frente de la cola.

  Agregar Equipaje :

  Opción 6: Solo disponible si hay aviones en la cola de despegue. Agrega maletas al avión que está al frente de la cola (los ID se generan automáticamente en formato "Maleta_IDAVION_XXXX").   El usuario debe ingresar el número de maletas a agregar.

  Retirar Equipaje :

  Opción 7: Permite elegir un avión que tenga equipaje y retire la maleta que está en la cima de la pila (siguiendo el principio LIFO).

  Mostrar estado :

  Opción 8: Muestra el estado actual de todas las colas y aviones en el aeropuerto.
Preparar para Despegue :

  Opción 9: Prepare múltiples aviones aterrizados que no tengan equipaje para volver a la cola de despegue. El usuario debe ingresar el número deseado de aviones a preparar.

  Salir : Opción 0 para terminar la ejecución del simulador.

Ejemplo de Uso
  - Primero, agrega 2 aviones a la cola de aterrizaje (uno normal y uno de emergencia).
  - Luego, procesa los aterrizajes: el avión de emergencia aterriza primero debido a su prioridad.
  - Agrega equipaje al avión que está en la cola de despegue.
  - Procesa el despegue del avión.
  - Retira el equipaje del avión que ha aterrizado.
  - Finalmente, preparan aviones aterrizados sin equipaje para un nuevo despegue.

  
Estructuras de datos
  El proyecto utiliza varias estructuras de datos de la STL de C++ para modelar eficientemente las operaciones del aeropuerto. A continuación, se explica el por qué y cómo se usa cada una de ellas:

1. std::queue <Avion> cola_despegue (Cola FIFO para Despegues)
  Por qué se usa : Los despegues deben procesarse en orden de llegada (primero en entrar, primero en salir), lo que simula una pista de despegue real donde los aviones esperan en fila.

  Cómo se usa :

  - Se utiliza push()para agregar aviones nuevos a la cola.
  - Se emplean front()y pop()para procesar el despegue del avión que está al frente.
  - Se accede directamente al frente para agregar equipaje sin remover el avión de la cola.

  Ventajas : Es eficiente para operaciones FIFO, con complejidad O(1) para inserción y eliminación.

2. std::priority_queue<Avion, vector <Avion> , Comparator> pq_aterrizaje (Cola de Prioridad para Aterrizajes)

  Por qué se usa : Los aterrizajes deben priorizar emergencias (aviones con prioridad 1) sobre aviones normales (prioridad 2), ya que en la vida real, las emergencias tienen precedencia para evitar riesgos.

  Cómo se usa :

  - Se utiliza push()para agregar aviones con su respectiva prioridad.
  - Se emplea top()y pop()para procesar el avión de mayor prioridad (el de menor número de prioridad).
  - Incluye un Comparador personalizado para implementar un max-heap invertido (donde menor prioridad significa mayor urgencia).

  Ventajas : Ordena automáticamente por prioridad, con complejidad O(log n) para inserción y eliminación.

3. std::vector <Avion> aviones_aterrizados (Vector para Aviones Aterrizados)
   
  Por qué se usa : Se necesita acceso directo a aviones específicos para retirar equipaje o prepararlos para despegue, lo que no es eficiente con colas. Los aviones aterrizados no siguen un orden estricto de llegada, pero requieren manipulación individual.

  Cómo se usa :

  - Se utiliza push_back()para agregar aviones después de que aterricen.
  - Se accede por índice para mostrar, retirar equipaje o preparar para despegue.
  - Se emplea erase()para remover aviones que se preparan para despegue.

  Ventajas : Proporciona acceso O(1) por índice y es flexible para operaciones que no son FIFO.

5. std::stack <string> equipaje (Pila LIFO para Equipaje por Avión)

  Por qué se usa : El equipaje se carga y descarga en orden inverso al de llegada (último en cargar, primero en descargar), lo que simula cómo se apilan maletas en un avión o en un carrito. Cada avión tiene su propia pila para asociar equipaje específico.

  Cómo se usa :

  - Se utiliza push()para agregar maletas (con ID únicos generados aleatoriamente).
  - Se emplea top()y pop()para retirar la maleta que está en la cima.
  - Se crea una copia temporal para mostrar el contenido sin modificarlo.

  Ventajas : Modela naturalmente el comportamiento LIFO, con complejidad O(1) para operaciones en la cima.

Consideraciones generales

  Eficiencia : Todas las estructuras provienen de la STL de C++, lo que las hace optimizadas para rendimiento. Por ejemplo, la prioridad_queue evita reordenamientos manuales.

  Modularidad : Cada estructura se utiliza en contextos específicos (aterrizaje, despegue, equipaje) para evitar complejidades innecesarias.

  Limitaciones : No se utilizan estructuras más avanzadas (como mapas) para mantener la simplicidad, pero permite extensiones futuras si es necesario.


Documentación del Código

  El código incluye comentarios en línea detallados en cada método y clase, explicando la lógica, parámetros y retornos. Por ejemplo:

  - Se incluyen comentarios en constructores y getters para describir su propósito.
  - Se agregan explicaciones en métodos de simulación, como "Agregar equipaje solo si hay aviones en despegue".
  - Se anotan notas sobre la complejidad y el uso de la STL.
  - Para obtener más detalles, revise directamente el archivo main.cppen el repositorio.


Autores

Juan Pablo Celis Parra - jucelisp@unal.edu.co
Frank Andres Fuentes Acero - Frfuentesa@unal.edu.co

Tutor
Jonatan Gomez Perdomo - jgomezpe@unal.edu.co
