# Lab04: Visualización de Datos en Raspberry Pi con Node-RED 

## Integrantes

Sharom Lizeth Cortes Reyes,
Juan David Romero Perez,
Daniel Duarte Borhoquez,
Andres Javier Vargas Manrique.

## Documentación
El proyecto se basa en el ecosistema de Node.js, utilizando Node-RED como orquestador visual. Esta herramienta permite transformar eventos físicos o de interfaz en datos digitales procesables. La arquitectura sigue un modelo de flujo de datos donde un nodo de entrada (Selector de color) genera un mensaje que es transformado y almacenado para su posterior análisis mediante un script de Python, demostrando la interoperabilidad entre herramientas de bajo y alto nivel.

Para la implementación, se realizó la instalación de Node-RED mediante una conexión SSH, optimizando el uso de CPU y RAM de la Raspberry Pi. Se configuró el inicio automático del servicio mediante el comando `sudo systemctl enable nodered.service`, garantizando la persistencia del flujo tras reinicios. El flujo diseñado integra nodos de `dashboard` para la interfaz de usuario, un nodo `function` para el formateo de datos RGB y un nodo `file` que escribe los valores en el sistema de archivos, permitiendo que un script externo de Python lea y procese la información en tiempo real.

## Diagrama de flujo
![Diagrama de flujo Node-RED](img/diagrama.png)

## Descripción de los Nodos

A continuación, se detalla la función de cada nodo utilizado en el flujo de Node-RED para procesar y almacenar los datos de color:

| Nodo | Función Principal | Descripción Técnica |
| :--- | :--- | :--- |
| **Function** | Procesamiento | Transforma los valores RGB recibidos del selector en una cadena de texto legible. |
| **Debug** | Monitoreo | Permite visualizar en la consola lateral de Node-RED los datos que fluyen en tiempo real para depuración. |
| **File** | Almacenamiento | Guarda los valores formateados en un archivo `.txt` de forma persistente para su uso en Python. |
| **Text Input** | Interfaz | Muestra el valor final en el Dashboard o permite la interacción manual con los datos. |

## Funcionamiento y Diagnóstico

### Flujo de Datos
El sistema opera bajo una arquitectura de flujo continuo dividida en tres etapas principales:

1. Captura: El proceso inicia cuando el usuario interactúa con la interfaz web y selecciona un color específico mediante el nodo Color Picker.
2. Procesamiento: El nodo Function intercepta el mensaje original, extrae los componentes RGB y los transforma en una cadena de texto formateada (ej. `R=255 G=0 B=0`).
3.  Salida: La información procesada se distribuye simultáneamente a tres destinos:
    * Panel de Debug: Para el monitoreo técnico y control de errores.
    * Nodo File Para el registro permanente en el sistema de archivos (puente con Python).
    * Text Input: Para la visualización directa del resultado en el dashboard del usuario.

---
## Flujo Node-RED
![Flujo Node-RED](img/Node-Red.jpeg)

## Conclusiones

* **Persistencia y Autonomía:** Al configurar Node-RED como un servicio del sistema (`systemd`), se logra que la aplicación sea independiente de la terminal de comandos, permitiendo que la Raspberry Pi actúe como un servidor IoT robusto y siempre disponible tras reinicios.

* **Eficiencia en el Desarrollo:** La integración de `node-red-dashboard` facilita la creación de interfaces de control complejas (GUI) sin necesidad de programar HTML/CSS desde cero, agilizando significativamente el despliegue de soluciones de visualización profesional.

* **Interoperabilidad:** Se demostró que el uso de archivos de texto como puente de datos es un método efectivo y ligero para comunicar flujos visuales con lógica de programación en Python, permitiendo aprovechar las fortalezas de procesamiento de ambos entornos.

* **Optimización de Hardware:** La limitación de memoria a 256MB y el acceso exclusivo vía terminal (SSH) aseguran que el sistema se mantenga estable incluso en modelos de Raspberry Pi con recursos limitados, evitando cuellos de botella y maximizando el rendimiento del hardware.
