# 📖 Bypass de Root Detection en Android con Frida

## 🔥 Objetivo

Aprender a evadir la detección de root en una aplicación Android utilizando **Frida**. Para este ejercicio, usaremos la app desarrollada en el workshop **Burning Down the Walls**, la cual realiza tres chequeos de root:

* Presencia de apps de superusuario
* Existencia del binario `su`
* Presencia de binarios comunes relacionados con root

---

## 🛠 Requisitos

* APK de la app generada (`RootChecker`)
* **JADX-GUI** para revisar el código descompilado
* **Frida** instalado en el sistema y en el dispositivo
* Un **dispositivo rooteado o emulador** con Frida Server corriendo

---

## 1️⃣ Inspeccionar la app con JADX-GUI

1. Abrimos **JADX-GUI**
2. Cargamos el APK de `RootChecker`
3. Vamos a `com.m41w4r3.burningdownthewalls.MainActivity`
4. Identificamos que los métodos relevantes están embebidos en `runChecks()` y usan funciones como:

   * `hasSuperUserApp()`
   * `hasSuBinary()`
   * `hasKnownRootBinary()`

Estas serán las funciones que necesitaremos hookear con Frida.

---

## 2️⃣ Bypassear cada método con Frida

### 🔹 Bypass de `hasSuperUserApp()` (detección de apps root)

```javascript
Java.perform(function() {
    var MainActivity = Java.use("com.m41w4r3.burningdownthewalls.MainActivity");
    MainActivity.hasSuperUserApp.implementation = function() {
        console.log("[*] Bypassing hasSuperUserApp()");
        return false;
    };
});
```

### ✅ Ejecutar con Frida:

```bash
frida -U -f com.m41w4r3.burningdownthewalls -l bypass_superuserapp.js
```

---

### 🔹 Bypass de `hasSuBinary()` (detección de `su` en PATH)

```javascript
Java.perform(function() {
    var MainActivity = Java.use("com.m41w4r3.burningdownthewalls.MainActivity");
    MainActivity.hasSuBinary.implementation = function() {
        console.log("[*] Bypassing hasSuBinary()");
        return false;
    };
});
```

### ✅ Ejecutar con Frida:

```bash
frida -U -f com.m41w4r3.burningdownthewalls -l bypass_subinary.js
```

---

### 🔹 Bypass de `hasKnownRootBinary()` (binarios sospechosos en /system)

```javascript
Java.perform(function() {
    var MainActivity = Java.use("com.m41w4r3.burningdownthewalls.MainActivity");
    MainActivity.hasKnownRootBinary.implementation = function() {
        console.log("[*] Bypassing hasKnownRootBinary()");
        return false;
    };
});
```

### ✅ Ejecutar con Frida:

```bash
frida -U -f com.m41w4r3.burningdownthewalls -l bypass_rootbinaries.js
```

---

## 3️⃣ Ejecutar todos los bypass desde un solo script

```javascript
Java.perform(function() {
    var MainActivity = Java.use("com.m41w4r3.burningdownthewalls.MainActivity");

    MainActivity.hasSuperUserApp.implementation = function() {
        console.log("[*] Bypassing hasSuperUserApp()");
        return false;
    };

    MainActivity.hasSuBinary.implementation = function() {
        console.log("[*] Bypassing hasSuBinary()");
        return false;
    };

    MainActivity.hasKnownRootBinary.implementation = function() {
        console.log("[*] Bypassing hasKnownRootBinary()");
        return false;
    };
});
```

### ✅ Ejecutar con Frida:

```bash
frida -U -f com.m41w4r3.burningdownthewalls -l bypass_root.js
```

---

## 🎯 Conclusión

Ahora puedes evadir los tres mecanismos de detección de root implementados en la app sin necesidad de modificar el APK. Frida nos permite manipular el comportamiento en tiempo real, lo cual es ideal para pruebas dinámicas en pentesting móvil.

--
