# Configuración del Entorno para pentest de Dispositivos Móviles

## Instalación de BlueStacks

1. **Descargar BlueStacks:**
   - Ve al [sitio web de BlueStacks](https://www.bluestacks.com/download.html) y descarga el instalador para tu sistema operativo (El ejecutable para windows ya esta precargado en este repo).

2. **Instalar BlueStacks:**
   - Ejecuta el instalador descargado y sigue las instrucciones en pantalla para instalar BlueStacks en tu computadora.

3. **Configurar BlueStacks:**
   - Después de la instalación, inicia BlueStacks.
   - Durante la primera ejecución, BlueStacks te guiará a través de un proceso de configuración inicial.
   - Sigue las instrucciones en pantalla para configurar las preferencias de idioma, iniciar sesión con tu cuenta de Google y otras opciones según tus preferencias.
  
4. **Habilitar ADB**
   - Abrir app player -> Ajustes -> Avanzado -> Puente de depuracion de Android
![image](https://github.com/user-attachments/assets/32190d5a-cc12-4f0c-9ddb-6094ce8ced82)

  
5. **Rootear Dispositivo**
   - Dirigirse al directorio de ***C:\ProgramData\BlueStacks_nxt*** y editar el archivo Bluestacks.conf y cambiar de 0 a 1 los siguientes parametros:
   -bst.instance.Pie64.enable_root_access="0"
   -bst.instance.Rvc64.enable_root_access="0"
   -bst.feature.rooting="0"

   - Despues de realizar esto cerrar el emulador y volver a iniciarlo

6. **Verificar Root**
   - Identificar funcion de instalar apk en Bluestacks e instalar el apk llamado ***Verificadorderoot.apk*** que se encuentra dentro de este repositorio, al abrir esta aplicacion nos aparecera si el proceso de root fue ejecutado con exito
