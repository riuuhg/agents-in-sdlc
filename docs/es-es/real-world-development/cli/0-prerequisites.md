---
title: "Ejercicio 0: Requisitos previos"
description: "Crea tu propia copia de Tailspin Toys y prepara un GitHub Codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar los ejercicios de Copilot CLI, tienes que dejarlo todo preparado. Crearás tu propia copia del repositorio Tailspin Toys y pondrás en marcha un [codespace][codespaces], cuyo terminal integrado usarás para instalar y ejecutar Copilot CLI en el siguiente ejercicio.

En esta lección:

- crearás tu propia copia del proyecto Tailspin Toys a partir de la plantilla;
- crearás un Codespace y confirmarás que el proyecto está listo.

## Configurar el repositorio del laboratorio

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][tailspin-template]. El nuevo repositorio contiene todos los archivos que necesitas para el laboratorio.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando **Use this template** y, después, **Create a new repository**.
3. Si estás realizando el taller como parte de un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el nuevo repositorio en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`), ya que la consultarás más adelante en el taller.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se genera automáticamente una lista de trabajo pendiente de incidencias de GitHub. Trabajarás con esas incidencias durante todo el taller; no tienes que crear ninguna por tu cuenta.

Utiliza una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, el código de la aplicación, pruebas, la habilidad `quality-checks` y la lista de trabajo pendiente que utilizarás. Si usas una copia anterior, consulta con el facilitador que contenga los archivos necesarios.

## Crear un codespace

A continuación, utilizarás un Codespace para completar el taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un editor completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Ve a tu repositorio recién creado.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración del Codespace. La plantilla instalará las dependencias del proyecto, Chromium de Playwright y la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio de Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y pasos siguientes

Ya tienes el entorno preparado. En esta lección:

- has creado tu propia copia del proyecto Tailspin Toys a partir de la plantilla;
- has creado un Codespace y confirmado que el proyecto está listo.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en tu Codespace y lo autenticarás con tu cuenta de GitHub.

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
[tailspin-template]: https://github.com/github-samples/tailspin-toys
