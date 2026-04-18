
# Lab04: Visualización de Datos en Raspberry Pi con Node-RED 

## Integrantes

Sharom Lizeth Cortes Reyes,
Juan David Romero Perez,
Daniel Duarte Borhoquez.

## Documentación
El proyecto se basa en el ecosistema de Node.js, utilizando Node-RED como orquestador visual. Esta herramienta permite transformar eventos físicos o de interfaz en datos digitales procesables. La arquitectura sigue un modelo de flujo de datos donde un nodo de entrada (Selector de color) genera un mensaje que es transformado y almacenado para su posterior análisis mediante un script de Python, demostrando la interoperabilidad entre herramientas de bajo y alto nivel.

Para la implementación, se realizó la instalación de Node-RED mediante una conexión SSH, optimizando el uso de CPU y RAM de la Raspberry Pi. Se configuró el inicio automático del servicio mediante el comando `sudo systemctl enable nodered.service`, garantizando la persistencia del flujo tras reinicios. El flujo diseñado integra nodos de `dashboard` para la interfaz de usuario, un nodo `function` para el formateo de datos RGB y un nodo `file` que escribe los valores en el sistema de archivos, permitiendo que un script externo de Python lea y procese la información en tiempo real.

## Diagrama de flujo
graph TD
    %% Define los estilos para mejorar la visualización
    classDef hardware fill:#f9f,stroke:#333,stroke-width:2px;
    classDef software fill:#bbf,stroke:#333,stroke-width:1px;
    classDef data fill:#dfd,stroke:#333,stroke-width:1px,stroke-dasharray: 5 5;

    Start((Inicio)) --> SSH[Conexión SSH a Raspberry Pi]
    SSH --> Install[Instalación de Node.js, npm y Node-RED]
    Install --> ConfigBoot[Configurar Node-RED on Boot<br/>sudo systemctl enable nodered.service]
    ConfigBoot --> Reboot[Reiniciar Raspberry Pi]

    Reboot --> AccessWeb[Acceder a Interfaz Web<br/>http://ip:1880]

    subgraph "Flujo en Node-RED (Lógica)"
        AccessWeb --> UI_Color[Node: Color Picker<br/>(Selección de Color)]
        UI_Color -->|msg.payload| Fn_Format[Node: Function<br/>(Formatear RGB)]
        Fn_Format --> UI_Text[Node: Text Input<br/>(Visualizar RGB)]
        Fn_Format --> File_Write[Node: File<br/>(Guardar en archivo .txt)]
    end

    File_Write -->|Escribe| TXT_File[Archivo de Texto]
    TXT_File -.->|Lectura| Python_Script[Script en Python 3]

    Python_Script --> ProcessData[Procesar valores RGB]
    ProcessData --> End((Fin))

    %% Asignación de estilos
    class SSH,Install,ConfigBoot,Reboot,TXT_File hardware;
    class AccessWeb,UI_Color,Fn_Format,UI_Text,File_Write,Python_Script,ProcessData software;
    class TXT_File data;

## Conclusiones
* **Persistencia y Autonomía:** Al configurar Node-RED como un servicio del sistema (`systemd`), se logra que la aplicación sea independiente de la terminal de comandos, permitiendo que la Raspberry Pi actúe como un servidor IoT robusto y siempre disponible.
* **Eficiencia en el Desarrollo:** La integración de `node-red-dashboard` facilita la creación de interfaces de control complejas (GUI) sin necesidad de programar HTML/CSS desde cero, agilizando el despliegue de soluciones de visualización.
* **Interoperabilidad:** Se demostró que el uso de archivos de texto como puente de datos es un método efectivo para comunicar flujos visuales con lógica de programación en Python, permitiendo aprovechar las librerías de procesamiento de ambos entornos.
* **Optimización de Hardware:** La limitación de memoria a 256MB y el acceso vía terminal aseguran que el sistema se mantenga estable incluso en modelos de Raspberry Pi con recursos limitados, evitando cuellos de botella durante la ejecución de procesos.