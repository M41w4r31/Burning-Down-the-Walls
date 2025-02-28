# 📖 Bypass de Root Detection en Android con Frida

## 🔥 Objetivo

Aprender a evadir la detección de root en una aplicación Android utilizando **Frida**. Para este ejercicio, partimos de un APK que verifica si el dispositivo está rooteado con tres métodos diferentes.

---

## 🛠 Requisitos

- **APK de la aplicación** (proporcionado en el workshop)
- **JADX-GUI** para descompilar el APK
- **Frida** instalado en el sistema y en el dispositivo
- **Un dispositivo rooteado o un emulador** con Frida server en ejecución

---

## 1️⃣ Descompilar el APK con JADX-GUI

1. **Abrimos JADX-GUI**.
2. **Cargamos el APK de la aplicación**.
3. **Buscamos la clase principal** que contiene las verificaciones de root.
4. **Identificamos los métodos responsables** de la detección de root.

En este caso, los métodos clave son:

- `checkRootMethod1()` → Revisa la existencia del archivo `su`
- `checkRootMethod2()` → Ejecuta `which su`
- `checkRootMethod3()` → Verifica `Build.TAGS`

---

## 2️⃣ Bypassear cada método con Frida

Ahora, procederemos con el bypass de cada uno de los métodos de manera individual, explicando paso a paso.

### **Bypass de checkRootMethod1() - Archivos `su`**

1. **Identificamos el método `checkRootMethod1()` en JADX-GUI**.
2. **Abrimos un editor de texto y creamos el archivo del script**:
   ```sh
   bypass_root_method1.js
   ```
3. **Escribimos el siguiente código en el script:**
   ```javascript
   Java.perform(function() {
       var MainActivity = Java.use("com.example.rootchecker.MainActivity");
       MainActivity.checkRootMethod1.implementation = function() {
           console.log("[*] Bypassing checkRootMethod1: Archivos su");
           return false;
       };
   });
   ```
4. **Guardamos y ejecutamos el script con Frida:**
   ```sh
   frida -U -f com.example.rootchecker -l bypass_root_method1.js
   ```
5. **Verificamos que la app ya no detecta root por este método.**

---

### **Bypass de checkRootMethod2() - Comando `which su`**

1. **Identificamos el método `checkRootMethod2()` en JADX-GUI**.
2. **Creamos el archivo de script:**
   ```sh
   bypass_root_method2.js
   ```
3. **Agregamos el siguiente código:**
   ```javascript
   Java.perform(function() {
       var MainActivity = Java.use("com.example.rootchecker.MainActivity");
       MainActivity.checkRootMethod2.implementation = function() {
           console.log("[*] Bypassing checkRootMethod2: which su");
           return false;
       };
   });
   ```
4. **Guardamos y ejecutamos con Frida:**
   ```sh
   frida -U -f com.example.rootchecker -l bypass_root_method2.js
   ```
5. **Verificamos que la app ya no detecta root por este método.**

---

### **Bypass de checkRootMethod3() - `Build.TAGS`**

1. **Identificamos el método `checkRootMethod3()` en JADX-GUI**.
2. **Creamos el archivo de script:**
   ```sh
   nano bypass_root_method3.js
   ```
3. **Agregamos el siguiente código:**
   ```javascript
   Java.perform(function() {
       var MainActivity = Java.use("com.example.rootchecker.MainActivity");
       MainActivity.checkRootMethod3.implementation = function() {
           console.log("[*] Bypassing checkRootMethod3: Build.TAGS");
           return false;
       };
   });
   ```
4. **Guardamos y ejecutamos con Frida:**
   ```sh
   frida -U -f com.example.rootchecker -l bypass_root_method3.js
   ```
5. **Verificamos que la app ya no detecta root por este método.**

---

## 3️⃣ Ejecutar todos los bypass en un solo script

Para mayor comodidad, podemos combinar todos los bypass en un solo archivo:

1. **Creamos un nuevo script:**
   ```sh
   nano bypass_root.js
   ```
2. **Agregamos todos los métodos:**
   ```javascript
   Java.perform(function() {
       var MainActivity = Java.use("com.example.rootchecker.MainActivity");
       
       MainActivity.checkRootMethod1.implementation = function() {
           console.log("[*] Bypassing checkRootMethod1: Archivos su");
           return false;
       };
       
       MainActivity.checkRootMethod2.implementation = function() {
           console.log("[*] Bypassing checkRootMethod2: which su");
           return false;
       };
       
       MainActivity.checkRootMethod3.implementation = function() {
           console.log("[*] Bypassing checkRootMethod3: Build.TAGS");
           return false;
       };
   });
   ```
3. **Ejecutamos el script con Frida:**
   ```sh
   frida -U -f com.example.rootchecker -l bypass_root.js
   ```
4. **Verificamos que la app no detecta root en ninguna verificación.**

---

## 🎯 Conclusión

Hemos aprendido a **bypassear cada método individualmente** y luego a **combinarlos en un solo script** para mayor eficiencia. Esto nos permite engañar a la aplicación sin modificar el APK ni recompilarlo. 🚀

---

## 🚀 Siguientes Pasos

- Experimenta con más verificaciones de root en otras aplicaciones.

