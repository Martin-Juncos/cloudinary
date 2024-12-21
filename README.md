### **Cloudinary**

Es un servicio en la nube que permite gestionar, almacenar y optimizar archivos multimedia como imágenes y videos. En este ejemplo, usamos **Cloudinary** para subir imágenes desde una aplicación React desarrollada con Vite. A continuación, se explica cómo funciona Cloudinary en este caso:

---

### **Flujo de Funcionamiento con el Ejemplo**

1. **Configuración Inicial**:

   - Se configuran las credenciales de Cloudinary (nombre de la cuenta y un preset de subida) como variables de entorno en el archivo `.env`:

     ```env
     VITE_PRESET_NAME=tu_preset_name
     VITE_CLOUD_NAME=tu_cloud_name
     ```

   - Estas variables se importan en el componente React usando `import.meta.env`:

     ```javascript
     const preset_name = import.meta.env.VITE_PRESET_NAME;
     const cloud_name = import.meta.env.VITE_CLOUD_NAME;
     ```

   - El `preset_name` define las configuraciones específicas para la subida, como la carpeta de destino o restricciones en los archivos aceptados.

---

2. **Seleccionar una Imagen**:
   - El usuario selecciona un archivo mediante un input de tipo `file`:
     ```javascript
     <input
       type="file"
       name="file"
       onChange={(e) => uploadImage(e)} // Evento que ejecuta la función de subida
     />
     ```

---

3. **Preparar el Archivo para Subirlo**:
   - Cuando el usuario selecciona una imagen, el evento `onChange` ejecuta la función `uploadImage`.
   - Dentro de esta función, se crea un objeto `FormData` para empaquetar el archivo seleccionado y el preset necesario para Cloudinary:
     ```javascript
     const files = e.target.files; // Recupera el archivo del input
     const data = new FormData();
     data.append("file", files[0]); // Agrega el archivo al objeto FormData
     data.append("upload_preset", preset_name); // Incluye el preset configurado en Cloudinary
     ```

---

4. **Subida del Archivo a Cloudinary**:

   - Se realiza una solicitud HTTP de tipo `POST` a la API de Cloudinary para subir el archivo:

     ```javascript
     const response = await fetch(
       `https://api.cloudinary.com/v1_1/${cloud_name}/image/upload`,
       {
         method: "POST",
         body: data, // Envía el objeto FormData en el cuerpo de la solicitud
       }
     );
     ```

   - La URL de la API contiene el nombre de la cuenta (`cloud_name`) para dirigir la solicitud al espacio adecuado en Cloudinary.

---

5. **Recibir la Respuesta**:

   - Cuando la imagen se sube correctamente, Cloudinary devuelve una respuesta en formato JSON con información detallada, incluyendo la URL pública (`secure_url`) del archivo subido:

     ```javascript
     const file = await response.json();
     setImage(file.secure_url); // Guarda la URL en el estado 'image'
     ```

   - En caso de error, se captura en un bloque `catch` y se registra en la consola:
     ```javascript
     catch (error) {
       console.error("Error uploading image:", error);
     }
     ```

---

6. **Mostrar el Resultado**:
   - Si la imagen se sube con éxito, su URL se usa para mostrarla en la interfaz del usuario:
     ```javascript
     {
       loading ? <h3>Loading...</h3> : <img src={image} alt="imagen subida" />;
     }
     ```

---

### **Ventajas de Usar Cloudinary**

1. **Gestión Automática**:

   - Cloudinary genera automáticamente URLs seguras y optimizadas para las imágenes.

2. **Fácil Integración**:

   - Su API REST es fácil de usar y permite integrar la funcionalidad de subida en cualquier aplicación web.

3. **Transformaciones en la Nube**:

   - Las imágenes subidas pueden ser manipuladas directamente en Cloudinary (por ejemplo, cambiar el tamaño, recortar, agregar filtros, etc.).

4. **Almacenamiento Seguro**:
   - Las imágenes se almacenan en la nube, evitando el uso de almacenamiento local en el servidor.

---

### **Conclusión**

En este ejemplo, Cloudinary actúa como un backend para gestionar las imágenes subidas desde la aplicación React. Esto reduce la complejidad de manejar archivos en el servidor y optimiza la entrega de contenido multimedia. Este enfoque es eficiente, seguro y escalable, ideal para aplicaciones modernas.

## Contribuciones

Si deseas contribuir, realiza un fork del repositorio, realiza tus cambios y envía un pull request.

---

## Licencia

Este proyecto está bajo la Licencia MIT. Puedes ver los detalles en el archivo `LICENSE` incluido en este repositorio.

---

## Recursos

- [Documentación de Cloudinary](https://cloudinary.com/documentation)
- [Documentación de Vite](https://vitejs.dev/)
- [Documentación de React](https://reactjs.org/)

Made by Prof. Martin with a lot of 💖 and ☕
