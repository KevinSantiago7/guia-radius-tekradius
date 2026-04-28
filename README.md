# Guía paso a paso: Configuración de un servidor RADIUS en Windows 11 usando TekRADIUS LT

## 1. Descripción

Esta guía explica cómo instalar, configurar y probar un servidor RADIUS en Windows 11 usando **TekRADIUS LT**.


---

## 2. Objetivo de la práctica

Configurar un servidor RADIUS en Windows 11, crear un usuario de prueba y validar la autenticación usando una herramienta cliente llamada **NTRadPing** y opcionalmente validar también la autenticación usando una herramienta para celular llamada **Simple Radius Test Tool**

---

## 3. ¿Qué es un servidor RADIUS?

RADIUS significa **Remote Authentication Dial-In User Service**.

Un servidor RADIUS permite validar usuarios que intentan acceder a una red o servicio. El servidor recibe una solicitud con usuario y contraseña, revisa si los datos son correctos y responde con uno de estos resultados:

```text
Access-Accept
```

Significa que el acceso fue permitido.

```text
Access-Reject
```

Significa que el acceso fue rechazado.

---

## 4. Herramientas necesarias

| Herramienta | Uso |
|---|---|
| Windows 11 | Sistema operativo usado en la práctica |
| TekRADIUS LT | Servidor RADIUS |
| NTRadPing | Cliente de prueba RADIUS |
| PowerShell | Ejecutar comandos de verificación |
| Celular Android | Prueba opcional desde otro dispositivo |
| GitHub | Publicación de la guía |

---

## 5. Links de descarga

Los archivos necesarios estarán disponibles desde Google Drive.

| Programa | Enlace |
|---|---|
| TekRADIUS LT | [Descargar desde Drive](PEGAR_AQUI_LINK_DE_TEKRADIUS) |
| NTRadPing | [Descargar desde Drive](PEGAR_AQUI_LINK_DE_NTRADPING) |
| App RADIUS para Android | [Descargar desde Drive](PEGAR_AQUI_LINK_DE_APP_ANDROID) |

> Reemplazar `PEGAR_AQUI_LINK...` por los enlaces reales de Google Drive.

---

## 6. Datos que se usarán en la práctica

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

---

# Paso 1: Crear la carpeta del proyecto

Abrir **PowerShell** y ejecutar:

```powershell
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS"
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS\capturas"
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS\herramientas"
mkdir "$env:USERPROFILE\Desktop\Proyecto-RADIUS\evidencias"
```

Verificar que las carpetas fueron creadas:

```powershell
dir "$env:USERPROFILE\Desktop\Proyecto-RADIUS"
```

Abrir la carpeta:

```powershell
explorer "$env:USERPROFILE\Desktop\Proyecto-RADIUS"
```

**Evidencia recomendada:** tomar captura de la carpeta creada.

---

# Paso 2: Descargar TekRADIUS LT

1. Abrir el enlace de descarga de TekRADIUS LT desde Drive.
2. Descargar el archivo comprimido.
3. Guardarlo en la carpeta:

```text
Proyecto-RADIUS\herramientas
```

4. Extraer el archivo `.zip`.
5. Buscar el archivo:

```text
Setup.exe
```

**Evidencia recomendada:** tomar captura del archivo descargado y extraído.

---

# Paso 3: Instalar TekRADIUS LT

1. Clic derecho sobre `Setup.exe`.
2. Seleccionar **Ejecutar como administrador**.
3. Clic en **Next**.
4. Aceptar las opciones por defecto.
5. Finalizar la instalación.

**Evidencia recomendada:** tomar captura del instalador.

---

# Paso 4: Verificar que TekRADIUS fue instalado

Abrir **PowerShell como administrador** y ejecutar:

```powershell
Get-Service *Tek*
```

Resultado esperado:

```text
Status   Name          DisplayName
------   ----          -----------
Stopped  TekRADIUSLT   TekRADIUSLT
```

Si aparece `TekRADIUSLT`, significa que el servicio fue instalado correctamente.

**Evidencia recomendada:** tomar captura del resultado.

---

# Paso 5: Abrir TekRADIUS Manager

1. Abrir el menú de inicio.
2. Buscar:

```text
TekRADIUS LT Manager
```

3. Ejecutarlo como administrador.

---

# Paso 6: Crear el cliente RADIUS local

Dentro de **TekRADIUS Manager**, ir a la pestaña:

```text
Clients
```

Completar los campos así:

| Campo | Valor |
|---|---|
| NAS | `127.0.0.1` |
| Secret | `ClaseRadius123` |
| Username Part | Dejar vacío |
| Label | `Local` |
| Description | `Cliente local de prueba` |
| Vendor | `ietf` |
| Enabled | `Yes` |
| CoA Enabled | `No` |

Luego hacer clic en el botón verde **+** para agregar el cliente.

**Evidencia recomendada:** tomar captura del cliente creado.

---

# Paso 7: Crear el usuario de prueba

Ir a la pestaña:

```text
Users
```

En el campo de usuario escribir:

```text
estudiante1
```

Dejar el grupo como:

```text
Default
```

Hacer clic en el botón verde **+** para crear el usuario.

Luego seleccionar el usuario `estudiante1`.

En la parte derecha, agregar el atributo de contraseña:

| Campo | Valor |
|---|---|
| Type | `Check` |
| Attribute | `User-Password` |
| Value | `Clase2026` |

Hacer clic en el botón verde **+** para guardar el atributo.

**Evidencia recomendada:** tomar captura del usuario creado.

---

# Paso 8: Iniciar el servicio TekRADIUS

Abrir **PowerShell como administrador** y ejecutar:

```powershell
Start-Service TekRADIUSLT
```

Luego verificar el estado:

```powershell
Get-Service TekRADIUSLT
```

Resultado esperado:

```text
Status   Name          DisplayName
------   ----          -----------
Running  TekRADIUSLT   TekRADIUSLT
```

**Evidencia recomendada:** tomar captura del servicio en estado `Running`.

---

# Paso 9: Verificar el puerto RADIUS

Ejecutar en PowerShell:

```powershell
netstat -ano -p udp | findstr ":1812"
```

El puerto `1812/UDP` se usa para autenticación RADIUS.

También se puede verificar el puerto de accounting:

```powershell
netstat -ano -p udp | findstr ":1813"
```

**Evidencia recomendada:** tomar captura del puerto activo.

---

# Paso 10: Descargar y preparar NTRadPing

1. Abrir el enlace de descarga de NTRadPing desde Drive.
2. Descargar el archivo comprimido.
3. Guardarlo en:

```text
Proyecto-RADIUS\herramientas
```

4. Extraer el archivo.
5. Verificar que existan estos archivos:

```text
NTRADPING.EXE
RADDICT.DAT
```

Ambos archivos deben quedar en la misma carpeta.

**Evidencia recomendada:** tomar captura de los archivos extraídos.

---

# Paso 11: Realizar la prueba correcta con NTRadPing

Ejecutar `NTRADPING.EXE` como administrador.

Configurar los campos así:

| Campo | Valor |
|---|---|
| RADIUS Server | `127.0.0.1` |
| Port | `1812` |
| RADIUS Secret key | `ClaseRadius123` |
| User-Name | `estudiante1` |
| Password | `Clase2026` |
| CHAP | Desmarcado |
| Request type | `Authentication Request` |

Luego hacer clic en:

```text
Send
```

Resultado esperado:

```text
Access-Accept
```

Esto significa que el servidor RADIUS aceptó el usuario y la contraseña.

**Evidencia recomendada:** tomar captura del resultado `Access-Accept`.

---

# Paso 12: Realizar la prueba incorrecta

En NTRadPing, cambiar solamente la contraseña:

```text
mala123
```

Dejar todos los demás campos iguales.

Hacer clic nuevamente en:

```text
Send
```

Resultado esperado:

```text
Access-Reject
```

Esto demuestra que el servidor RADIUS no acepta una contraseña incorrecta.

**Evidencia recomendada:** tomar captura del resultado `Access-Reject`.

---

# Paso 13: Revisar logs o eventos

En TekRADIUS Manager, ir a la pestaña:

```text
Events
```

Si no aparecen eventos, revisar la configuración de logs en:

```text
Settings
```

También se pueden buscar logs desde PowerShell:

```powershell
Get-ChildItem "C:\Program Files\TekRADIUS LT" -Recurse -File |
Where-Object { $_.Extension -in ".log",".txt" } |
Sort-Object LastWriteTime -Descending |
Select-Object LastWriteTime, FullName -First 15
```

**Evidencia recomendada:** tomar captura de eventos, logs o configuración de logging.

---

# Paso 14: Prueba opcional desde celular Android

Esta prueba permite comprobar si el servidor RADIUS puede recibir solicitudes desde otro dispositivo conectado a la misma red Wi-Fi.

## 14.1 Obtener la IP de la PC

En la PC donde está TekRADIUS, ejecutar:

```powershell
ipconfig
```

Buscar la dirección IPv4 del adaptador Wi-Fi.

Ejemplo:

```text
192.168.1.50
```

Esta será la IP del servidor RADIUS.

---

## 14.2 Obtener la IP del celular

En Android, ir a:

```text
Ajustes > Wi-Fi > Red conectada > Detalles
```

Buscar la IP del celular.

Ejemplo:

```text
192.168.1.80
```

---

## 14.3 Agregar el celular como cliente RADIUS

En TekRADIUS Manager, ir a:

```text
Clients
```

Agregar un nuevo cliente con estos datos:

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

---

## 14.4 Abrir el firewall de Windows

En PowerShell como administrador, ejecutar:

```powershell
New-NetFirewallRule -DisplayName "Permitir RADIUS UDP 1812" -Direction Inbound -Protocol UDP -LocalPort 1812 -Action Allow
```

Opcionalmente, abrir también el puerto `1813`:

```powershell
New-NetFirewallRule -DisplayName "Permitir RADIUS UDP 1813" -Direction Inbound -Protocol UDP -LocalPort 1813 -Action Allow
```

---

## 14.5 Configurar la app RADIUS en el celular

En la aplicación cliente RADIUS del celular, usar estos datos:

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

Resultado esperado:

```text
Access-Accept
```

Luego probar con contraseña incorrecta:

```text
mala123
```

Resultado esperado:

```text
Access-Reject
```

---

## 14.6 Posibles errores en la prueba desde celular

| Problema | Posible causa |
|---|---|
| No responde | Firewall bloqueando el puerto `1812` |
| No responde | El celular no está en la misma red |
| No responde | La red Wi-Fi tiene aislamiento de clientes |
| Access-Reject | Usuario o contraseña incorrectos |
| Access-Reject | El celular no fue agregado como cliente RADIUS |
| Error de secret | El secret no coincide |

La prueba desde celular es opcional. La prueba principal de la guía es la realizada con NTRadPing.

---

# Paso 15: Evidencias finales

Al terminar, se recomienda tener estas capturas:

| Evidencia | Descripción |
|---|---|
| 01 | Carpeta del proyecto creada |
| 02 | Descarga de TekRADIUS LT |
| 03 | Instalación de TekRADIUS LT |
| 04 | Servicio TekRADIUSLT instalado |
| 05 | Cliente RADIUS creado |
| 06 | Usuario `estudiante1` creado |
| 07 | Servicio en estado `Running` |
| 08 | Puerto `1812/UDP` activo |
| 09 | Prueba `Access-Accept` |
| 10 | Prueba `Access-Reject` |
| 11 | Logs o configuración de TekRADIUS |
| 12 | Prueba opcional desde celular |

---

# Conclusión

Se configuró un servidor RADIUS en Windows 11 usando TekRADIUS LT.

La prueba con NTRadPing permitió validar el funcionamiento del servidor. Cuando se usó el usuario correcto, el servidor respondió `Access-Accept`. Cuando se usó una contraseña incorrecta, respondió `Access-Reject`.

Esto demuestra que el servidor RADIUS fue configurado correctamente y que puede validar credenciales de usuario.

---

# Autor

Guía elaborada por:

**Kevin Oliveros**
