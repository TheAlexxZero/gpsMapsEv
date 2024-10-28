# Proyecto GPSMapsEv

## Descripción
Este proyecto es una aplicación Android que utiliza Google Maps y GPS, implementando medidas de seguridad para proteger contra vulnerabilidades comunes. La aplicación está diseñada para ofrecer funcionalidades de mapeo y localización, asegurando al mismo tiempo la protección de los datos sensibles del usuario.

## Análisis de Vulnerabilidades
Se realizó un análisis de seguridad en la aplicación utilizando MobSF (Mobile Security Framework), que incluyó un análisis estático y dinámico.

### Análisis Estático
#### Permisos de la Aplicación
El análisis estático reveló los siguientes permisos utilizados por la aplicación:

| PERMISO                                       | ESTADO     | DESCRIPCIÓN                                                                                                              |
|-----------------------------------------------|------------|--------------------------------------------------------------------------------------------------------------------------|
| `android.permission.ACCESS_COARSE_LOCATION`    | peligroso  | Permite acceder a fuentes de ubicación aproximada, como la base de datos de redes móviles.                               |
| `android.permission.ACCESS_FINE_LOCATION`      | peligroso  | Permite acceder a fuentes de ubicación precisas, como el GPS.                                                          |
| `android.permission.ACCESS_NETWORK_STATE`      | normal     | Permite que la aplicación vea el estado de todas las redes.                                                             |
| `android.permission.INTERNET`                  | normal     | Permite que la aplicación cree sockets de red.                                                                          |
| `com.example.gpsmapsapp.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION` | desconocido | Permiso desconocido de la referencia de Android.                                                                        |

#### Análisis del Certificado
- **Severidad Alta**: La aplicación está firmada con un certificado de depuración. No se debe enviar una aplicación de producción con un certificado de depuración.
- **Información**: La aplicación está firmada con un certificado de firma de código.

#### Análisis del Manifiesto
1. **Debug Enabled For App**: Se habilitó la depuración, lo que facilita a los ingenieros inversos conectar un depurador a la aplicación.
2. **Application Data can be Backed up**: La aplicación permite que cualquiera respalde sus datos a través de ADB, lo que podría exponer datos sensibles.
3. **Broadcast Receiver Protected**: Se encontró un Broadcast Receiver que es accesible para otras aplicaciones en el dispositivo. El nivel de protección del permiso debe ser revisado.

### Recomendaciones
Para mejorar la seguridad de la aplicación, se recomienda:

- Desactivar la depuración para la versión de producción.
- Revisar y limitar el acceso a los datos sensibles.
- Proteger los Broadcast Receivers de forma adecuada.

## Implementación de Mejores Prácticas
1. **Asegurar el Código y la Infraestructura**: Se han seguido las mejores prácticas de codificación para garantizar la seguridad del código.
2. **Cifrado para Datos Sensibles**: Se implementa cifrado para proteger la información sensible del usuario.
3. **Comunicación Segura**: La comunicación de red se asegura utilizando HTTPS.
4. **Validación y Sanitización**: Todas las entradas del usuario son validadas y sanitizadas para evitar inyecciones de código.

## Documentación Adicional
- [Análisis de Vulnerabilidades](analisis_de_vulnerabilidades.md)
- [Mejores Prácticas](best_practices.md)
- [Implementación](implementacion.md)

## Cómo Ejecutar la Aplicación de Forma Segura
1. Clona el repositorio.
2. Importa el proyecto en Android Studio.
3. Ejecuta la aplicación en un dispositivo o emulador.
4. Asegúrate de que los permisos necesarios están configurados.

## Reporte de Vulnerabilidades
El reporte detallado de las pruebas de vulnerabilidad realizadas se encuentra en el archivo `vulnerability_report.pdf`.