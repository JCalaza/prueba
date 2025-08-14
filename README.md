# Posit Connect

Ejecutar una aplicación de Shiny sin hacerlo directamente desde R, sino a través de una URL, se logra al desplegar la aplicación en un servidor. Esto permite que cualquier persona con el enlace pueda interactuar con ella sin necesidad de tener R o RStudio instalado.

## Despliegue en Servidores Gratuitos

La forma más común y accesible para los desarrolladores es utilizar servicios de alojamiento de Shiny. Estos son algunos de los más populares:
- Shinyapps.io: Este servicio de RStudio (ahora Posit) es la opción más sencilla para principiantes. Solo necesitas una cuenta de Posit Cloud y puedes publicar tu app con unos pocos clics desde RStudio. El plan gratuito permite alojar varias aplicaciones con un número limitado de horas de uso al mes.
- Posit Connect: Es una solución empresarial que ofrece más control y funciones avanzadas, como autenticación y permisos. Es ideal para organizaciones que necesitan compartir aplicaciones internamente. Para desplegar una aplicación en Posit Connect, el proceso es similar al de shinyapps.io en lo que respecta al uso de RStudio, pero requiere de una configuración previa del servidor. A diferencia de shinyapps.io, que es un servicio alojado, Posit Connect es una solución empresarial que se instala en un servidor propio o en la nube.
## Publicar desde un repositorio de GitHub

Publicar una aplicación de Shiny directamente desde un repositorio de GitHub es una forma eficiente de mantener tu aplicación actualizada y al mismo tiempo aprovechar las ventajas del control de versiones. El proceso puede variar un poco dependiendo de la plataforma de destino, pero en ambos casos se integra la conexión con Git.
### Despliegue en Posit Connect
Posit Connect tiene una funcionalidad llamada "Git-Backed Content" que te permite desplegar contenido directamente desde un repositorio de Git, e incluso configurarlo para que se actualice automáticamente cada vez que haya un nuevo “commit”.
- Crear un manifest.json: En la raíz de tu proyecto, debes crear un archivo manifest.json. Este archivo es crucial, ya que le dice a Posit Connect qué paquetes de R necesita instalar y cómo debe ejecutar la aplicación. Puedes generarlo fácilmente desde R con el comando rsconnect::writeManifest().
- Vincular el repositorio en Posit Connect:
  1. Inicia sesión en la interfaz de usuario de tu servidor de Posit Connect.
  2. Haz clic en "Publish" y luego en "Import from Git".
  3. Introduce la URL de tu repositorio de GitHub. Posit Connect escaneará el repositorio en busca de archivos manifest.json
  4. Selecciona el manifest.json que corresponde a tu aplicación y elige la rama que deseas monitorear.
  
Una vez configurado, Posit Connect se encargará de clonar el repositorio, instalar las dependencias y desplegar la aplicación. Si configuras la opción de auto-actualización, cualquier cambio en la rama que especificaste activará un nuevo despliegue.
¿Qué es un manifest.json?
manifest.json es un archivo fundamental para el despliegue de contenido en Posit Connect, especialmente cuando se utiliza la funcionalidad de "Git-Backed Content". Su función es actuar como un "manual de instrucciones" para el servidor, indicándole exactamente cómo debe desplegar y ejecutar tu aplicación.
¿Qué contiene el manifest.json?
Este archivo de texto con formato JSON almacena información crítica sobre tu proyecto, incluyendo:
Dependencias: Una lista de todos los paquetes de R (y sus versiones específicas) que tu aplicación necesita para funcionar. Esto garantiza que el entorno en el servidor de Posit Connect sea idéntico al que usaste para desarrollar la aplicación.
Archivos: Un listado de todos los archivos y recursos que forman parte de tu aplicación, como scripts de R, archivos de datos (.csv), imágenes, etc.
Configuración del entorno: Información sobre la versión de R que se debe usar.
¿Cómo se crea?
El manifest.json no se crea manualmente. Se genera automáticamente utilizando el paquete rsconnect de R.
Asegúrate de que estás en la carpeta raíz de tu proyecto de Shiny.
Abre la consola de RStudio.
Ejecuta el siguiente comando:  rsconnect::writeManifest()
Al ejecutar este comando, rsconnect escaneará tu proyecto, detectará todos los archivos y dependencias, y creará un archivo llamado manifest.json en la misma carpeta.
Proceso de despliegue con Git-Backed Content
Una vez que tengas el manifest.json en tu proyecto:
Añade y sube el archivo a Git: Debes incluir el manifest.json en tu repositorio. Cada vez que actualices el código o añadas nuevas dependencias, debes volver a ejecutar rsconnect::writeManifest() y subir el manifest.json actualizado.
Vincula en Posit Connect: Cuando enlaces tu repositorio en Posit Connect, este buscará el manifest.json para entender cómo debe desplegar la aplicación. La presencia de este archivo le permite a Posit Connect replicar el entorno exacto de tu proyecto.
¿Qué es Posit Connect?
Posit Connect (anteriormente conocido como RStudio Connect) es una plataforma de publicación para equipos y organizaciones que desean compartir el trabajo de ciencia de datos de manera segura y controlada. A diferencia de shinyapps.io, que es un servicio de alojamiento para aplicaciones de Shiny, Posit Connect es un servidor que se instala en la infraestructura de la empresa.
Su objetivo principal es ser un repositorio centralizado donde los equipos de ciencia de datos puedan publicar todo su trabajo (no solo aplicaciones de Shiny) y los usuarios de la organización puedan acceder a él de manera fácil y segura a través de un navegador web.

Características clave
Múltiples tipos de contenido: Posit Connect es una solución versátil que soporta una amplia gama de productos de ciencia de datos creados en R y Python:
Aplicaciones de Shiny: El uso más popular, permitiendo compartir aplicaciones interactivas.
Documentos estáticos: R Markdown, Quarto, y documentos de Python. Se pueden publicar como informes estáticos o dinámicos.
APIs: Permite publicar APIs creadas con Plumber (R) y Flask/FastAPI (Python), que pueden ser consumidas por otras aplicaciones.
Notebooks: Jupyter Notebooks, que pueden ser ejecutados y visualizados por otros usuarios.
Scripts: Scripts de R o Python que se pueden ejecutar bajo demanda o programar para que se ejecuten automáticamente.
Seguridad y control de acceso: Esta es una de las mayores ventajas sobre otras plataformas. Posit Connect ofrece funcionalidades de seguridad a nivel empresarial:
Autenticación: Se integra con sistemas de autenticación existentes en la empresa como LDAP, SAML o Active Directory.
Permisos: Puedes controlar quién puede ver, editar o ejecutar cada pieza de contenido. Los permisos se pueden asignar a usuarios o grupos.
Registro de auditoría: Permite rastrear y auditar quién accede a qué contenido y cuándo.
Automatización y programación:
Puedes configurar informes de R Markdown o Quarto para que se generen automáticamente en un horario regular (por ejemplo, un informe de ventas que se actualice cada mañana).
Los scripts de R y Python se pueden programar para que se ejecuten en un momento específico, lo que es ideal para ETL (Extract, Transform, Load) o la actualización de datos.
Versión del contenido y Git-Backed:
Permite un control de versiones de tu contenido.
Con la función "Git-Backed Content", puedes vincular tu repositorio de GitHub o Bitbucket. Cada vez que hagas un commit a la rama principal, Posit Connect puede detectar el cambio y actualizar automáticamente la aplicación o el informe, manteniendo todo sincronizado.
¿Cómo funciona el flujo de trabajo?
Desarrollo: Un científico de datos o desarrollador crea un informe, una aplicación, una API, etc., en su IDE local (como RStudio).
Publicación: Desde RStudio, usa el paquete rsconnect o el botón "Publish" para enviar el contenido al servidor de Posit Connect. Posit Connect detecta las dependencias y crea un entorno aislado para que el contenido se ejecute correctamente.
Acceso y gestión: El contenido ahora tiene una URL única en el servidor. El autor o un administrador puede asignar permisos a los usuarios. Los usuarios solo necesitan un navegador para acceder al contenido, sin necesidad de instalar R o Python.
En resumen
Posit Connect es una solución robusta y escalable para organizaciones que necesitan una plataforma segura y centralizada para compartir y gestionar su trabajo de ciencia de datos, yendo mucho más allá del simple alojamiento de aplicaciones de Shiny.

