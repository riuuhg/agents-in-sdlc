---
title: "Ejercicio 2 - Añadir funciones con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen implicar cambios en varios archivos y tareas como ejecutar pruebas. Normalmente, como desarrollador, tienes que localizar todos los archivos necesarios, realizar los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo. Y así sucesivamente.

El modo de agente de Copilot te ayuda con este proceso.

El modo de agente de Copilot está diseñado para trabajar con mayor autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite recibir comentarios rápidamente e interactuar con él. El agente en la nube trabaja de forma asíncrona como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- implementar funciones en la interfaz y la capa de datos con el modo de agente de GitHub Copilot;
- revisar los cambios y las pruebas que genera el modo de agente antes de combinarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya creaste una función auxiliar para obtener editoriales. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web Tailspin Toys

Antes de realizar cambios, explora el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con un elemento `<script>` de Astro de ámbito reducido que utiliza las API DOM estándar.

### Iniciar el sitio web

La tarea `npm run dev` inicia un único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal para abrir el sitio.

> [!NOTE]
> En un codespace, al seleccionar el enlace `localhost` en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Una vez que se esté ejecutando, explora las funciones del sitio. Entre otras:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar la lista de trabajo pendiente con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada paso indicarán cuáles debes utilizar.

La versión inicial del sitio funciona, pero queremos añadir capacidades. Empecemos revisando la lista de trabajo pendiente. Al crear el proyecto a partir de la plantilla, se generó una lista de incidencias automáticamente. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y utilizar herramientas.

   ![Selector de agente de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Las personas que han creado este taller no están expresando preferencia por ningún modelo. Durante su desarrollo se utilizó Claude Sonnet 4.5 y, por eso, se incluye en estas instrucciones. El objetivo es que las sugerencias de código sean relativamente coherentes y la experiencia sea satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es normal que las sugerencias varíen.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que enumere todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contiene directrices para añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones específicos para varios tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la función de filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo de agente de Copilot puede realizar estas tareas. Añadamos la función.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Selector de agente de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la función. Debería encontrar las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Mientras trabaja, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás que aceptar o deshacer todos los cambios propuestos; no hace falta que lo hagas mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas e incluso fallos en algunas pruebas. No pasa nada: Copilot alternará entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva función.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has visto cómo:

- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos;
- el modo de agente de Copilot puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos en el código.

A continuación, [probarás la función con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo que has aprendido para pedir a Copilot que la implemente en el sitio. Al redactar la indicación:

- sigue las buenas prácticas existentes e incluye los archivos de instrucciones pertinentes;
- decide cómo quieres implementar la paginación: por ejemplo, si los usuarios podrán elegir el tamaño de página o si estará fijado;
- proporciona a Copilot las directrices necesarias para implementar la función según tus preferencias;
- itera con GitHub Copilot, solicitando cambios y proporcionando contexto. Es lo habitual cuando se trabaja con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: diferencias y usos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
