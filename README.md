# Prototipo de Asistente de IA para gvSIG Desktop (Jython)

> **⚠️ Repositorio Archivado: Prototipo Educativo ⚠️**
>
> Este código es el prototipo inicial descrito en la serie de artículos sobre la integración de IA en gvSIG Desktop. **No está mantenido y no es para uso en producción.** Su propósito es puramente educativo y sirve como acompañamiento a los artículos que narran el viaje de desarrollo.

## Propósito de este Repositorio

Este repositorio contiene el código fuente del primer prototipo funcional que permitió "hablar" con la API de Gemini desde gvSIG Desktop. Fue desarrollado en pocos días usando **Jython** y librerías estándar de Java para validar la idea principal.

El código aquí presente corresponde a la fase descrita en el artículo:
*   **"[Cómo empecé a hablar con la IA desde la aplicación de escritorio gvSIG desktop en Java](https://blog.gvsig.org/2025/09/08/como-empece-a-hablar-con-la-ia-desde-la-aplicacion-de-escritorio-gvsig-desktop/)"**

## Contexto y Evolución

Aunque este prototipo fue funcional y demostró la viabilidad del concepto, sus limitaciones llevaron a una reescritura completa en Java puro. Esta evolución se detalla en el siguiente artículo de la serie:
*   **"[De chat a asistente, creando un conjunto de herramientas contextuales para gvSIG-desktop](https://blog.gvsig.org/2025/09/15/de-chat-a-asistente-creando-un-conjunto-de-herramientas-contextuales-para-gvsig-desktop/)"**

## Instalación y Uso (Contexto de Prototipo)

Las siguientes instrucciones describen cómo se instalaba y configuraba el prototipo en su entorno original. Se proporcionan únicamente con fines educativos.

1.  **Prerrequisitos:**
    *   Una instalación funcional de gvSIG Desktop con el intérprete de Jython configurado.
    *   Una API Key válida para la API de Google Gemini.
    *   (Opcional) Una instalación local de [PlantUML](https://plantuml.com/) si se desea utilizar el procesador de diagramas.

2.  **Configuración de la API Key:**
    *   En el directorio `HOME` de tu sistema operativo, crea (o edita) un fichero llamado `.gvsig-devel.properties`.
    *   Añade la siguiente línea al fichero, reemplazando `TU_API_KEY_AQUI` por tu clave real:
        ```properties
        gemini_2_5_flash_api_key=TU_API_KEY_AQUI
        ```

3.  **Instalación del Script:**
    *   Copia el directorio completo `chatagent` (que contiene todos los ficheros `.py` del prototipo) dentro del directorio de `scripts` de tu instalación de gvSIG.

4.  **Ejecución:**
    *   Una vez iniciado gvSIG, el asistente debería estar disponible en el menú de herramientas, bajo una entrada similar a "Herramientas > Chat con el asistente virtual".

## Estructura del Código Fuente

El prototipo se organizó siguiendo una arquitectura modular para separar las responsabilidades. Los componentes clave son:

*   **`chatagent/chatagent.py`:** El punto de entrada principal que inicializa la aplicación y registra los distintos "procesadores" de respuestas.

*   **`chatagent/chat_panel.py`:** Contiene toda la lógica de la interfaz de usuario (UI) construida con Swing. Gestiona la interacción, el historial del chat y el uso de `SwingWorker` para evitar congelar la aplicación durante las llamadas a la IA.

*   **`chatagent/aiclients/gemini.py`:** Un cliente de API encapsulado y robusto para comunicarse con Google Gemini. Gestiona el historial de la conversación, la construcción del payload JSON y las peticiones HTTP usando librerías estándar de Java.

*   **`chatagent/processors/`:** El corazón de la lógica de negocio. Es un sistema extensible donde cada subdirectorio implementa un procesador para un tipo de respuesta específico:
    *   **`text_processor`:** Maneja respuestas de texto simples.
    *   **`sql_processor`:** Ejecuta consultas SQL contra la base de datos conectada y muestra los resultados en una tabla.
    *   **`plantuml_processor`:** Invoca a la herramienta externa PlantUML para generar diagramas (ej. ERD) a partir de texto.
    *   **`chart_processor`:** La implementación más compleja. Utiliza un ingenioso sistema de 3 pasos donde el LLM genera una consulta SQL, describe su `ResultSet` y finalmente escribe una función Jython para crear un gráfico con la librería XChart.

*   **`chatagent/gvsigdesktop/`:** La capa de integración con gvSIG. Contiene utilidades clave para obtener el DDL de las tablas, añadir acciones a la barra de herramientas o interactuar con el contexto de la aplicación.

## Lo que puedes aprender de este código

*   Un ejemplo real de prototipado rápido en un entorno Java/Jython.
*   Cómo realizar llamadas a una API REST moderna (Gemini) usando solo librerías estándar de Java (`HttpURLConnection`, `javax.json`).
*   La implementación inicial de un sistema de "prompting estructurado" para forzar salidas JSON.
*   El concepto de "procesadores" para manejar diferentes tipos de respuestas de la IA de forma modular y extensible.

## Licencia

Este proyecto se distribuye bajo la Licencia Pública General de GNU v3.0 (GPLv3). Consulta el fichero `LICENSE` para más detalles.

