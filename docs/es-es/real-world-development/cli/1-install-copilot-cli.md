---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado inicial."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas y solicitar cambios sin perder la concentración. El primer paso es instalar la herramienta; por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm;
- iniciarás sesión con tu cuenta de GitHub;
- confiarás en el repositorio del taller y probarás una conversación;
- localizarás la incidencia de filtrado con el servidor GitHub MCP integrado.

## Escenario

Tu equipo está empezando a utilizar agentes de IA para abordar una lista de trabajo pendiente cada vez mayor. Copilot CLI incorpora esa capacidad al terminal, donde ya trabajan muchos desarrolladores. En esta lección instalarás y autenticarás la herramienta, y la dejarás preparada para el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces ya incluye Node.js, utilizaremos npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Debe aparecer la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Iniciar sesión en GitHub

La primera vez que se inicia, Copilot CLI te pedirá que inicies sesión con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para iniciar sesión y autorizar Copilot CLI.
3. Copilot CLI mostrará esta solicitud:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Es posible que ya tengas una sesión iniciada en Codespaces mediante tu cuenta de GitHub. Si Copilot CLI se inicia sin pedirte que inicies sesión, ya está listo.

## Familiarizarse con la herramienta

Los comandos que escribes en el terminal normal se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor GitHub MCP integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI utiliza la rama seleccionada en el terminal; no crea automáticamente un worktree. Crearás una rama de funcionalidad antes de cada cambio.

## Utilizar el atajo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas que no forman parte de los permisos establecidos. Para este taller, lo reiniciarás con `--yolo`, un atajo aprobado por ti que elimina estas solicitudes de permiso dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y nunca lo configures como alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que se utilizan en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso al equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la función.

## Resumen y pasos siguientes

¡Enhorabuena! Has instalado y autenticado GitHub Copilot CLI. Has aprendido a:

- instalar Copilot CLI con npm;
- iniciar sesión con tu cuenta de GitHub;
- confiar en el repositorio del taller y probar una conversación;
- localizar la incidencia de filtrado con el servidor GitHub MCP integrado.

A continuación, [empezarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar una valoración por estrellas en las tarjetas de los juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
