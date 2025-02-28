# 📖 Bypass de Root Detection en RootBeer con Frida

## 🔥 Objetivo

Aprender a evadir la detección de root en una aplicación que usa **RootBeer** mediante **Frida**. RootBeer implementa múltiples checks, por lo que trabajaremos en **bypassear cada uno de ellos** individualmente y luego combinarlos en un solo script.

---

## 🛠 Requisitos

- **APK de la aplicación** que usa RootBeer.
- **JADX-GUI** para descompilar el APK.
- **Frida** instalado en el sistema y en el dispositivo.
- **Un dispositivo rooteado o un emulador** con Frida server en ejecución.
---

## 1️⃣ Descompilar el APK con JADX-GUI

1. **Abrimos JADX-GUI**.
2. **Cargamos el APK de la aplicación**.
3. **Buscamos la clase `RootBeer`**, que contiene las verificaciones de root.
4. **Identificamos los métodos responsables** de la detección de root:

- `detectRootManagementApps()` → Detecta apps de gestión de root.
- `detectPotentiallyDangerousApps()` → Detecta apps potencialmente peligrosas.
- `checkForBinary("su")` → Busca el binario `su`.
- `checkForDangerousProps()` → Verifica propiedades peligrosas del sistema.
- `checkForRWPaths()` → Verifica permisos de escritura en rutas del sistema.
- `detectTestKeys()` → Comprueba si `Build.TAGS` contiene `test-keys`.
- `checkSuExists()` → Ejecuta `which su`.
- `checkForRootNative()` → Usa una librería nativa para detectar root.
- `checkForMagiskBinary()` → Busca el binario `magisk`.

---

## 2️⃣ Bypassear un método con Frida

1. **Creamos el archivo de script:**
   ```sh
   nano bypass_rootbeer_single.js
   ```

2. **Agregamos el siguiente código:**
   ```javascript
   Java.perform(function() {
       var RootBeer = Java.use("com.scottyab.rootbeer.RootBeer");
       
       RootBeer.detectRootManagementApps.implementation = function() {
           console.log("[*] Bypassing detectRootManagementApps");
           return false;
       };
   });
   ```

3. **Ejecutamos el script con Frida:**
   ```sh
   frida -U -f com.example.app -l bypass_rootbeer_single.js
   ```

---

## 🚀 Siguientes Pasos

- **Desarrolla un script que bypassee todos los métodos identificados en el análisis.**
- Experimenta con más verificaciones de root en otras aplicaciones.
- Explora cómo Frida puede modificar otros aspectos de una app.
