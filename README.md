# FTOptix Advanced - Monitoreo de tanque por MQTT

Proyecto educativo de FactoryTalk Optix para publicar y recibir el nivel de un tanque mediante un broker MQTT remoto. La interfaz incluye representaci�n del tanque, control deslizante, valores publicado y recibido, y tendencia del nivel recibido.

## Abrir el proyecto

1. Utilizar **FactoryTalk Optix Studio Desktop 1.7.3.39**, versi�n con la que se guard� esta copia.
2. Descargar o clonar el repositorio completo.
3. En Studio, seleccionar **Open**, buscar `FTOptixAdvancedRodolfo/FTOptixAdvancedRodolfo.optix` y abrirlo.
4. Conservar una copia antes de aceptar una actualizaci�n si se abre con otra versi�n de Studio.

## Configurar la conexi�n propia

La copia p�blica tiene **Broker address, Username y Password vac�os**. Cada persona debe introducir los datos de su propio broker; el proyecto no se conecta sin esa configuraci�n.

En `MQTT > MQTTClient1`, configurar:

| Propiedad | Configuraci�n |
| --- | --- |
| Broker address | Nombre de servidor del broker, sin prefijo de protocolo ni puerto |
| Port | `8883` |
| Client Id | Identificador �nico para esta instancia; evitar ejecutarlo simult�neamente en otra instancia con el mismo identificador |
| SSL/TLS enabled | `True` |
| Validate broker certificate | `True` |
| CA certificate file | `ProjectFiles/PKI/Own/Certs/isrgrootx1.pem`, incluido como certificado p�blico de CA; utilizar la CA correspondiente si el broker propio requiere otra |
| User identity type | `Username/Password` |
| Username / Password | Credenciales MQTT del broker propio, con permiso para publicar y suscribirse a `Tank1` |

La contrase�a de la cuenta web del proveedor y las credenciales MQTT pueden ser diferentes. Los secretos y las claves privadas deben permanecer fuera del repositorio.

## Modelo y MQTT

- `Model/Publisher/TankLevel`: variable **Double** controlada desde el control deslizante.
- `Model/Subscriber/TankLevel`: variable **Double** independiente que se actualiza mediante la suscripci�n MQTT.
- `MQTTPublisher1`: carpeta `Model/Publisher`, tema `Tank1`, muestreo **Change in value**, **QoS 2**.
- `MQTTSubscriber1/Subscriptions/DefaultJSONSubscription1`: carpeta `Model/Subscriber`, tema `Tank1`, formato JSON predeterminado de Optix. La suscripci�n conserva su QoS predeterminado del proyecto.
- `UI/MainWindow`: representaci�n gr�fica del tanque, control de nivel publicado, valor publicado, valor recibido y Trend enlazado al nivel del Subscriber.
- `UI/NativePresentationEngine/Start window`: `MainWindow`.

## Comprobaci�n funcional

1. Configurar el broker propio y guardar el proyecto.
2. Ejecutar **Emulator** desde Optix Desktop.
3. En un cliente externo conectado al mismo broker, suscribirse a `Tank1`.
4. Mover el control del nivel, por ejemplo a 20, 50 y 80, dejando unos segundos entre cambios.
5. Comprobar mensajes nuevos con `TankLevel` en el cliente externo, actualizaci�n del valor recibido en Optix y cambios de la tendencia.

Las dos variables del modelo se mantienen separadas para poder comprobar el recorrido de publicaci�n y recepci�n a trav�s del broker.

## Evidencia de la interfaz

Captura de la HMI del proyecto facilitada por el participante:

![HMI de monitoreo MQTT del tanque](evidencias/hmi-mqtt.png)

## Contenido publicado

Se incluye el proyecto editable y su certificado p�blico de CA. Se excluyen compilaciones, cach�s, logs, almacenes locales de secretos, claves privadas, credenciales de autenticaci�n y documentos del curso.
