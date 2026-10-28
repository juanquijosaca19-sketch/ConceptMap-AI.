# ConceptMap AI — versión de escritorio multiplataforma

Este proyecto utiliza **Tauri 2 + Vite + HTML/CSS/JavaScript**. La interfaz web se empaqueta como aplicación de escritorio para Windows, Linux y macOS.

## Importante
El ZIP contiene el código fuente y la configuración de empaquetado, no instaladores ya compilados. Los instaladores deben compilarse en cada sistema operativo (o mediante una configuración de compilación cruzada correctamente preparada). No se ha podido probar la compilación en los tres sistemas desde este entorno.

## Requisitos comunes
- Node.js LTS y npm
- Rust estable (instalado con rustup)
- Dependencias de compilación de Tauri 2 para el sistema operativo

## Windows
1. Instala Node.js LTS.
2. Instala Rust con rustup.
3. Instala Microsoft C++ Build Tools y WebView2 Runtime siguiendo la guía oficial de Tauri.
4. Abre PowerShell en la carpeta del proyecto y ejecuta:
   ```powershell
   npm install
   npm run tauri:dev
   ```
5. Para crear instaladores:
   ```powershell
   npm run tauri:build
   ```
   Busca los instaladores en `src-tauri/target/release/bundle/` (por ejemplo, `.msi` o `.exe`, según las herramientas disponibles).

## Linux (Ubuntu/Debian)
Instala primero las dependencias del sistema indicadas por la documentación de Tauri para tu distribución. En versiones Ubuntu/Debian habituales suelen incluir WebKitGTK, build-essential, curl, wget, file, librerías GTK y librerías de AppIndicator. Los nombres exactos cambian por versión.
```bash
# instala Node.js LTS y Rust según sus instrucciones oficiales
npm install
npm run tauri:dev
npm run tauri:build
```
Los paquetes generados estarán bajo `src-tauri/target/release/bundle/`, por ejemplo AppImage/deb, dependiendo de los targets y herramientas instaladas.

## macOS
1. Instala Node.js LTS y Rust.
2. Instala Xcode Command Line Tools:
   ```bash
   xcode-select --install
   ```
3. En Terminal, dentro de la carpeta del proyecto:
   ```bash
   npm install
   npm run tauri:dev
   npm run tauri:build
   ```
   Los artefactos se generan bajo `src-tauri/target/release/bundle/`, normalmente `.app` y/o `.dmg`, según la configuración.
   Para distribuir la aplicación fuera de tu Mac, puede ser necesario firmar y notarizarla con certificados de Apple.

## Abrir en modo desarrollo
```bash
npm install
npm run tauri:dev
```

## Qué hace esta versión
- Importa PDF con texto, DOCX, PPTX y TXT.
- Extrae frases y propone conceptos mediante heurísticas.
- Permite editar el texto de los nodos.
- Exporta PNG/SVG e imprime o guarda como PDF desde el diálogo del sistema.

## Limitaciones
- No es todavía IA generativa real: la selección de ideas es automática mediante reglas.
- PDF escaneados requieren OCR.
- Las librerías para leer PDF/DOCX/PPTX se cargan por CDN; por eso esta versión requiere Internet para esa parte. La interfaz y el shell de escritorio sí se empaquetan localmente.
- Antes de publicar, conviene fijar versiones de dependencias, revisar CSP, probar en los tres sistemas y añadir iconos/firmado.
- No pongas claves API de IA en el frontend. Una integración real debe usar un backend seguro o un servicio local protegido.

## Documentación oficial
- Tauri: https://v2.tauri.app/start/
- Tauri prerequisites: https://v2.tauri.app/start/prerequisites/
- Tauri bundling: https://v2.tauri.app/distribute/
- Node.js: https://nodejs.org/
- Rust: https://www.rust-lang.org/tools/install

## Generar los tres instaladores desde una computadora Windows (GitHub Actions)

No necesitas tener Linux ni macOS para solicitar las compilaciones: GitHub Actions ejecutará el proyecto en máquinas de Windows, Ubuntu y macOS. Se necesita una cuenta de GitHub y conexión a Internet.

1. Descomprime el ZIP en tu computadora.
2. Entra a [GitHub](https://github.com/) e inicia sesión (crea una cuenta si no tienes).
3. Crea un repositorio nuevo, por ejemplo `ConceptMap-AI`. Puedes dejarlo privado.
4. Sube al repositorio **todo el contenido** de esta carpeta, incluida la carpeta oculta `.github/workflows/build.yml`. Si usas la web de GitHub, puede ser más sencillo instalar GitHub Desktop o usar Git desde PowerShell.
5. En el repositorio, abre la pestaña **Actions** y selecciona el flujo **“Compilar ConceptMap AI para Windows, Linux y macOS”**.
6. Pulsa **Run workflow** y vuelve a pulsar el botón verde para iniciar la compilación. También se ejecutará automáticamente cuando hagas push a `main` o `master`.
7. Espera a que terminen los tres trabajos: Windows, Linux y macOS. Si alguno falla, abre ese trabajo y revisa el primer error rojo.
8. En la ejecución terminada, baja a **Artifacts** y descarga los archivos de ConceptMap AI generados por cada sistema. Descomprime el artefacto correspondiente para encontrar el instalador.

### Qué esperar
- **Windows:** instalador `.msi` o `.exe`, según los formatos producidos por la configuración de Tauri.
- **Linux:** paquete compatible con el formato que genere el runner, normalmente `.deb` y/o AppImage si está configurado y disponible.
- **macOS:** paquete `.dmg` y/o `.app` según los formatos producidos.

Esta automatización crea artefactos de compilación; no publica una versión pública automáticamente. La aplicación de macOS sin firma/notarización puede mostrar advertencias al instalarse, y la distribución pública de Windows también puede beneficiarse de firma de código. La firma/notarización requiere certificados y configuración adicionales.

Guía oficial de Tauri para GitHub Actions: https://v2.tauri.app/distribute/pipelines/github/
