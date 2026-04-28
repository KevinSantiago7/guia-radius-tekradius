# Configuración de un servidor RADIUS en Windows 11 usando TekRADIUS LT

## Descripción

Este repositorio contiene una guía paso a paso para instalar, configurar y probar un servidor RADIUS en Windows 11 usando TekRADIUS LT.

El proyecto fue realizado con el objetivo de explorar el proceso de configuración de un servidor RADIUS y documentarlo de forma clara para que otro estudiante pueda replicarlo durante una clase.

## Objetivo general

Configurar un servidor RADIUS en Windows 11 usando TekRADIUS LT, crear un cliente RADIUS local, registrar un usuario de prueba y validar la autenticación mediante una herramienta cliente.

## Objetivos específicos

- Instalar TekRADIUS LT en Windows 11.
- Configurar un cliente RADIUS local.
- Crear un usuario de prueba.
- Iniciar el servicio TekRADIUSLT.
- Probar autenticación con NTRadPing.
- Realizar una prueba positiva con `Access-Accept`.
- Realizar una prueba negativa con `Access-Reject`.
- Documentar el proceso con capturas propias.
- Publicar la guía en GitHub para que pueda ser replicada.
- Incluir una prueba opcional desde celular Android.

## ¿Qué es RADIUS?

RADIUS significa **Remote Authentication Dial-In User Service**.

Es un protocolo utilizado para autenticar, autorizar y registrar accesos de usuarios en redes. Un servidor RADIUS recibe una solicitud de autenticación, valida las credenciales y responde si el acceso debe ser permitido o rechazado.

Las respuestas más importantes son:

```text
Access-Accept
```

Indica que el usuario fue aceptado.

```text
Access-Reject
```

Indica que el usuario fue rechazado.

## ¿Para qué sirve un servidor RADIUS?

Un servidor RADIUS permite centralizar la autenticación de usuarios. En lugar de configurar usuarios directamente en cada equipo de red, los dispositivos consultan al servidor RADIUS para saber si un usuario puede acceder o no.

Se puede usar en escenarios como:

- Redes Wi-Fi empresariales o universitarias.
- VPN.
- Switches administrables.
- Firewalls.
- Hotspots.
- Control de acceso por usuario.
- Autenticación 802.1X.

## Escenario de práctica

En esta práctica se configuró TekRADIUS LT en Windows 11 y se probó usando NTRadPing desde el mismo equipo.

```text
NTRadPing
   ↓
127.0.0.1:1812
   ↓
TekRADIUS LT
   ↓
Usuario estudiante1
```

También se incluye una prueba opcional desde celular Android usando una aplicación cliente RADIUS.

```text
Celular Android
   ↓
Red Wi-Fi
   ↓
PC Windows 11 con TekRADIUS LT
   ↓
Usuario estudiante1
```

## Herramientas utilizadas

| Herramienta | Función |
|---|---|
| Windows 11 | Sistema operativo usado para la práctica |
| TekRADIUS LT | Servidor RADIUS para Windows |
| NTRadPing | Herramienta cliente para probar autenticación RADIUS |
| PowerShell | Verificación de servicio, carpetas, puertos y firewall |
| GitHub | Publicación de la guía |
| Celular Android | Prueba opcional desde otro dispositivo |

## Links de descarga

Los instaladores utilizados en esta práctica estarán disponibles desde Google Drive.

| Programa | Enlace |
|---|---|
| TekRADIUS LT | [Descargar desde Drive](PEGAR_AQUI_LINK_DE_TEKRADIUS) |
| NTRadPing | [Descargar desde Drive](PEGAR_AQUI_LINK_DE_NTRADPING) |
| App RADIUS para Android | [Descargar desde Drive](PEGAR_AQUI_LINK_DE_APP_ANDROID) |

> Nota: reemplazar los textos `PEGAR_AQUI_LINK...` por los enlaces reales de Google Drive.

## Datos usados en la práctica

| Elemento | Valor |
|---|---|
| Servidor RADIUS local | `127.0.0.1` |
| Puerto de autenticación | `1812` |
| Puerto de accounting | `1813` |
| Shared Secret | `ClaseRadius123` |
| Usuario de prueba | `estudiante1` |
| Contraseña correcta | `Clase2026` |
| Contraseña incorrecta | `mala123` |
| Método de autenticación | `PAP` |

## Estructura del repositorio

```text
guia-radius-tekradius
├── README.md
├── docs
│   └── guia-paso-a-paso.md
├── capturas
│   └── README.md
├── evidencias
│   └── README.md
├── scripts
│   ├── crear-estructura.ps1
│   ├── verificar-servicio.ps1
│   └── abrir-firewall-radius.ps1
└── assets
    └── img
```

## Resultado esperado

Al realizar la prueba con el usuario correcto:

```text
Usuario: estudiante1
Contraseña: Clase2026
```

El servidor debe responder:

```text
Access-Accept
```

Al realizar la prueba con contraseña incorrecta:

```text
Usuario: estudiante1
Contraseña: mala123
```

El servidor debe responder:

```text
Access-Reject
```

## Evidencias recomendadas

Durante la práctica se deben guardar capturas de:

1. Creación de la carpeta del proyecto.
2. Descarga de TekRADIUS LT.
3. Instalación de TekRADIUS LT.
4. Servicio TekRADIUSLT instalado.
5. Cliente RADIUS creado.
6. Usuario de prueba creado.
7. Servicio TekRADIUSLT en estado `Running`.
8. Puerto RADIUS `1812/UDP` activo.
9. Prueba `Access-Accept`.
10. Prueba `Access-Reject`.
11. Configuración de logs o eventos.
12. Prueba opcional desde celular.

## Comandos principales usados

### Crear carpeta de trabajo

```powershell
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS"
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS\capturas"
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS\herramientas"
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS\guia"
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS\evidencias"
```

### Verificar servicio TekRADIUS

```powershell
Get-Service *Tek*
```

### Iniciar servicio TekRADIUS

```powershell
Start-Service TekRADIUSLT
```

### Confirmar que el servicio está activo

```powershell
Get-Service TekRADIUSLT
```

### Verificar puerto RADIUS

```powershell
netstat -ano -p udp | findstr ":1812"
```

### Abrir puerto 1812 en firewall de Windows

```powershell
New-NetFirewallRule -DisplayName "Permitir RADIUS UDP 1812" -Direction Inbound -Protocol UDP -LocalPort 1812 -Action Allow
```

### Abrir puerto 1813 en firewall de Windows

```powershell
New-NetFirewallRule -DisplayName "Permitir RADIUS UDP 1813" -Direction Inbound -Protocol UDP -LocalPort 1813 -Action Allow
```

## Prueba principal con NTRadPing

La prueba principal se realiza con NTRadPing usando los siguientes datos:

| Campo | Valor |
|---|---|
| RADIUS Server | `127.0.0.1` |
| Port | `1812` |
| RADIUS Secret key | `ClaseRadius123` |
| User-Name | `estudiante1` |
| Password | `Clase2026` |
| CHAP | Desmarcado |
| Request type | `Authentication Request` |

Resultado esperado:

```text
Access-Accept
```

## Prueba negativa

Para comprobar que el servidor no acepta cualquier contraseña, se cambia únicamente la contraseña:

```text
mala123
```

Resultado esperado:

```text
Access-Reject
```

## Prueba opcional desde celular

Además de la prueba con NTRadPing en Windows, también se puede intentar una prueba desde un celular Android usando una aplicación cliente RADIUS.

Esta prueba permite comprobar que el servidor TekRADIUS puede recibir solicitudes desde otro dispositivo conectado a la misma red Wi-Fi.

### Requisitos para la prueba desde celular

- Celular Android conectado a la misma red Wi-Fi que la PC.
- Una aplicación cliente RADIUS para Android.
- TekRADIUS LT ejecutándose en Windows.
- Puerto UDP `1812` permitido en el firewall.
- IP del celular agregada como cliente RADIUS en TekRADIUS.

### Obtener la IP del servidor

En la PC donde está instalado TekRADIUS, abrir PowerShell y ejecutar:

```powershell
ipconfig
```

Buscar la dirección IPv4 del adaptador Wi-Fi.

Ejemplo:

```text
192.168.1.50
```

Esa será la IP del servidor RADIUS.

### Obtener la IP del celular

En Android, ir a:

```text
Ajustes > Wi-Fi > Red conectada > Detalles
```

Buscar la dirección IP del celular.

Ejemplo:

```text
192.168.1.80
```

### Agregar el celular como cliente RADIUS

En TekRADIUS Manager, ir a la pestaña **Clients** y agregar:

| Campo | Valor |
|---|---|
| NAS | IP del celular |
| Secret | `ClaseRadius123` |
| Vendor | `ietf` |
| Enabled | `Yes` |
| Description | `Celular Android prueba RADIUS` |

Ejemplo:

| Campo | Valor |
|---|---|
| NAS | `192.168.1.80` |
| Secret | `ClaseRadius123` |

### Configurar la app RADIUS en el celular

En la aplicación cliente RADIUS del celular, configurar:

| Campo | Valor |
|---|---|
| Server / Host | IP de la PC con TekRADIUS |
| Port | `1812` |
| Secret | `ClaseRadius123` |
| Username | `estudiante1` |
| Password | `Clase2026` |
| Authentication | `PAP` |

Ejemplo:

| Campo | Valor |
|---|---|
| Server | `192.168.1.50` |
| Port | `1812` |
| Secret | `ClaseRadius123` |
| Username | `estudiante1` |
| Password | `Clase2026` |

### Resultado esperado desde celular

Con la contraseña correcta, la app debería mostrar una respuesta similar a:

```text
Access-Accept
```

Luego se puede hacer una prueba con contraseña incorrecta:

```text
mala123
```

En ese caso, la respuesta esperada es:

```text
Access-Reject
```

### Nota sobre la prueba desde celular

La prueba desde celular puede fallar si:

- La red Wi-Fi tiene aislamiento de clientes.
- El firewall de Windows bloquea el puerto `1812`.
- La IP del celular no fue agregada como cliente RADIUS.
- El celular no está en la misma red que la PC.
- La app RADIUS no usa el mismo método de autenticación.

Por eso, la prueba principal de esta guía será NTRadPing en Windows, y la prueba con celular queda como prueba complementaria.

## Guía completa

La guía detallada se encuentra en:

[docs/guia-paso-a-paso.md](docs/guia-paso-a-paso.md)

## Conclusión

Se logró configurar un servidor RADIUS en Windows 11 usando TekRADIUS LT. La autenticación fue comprobada mediante NTRadPing usando una prueba correcta y una prueba incorrecta.

La respuesta `Access-Accept` confirmó que el usuario válido fue aceptado, mientras que la respuesta `Access-Reject` comprobó que el servidor rechaza credenciales incorrectas.

## Autor

Guía elaborada por:

**Kevin Oliveros**

## Estado del proyecto

Servidor RADIUS configurado y probado correctamente usando TekRADIUS LT en Windows 11.
