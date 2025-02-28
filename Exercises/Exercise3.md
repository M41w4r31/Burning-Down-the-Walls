# 📖 Bypass de Jailbreak Detection en iOS con Frida

## 🔥 Objetivo

Aprender a evadir la detección de Jailbreak en una aplicación iOS utilizando **Frida**. Para este ejercicio, partimos de un  que verifica si el dispositivo está rooteado con tres métodos diferentes.

---

## 🛠 Requisitos

- **IPA de la aplicación** (proporcionado en el workshop)
- **Frida** instalado en el sistema y en el dispositivo
- **Un dispositivo iOS** con Frida server en ejecución

---

## 1️⃣ Ejecutar el IPA con Objection

1. **Usamos Frida para listar los modulos o aplicaciones**.
2. **Ejecutamos objection**.
---

## 2️⃣ Bypassear cada prueba con Objection

Ahora, procederemos con el bypass de la prueba Jailbreak Test 2.

### **Bypass de Jailbreak Test 2- Modificar el valor de retorno `bool`**

1. Utilizando Objection vamos a buscar por clases con la palabra `jailbreak`
    ```sh
   ios hooking search classes jailbreak
   ```
   Vamos a encontrar dos resultados:
   ```sh
JailbreakDetection
DVIA_v2.JailbreakDetectionViewController

Found 2 classes   
```
2. Vamos a listar todos los métodos de la clase JailbreakDetection

    ```sh
    ios hooking list class_methods JailbreakDetection

    ```
    Se encontró solo un método

    ```sh
    + isJailbroken

    Found 1 methods
    ```
3. Es necesario saber cuando se está utilizando un método
    ```sh
    ios hooking watch method "+[JailbreakDetection isJailbroken]" --dump-args --dump-return
    ```
4. Ahora es necesario saber cual prueba genera la alerta: Jailbreak Test 2
    ```sh
    (agent) [937378] Called: +[JailbreakDetection isJailbroken] 0 arguments(Kind: class) (Super: NSObject)
    (agent) [937378] Return Value: 0x1
    ```
5. Ahora podemos retornar el valor de `0x0` que significa falso.
    ```sh
    ios hooking set return_value "+[JailbreakDetection isJailbroken]" false
    ```
6. Si ejecutamos la prueba Jailbreak Test 2 veremos como sobreescribimos el valor a falso.

    ```sh
    Called: +[JailbreakDetection isJailbroken] 0 arguments(Kind: class) (Super: NSObject)
    (agent) [937378] Return Value: 0x1
    (agent) [427208] +[JailbreakDetection isJailbroken] Return value was: 0x1, overriding to 0x0
    ```
7. Para bypassear todas las pruebas es posible realizarlo con el script incluido en objection.
    ```sh
    ios jailbreak disable
    ```
---

---

## 🎯 Conclusión

Hemos aprendido a **bypassear un método** Utilizando técnicas de hooking con Objection (FRIDA).
El script de incluido en la herramienta de objection suele ser completo.


---

## 🚀 Siguientes Pasos

- Experimenta con más verificaciones de jailbreak en otras aplicaciones.
- Explora cómo Frida puede modificar otros aspectos de una app.