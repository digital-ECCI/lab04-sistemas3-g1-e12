
# Lab04: Visualización de Datos en Raspberry Pi con Node-RED 

## Integrantes

Sharom Lizeth Cortes Reyes,
Juan David Romero Perez,
Daniel Duarte Borhoquez.

## Documentación
El proyecto se basa en el ecosistema de Node.js, utilizando Node-RED como orquestador visual. Esta herramienta permite transformar eventos físicos o de interfaz en datos digitales procesables. La arquitectura sigue un modelo de flujo de datos donde un nodo de entrada (Selector de color) genera un mensaje que es transformado y almacenado para su posterior análisis mediante un script de Python, demostrando la interoperabilidad entre herramientas de bajo y alto nivel.

Para la implementación, se realizó la instalación de Node-RED mediante una conexión SSH, optimizando el uso de CPU y RAM de la Raspberry Pi. Se configuró el inicio automático del servicio mediante el comando `sudo systemctl enable nodered.service`, garantizando la persistencia del flujo tras reinicios. El flujo diseñado integra nodos de `dashboard` para la interfaz de usuario, un nodo `function` para el formateo de datos RGB y un nodo `file` que escribe los valores en el sistema de archivos, permitiendo que un script externo de Python lea y procese la información en tiempo real.

## Conclusiones
* **Persistencia y Autonomía:** Al configurar Node-RED como un servicio del sistema (`systemd`), se logra que la aplicación sea independiente de la terminal de comandos, permitiendo que la Raspberry Pi actúe como un servidor IoT robusto y siempre disponible.
* **Eficiencia en el Desarrollo:** La integración de `node-red-dashboard` facilita la creación de interfaces de control complejas (GUI) sin necesidad de programar HTML/CSS desde cero, agilizando el despliegue de soluciones de visualización.
* **Interoperabilidad:** Se demostró que el uso de archivos de texto como puente de datos es un método efectivo para comunicar flujos visuales con lógica de programación en Python, permitiendo aprovechar las librerías de procesamiento de ambos entornos.
* **Optimización de Hardware:** La limitación de memoria a 256MB y el acceso vía terminal aseguran que el sistema se mantenga estable incluso en modelos de Raspberry Pi con recursos limitados, evitando cuellos de botella durante la ejecución de procesos.