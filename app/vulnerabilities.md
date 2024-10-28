# Análisis de Vulnerabilidades

Este documento detalla las vulnerabilidades identificadas en la aplicación Android de mapas y GPS, así como las recomendaciones para mitigarlas.

## Análisis de Permisos de la Aplicación

El análisis estático reveló varios permisos solicitados por la aplicación, los cuales se describen a continuación:

| Permiso                                 | Estado    | Información                             | Descripción                                                                                                                                                             |
|-----------------------------------------|-----------|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `android.permission.ACCESS_COARSE_LOCATION` | Peligroso | Ubicación aproximada (basada en red)  | Permite a la aplicación acceder a fuentes de ubicación aproximadas, como la base de datos de la red móvil, para determinar una ubicación aproximada del teléfono. Las aplicaciones maliciosas pueden usar esto para determinar aproximadamente dónde te encuentras. |
| `android.permission.ACCESS_FINE_LOCATION`   | Peligroso | Ubicación precisa (GPS)                | Permite a la aplicación acceder a fuentes de ubicación precisas, como el Sistema de Posicionamiento Global (GPS) en el teléfono. Las aplicaciones maliciosas pueden usar esto para determinar tu ubicación exacta y pueden consumir batería adicional. |
| `android.permission.ACCESS_NETWORK_STATE`  | Normal    | Ver estado de la red                   | Permite que una aplicación vea el estado de todas las redes.                                                                                                                                          |
| `android.permission.INTERNET`               | Normal    | Acceso completo a Internet             | Permite que una aplicación cree sockets de red.                                                                                                                                           |
| `com.example.gpsmapsapp.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION` | Desconocido | Permiso desconocido                     | Este permiso no está documentado en la referencia de Android, lo que plantea dudas sobre su propósito y seguridad.                                                                 |

### Evaluación de los Permisos

- **Permisos Peligrosos**: Los permisos `ACCESS_COARSE_LOCATION` y `ACCESS_FINE_LOCATION` son considerados peligrosos y deben ser solicitados con cuidado.

- **Permisos Normales**: Los permisos `ACCESS_NETWORK_STATE` e `INTERNET` son considerados normales y no plantean riesgos significativos.

- **Permiso Desconocido**: Se recomienda investigar más sobre el permiso desconocido.

### Recomendaciones

1. **Revisión de Permisos**: Realizar una revisión exhaustiva de los permisos solicitados.
2. **Justificación de Permisos Peligrosos**: Asegurarse de que haya una justificación clara para su uso.
3. **Documentación del Permiso Desconocido**: Investigar el permiso desconocido para comprender su propósito.

## Análisis de Certificados

Durante el análisis de seguridad, se identificaron las siguientes cuestiones relacionadas con los certificados utilizados en la aplicación:

| Título                                       | Severidad | Descripción                                                                                                           |
|----------------------------------------------|-----------|-----------------------------------------------------------------------------------------------------------------------|
| Aplicación firmada con certificado de depuración | Alta      | La aplicación está firmada con un certificado de depuración. Las aplicaciones en producción no deben ser distribuidas con un certificado de depuración. |
| Aplicación firmada                           | Información | La aplicación está firmada con un certificado de firma de código.                                                      |

### Evaluación de los Certificados

- **Aplicación firmada con certificado de depuración**: Este hallazgo es crítico, ya que puede permitir a un atacante modificar el código.

- **Aplicación firmada**: Este hallazgo indica que la aplicación está firmada correctamente.

### Recomendaciones

1. **Eliminar el uso de certificados de depuración**: Asegúrate de que la aplicación se firme con un certificado de producción antes de ser distribuida.
2. **Verificación de la firma de la aplicación**: Realiza verificaciones periódicas de la firma de la aplicación.

## Análisis del Manifiesto

Durante el análisis de seguridad, se identificaron las siguientes cuestiones relacionadas con el manifiesto de la aplicación:

| No | Problema                                                                                  | Severidad | Descripción                                                                                                                                                                      | Opciones                                                                                               |
|----|-------------------------------------------------------------------------------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| 1  | Depuración habilitada para la aplicación [android:debuggable=true]                      | Alta      | La depuración estaba habilitada en la aplicación, lo que facilita a los ingenieros inversos conectar un depurador a la misma. Esto permite volcar un stack trace y acceder a clases de ayuda de depuración. |
| 2  | Los datos de la aplicación pueden ser respaldados [android:allowBackup=true]             | Advertencia | Esta opción permite que cualquier persona respalde los datos de la aplicación a través de adb. Permite a los usuarios que han habilitado la depuración USB copiar los datos de la aplicación fuera del dispositivo. |
| 3  | El Broadcast Receiver (androidx.profileinstaller.ProfileInstallReceiver) está protegido por un permiso, pero el nivel de protección del permiso debe ser verificado. Permiso: android.permission.DUMP [android:exported=true] | Advertencia | Se encontró un Broadcast Receiver que se comparte con otras aplicaciones en el dispositivo, lo que lo hace accesible a cualquier otra aplicación. Está protegido por un permiso que no está definido en la aplicación analizada. Como resultado, el nivel de protección del permiso debe ser verificado. |

### Evaluación de Problemas del Manifiesto

- **Depuración habilitada**: Este hallazgo es crítico y debe ser corregido.

- **Respaldos de datos de la aplicación**: Se recomienda establecer `android:allowBackup=false`.

- **Broadcast Receiver expuesto**: Se debe verificar el nivel de protección del permiso.

### Recomendaciones

1. **Desactivar la depuración**: Configurar `android:debuggable=false`.
2. **Deshabilitar el respaldo de datos**: Establecer `android:allowBackup=false`.
3. **Revisar permisos del Broadcast Receiver**: Verificar el nivel de protección del permiso asociado.

## Conclusiones

Las vulnerabilidades identificadas en la aplicación deben abordarse para mejorar la seguridad general. Es fundamental implementar las recomendaciones mencionadas para reducir el riesgo de ataques y proteger los datos del usuario.
