### **Introducción**

Si has escuchado sobre Oracle APEX y quieres entender cómo funciona por dentro, este artículo es para ti. Explicaré en términos sencillos cómo está estructurado y por qué es una herramienta tan poderosa para el desarrollo de aplicaciones web empresariales.

### **¿Qué es Oracle APEX?**
Oracle APEX (Application Express) es un entorno de desarrollo rápido de aplicaciones (RAD) basado en la web que permite crear aplicaciones seguras y escalables utilizando solo un navegador. Se ejecuta completamente dentro de una base de datos Oracle y aprovecha su potencia para manejar datos de manera eficiente.

### **Componentes principales de la arquitectura de Oracle APEX**
Oracle APEX está compuesto por varios elementos clave que trabajan juntos para ofrecer un entorno de desarrollo sin necesidad de instalar software adicional en los clientes. Estos son:

> #### Base de Datos Oracle
> Oracle APEX reside dentro de una base de datos Oracle. Todas las definiciones de las aplicaciones, datos, usuarios y configuraciones se almacenan en esquemas dentro de la base de datos.

> #### Motor APEX
> El motor APEX es el corazón del sistema y está compuesto por un conjunto de paquetes PL/SQL que interpretan y ejecutan las solicitudes de los usuarios. Cuando alguien usa una aplicación APEX, las interacciones generan llamadas a estos paquetes, que procesan la lógica de negocio y devuelven contenido dinámico a la interfaz de usuario.

> #### Oracle REST Data Services (ORDS) o el Embedded PL/SQL Gateway
> APEX necesita un puente para conectar el navegador con la base de datos. Para esto, se pueden utilizar:

> **ORDS (Oracle REST Data Services)**: Un servicio intermedio basado en Java que recibe solicitudes HTTP y las traduce a llamadas PL/SQL dentro de la base de datos.

> **Embedded PL/SQL Gateway (EPG)**: Un método más simple pero menos recomendado para entornos de producción, que permite la comunicación sin un middleware adicional.

> #### El Navegador Web
> El usuario final accede a las aplicaciones de APEX mediante un navegador web. La interfaz de usuario está basada en HTML, CSS, JavaScript y AJAX, permitiendo una experiencia fluida y dinámica.

### **Flujo de una solicitud en APEX**

Para entender cómo funciona APEX en acción, veamos el flujo básico de una solicitud:

1. Un usuario accede a una aplicación APEX desde su navegador.

2. La solicitud llega al servidor ORDS (o EPG), que la traduce en llamadas PL/SQL.

3. El motor APEX en la base de datos ejecuta la lógica y genera una respuesta en HTML.

4. ORDS devuelve la respuesta al navegador, mostrando la página con la información solicitada.

### **Conclusión**

>  **Oracle APEX** es una plataforma robusta que simplifica el desarrollo de aplicaciones web sin necesidad de conocimientos avanzados en programación front-end. Su arquitectura basada en la base de datos Oracle garantiza seguridad, escalabilidad y facilidad de mantenimiento.

En el próximo artículo, exploraremos más a fondo cómo desde cero, instalar, configurar un entorno de desarrollo con Oracle APEX y ORDS, On Premise y luego en la Nube.

Siguemé, suscribeté y activa las notificaciones, que van a ser muchos post, y no te olvides de compartir.

