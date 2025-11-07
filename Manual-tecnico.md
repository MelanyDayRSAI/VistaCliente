## Manual tecnico

## Diagrama de arquitectura
frontTablero/
├── .vs/
│   ├── ProjectSettings.json
│   ├── VSWorkspaceState.json
│   └── slnx.sqlite
│
├── .vscode/
│   ├── extensions.json
│   ├── launch.json
│   └── tasks.json
│
├── public/
│   └── favicon.ico
│
├── src/
│   └── ... (código fuente del proyecto Angular)
│
├── .editorconfig
├── .gitignore
├── Dockerfile
├── README.md
├── angular.json
├── nginx-default.conf
├── package-lock.json
├── package.json
├── tsconfig.app.json
├── tsconfig.json
└── tsconfig.spec.json

## Detalle de microservicios y lenguajes
 # VistaCliente (frontTablero)
  ♥ Tipo: Aplicación web frontend
  ♥ Lenguaje: TypeScript
  ♥ Framework: Angular 17
  ♥ Función principal: Interfaz solo lectura para mostrar en tiempo real:
    - marcador del partido
    - periodo
    - posesión
    - bonus
    - faltas
    - reloj de juego
    - reloj de tiro (24s)
    - contador de 8 segundos
  ♥ Tecnologías de soporte:
    - HTML / CSS
    - Angular Router
    - Mecanismo de modo oscuro/claro
    - LocalStorage para persistencia de tema
♥ No es un microservicio.
♥ No expone APIs.
♥ Solo consume datos del backend (no incluidos en esta parte).

## Cómo levantar el sistema localmente 
 # Requisitos
  ♥ Node.js 18+
  ♥ npm
  ♥ Angular CLI (opcional para desarrollo local)
  ♥ Navegador moderno
 # Levantar localmente sin Docker
-Instalar dependencias
  npm install
-Modo desarrollo
  npm start  ng serve -o
-Compilar para producción
  npm run build
-Levantar con Docker
Dockerfile ya incluido en el proyecto
Para construir la imagen:
  docker build -t vista-cliente .
Para ejecutar: 
  docker run -p 80:80 vista-cliente
Puerto por defecto:
  Se sirve en http://localhost:80

## Especificación de endpoints por microservicio
Este módulo no implementa endpoints propios, ya que funciona únicamente como
cliente de visualización. Toda la información es consumida desde microservicios externos.

## Seguridad:
Este módulo no implementa autenticación mediante JWT u OAuth.  
Toda la seguridad del sistema se maneja en los microservicios del backend y en 
el servicio de autenticación (Keycloak / proxy). El cliente solo consume información
de lectura sin requerir credenciales.

## Bibliotecas/librerías utilizadas
 # Dependencias base (Angular 17)
| Librería                              | ¿Para qué sirve?                                  |
| ------------------------------------- | ------------------------------------------------- |
| **@angular/core**                     | Núcleo del framework Angular.                     |
| **@angular/common**                   | Funciones y utilidades comunes.                   |
| **@angular/platform-browser**         | Renderizado en navegador web.                     |
| **@angular/platform-browser-dynamic** | Bootstrap dinámico del app Angular.               |
| **@angular/router**                   | Sistema de rutas SPA (las rutas que mencionaste). |
| **@angular/animations**               | Animaciones (si el UI usa transiciones).          |

# Dependencias de Angular necesarias para formularios/UI
| Librería                              | ¿Para qué sirve?                                  |
| ------------------------------------- | ------------------------------------------------- |
| **@angular/forms**                   	|Control de formularios reactivos y template-driven.|

 # Librerías esenciales que SIEMPRE están en Angular 17
| Librería    | ¿Para qué sirve?                                                                                      |
| ----------- | ----------------------------------------------------------------------------------------------------- |
| **rxjs**    | Programación reactiva: timers, intervalos, observables, streams (perfecto para relojes del marcador). |
| **tslib**   | Helpers compilados de TypeScript.                                                                     |
| **zone.js** | Manejo del ciclo de detección de cambios en Angular.                                                  |

 # Dependencias de desarrollo (Angular CLI y TypeScript)
| Librería         | ¿Para qué sirve?                                         |
| ---------------- | -------------------------------------------------------- |
| **@angular/cli** | Herramienta para servir, compilar y generar componentes. |
| **typescript**   | Lenguaje base del proyecto.                              |
| **@types/node**  | Tipos de Node para ejecutar Angular CLI.                 |
 
  ♥ Angular core
  ♥ Angular material (si lo usan)
  ♥ RXJS
  ♥ Bootstrap / Tailwind
  ♥ librerías de UI
  ♥ librerías de auth

| Librería        | Versión | Para qué sirve              |
| --------------- | ------- | --------------------------- |
| @angular/core   |  16.x   | framework base del frontend |
| @angular/router |  16.x   | manejo de rutas SPA         |
| rxjs            |  7.x    | programación reactiva       |
| tslib           |  2.x    | helpers de TypeScript       |

## Posibles errores y soluciones
# Error: “Node version not supported”
-Causa: Se está usando una versión antigua de Node.js (Angular 17 requiere Node 18+).
-Solución:
   -Instala Node.js 18 o superior.
   -Verifica versión actual:
   node -v
# Error: “Cannot find module …” al ejecutar npm start
-Causa: Dependencias incompletas o dañadas.
-Solución:
   rm -r node_modules
   npm install
# Error: “NG0303: Can’t bind to ‘…’ since it isn’t a known property”
-Causa: Falta importar un módulo en Angular (por ejemplo CommonModule, FormsModule o un componente independiente).
-Solución:
   Verificar que el componente tenga los imports necesarios.
   Revisar el @Component({ imports: [...] }) si usa Angular standalone.
# Error: estilos no aplican o el modo oscuro no cambia
-Causa: 
localStorage no guarda el tema. 
Conflicto en Tailwind / CSS.
Preferencia de color del navegador priorizada
-Solución:
   Limpiar cache: Ctrl + F5
   Revisar si prefers-color-scheme está sobreescribiendo el tema
   Validar clave correcta en localStorage (ejemplo: "theme")
# Error: la aplicación inicia pero muestra pantalla en blanco
-Causas posibles:
Error en compilación
Rutas mal configuradas
base-href incorrecto al construir para producción
-Soluciones:
   Revisar consola del navegador (F12)
   Ver si hay errores de importación o rutas
   Construir con base correcta:   
      ng build --base-href=/
# Error: “Failed to fetch data”
-Causa:
Backend caído
URL incorrecta
Problema de conexión
-Soluciones:
   Verificar URL en environment.ts
   Revisar si el backend está levantado
   Confirmar puertos expuestos en Docker  
