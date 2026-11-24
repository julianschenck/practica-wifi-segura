# Informe de Auditoría de Red Wi-Fi Insegura

## 1. Introducción

El objetivo de esta práctica es analizar los riesgos de seguridad y privacidad asociados a la navegación por Internet mediante redes Wi-Fi públicas.

Para ello, se realizó una exploración del tráfico HTTP generado al acceder al sitio web NeverSSL, utilizando las herramientas de desarrollador del navegador Google Chrome.

A través de esta actividad se busca identificar qué información se transmite sin cifrado, comprender las diferencias entre HTTP y HTTPS y analizar cómo el uso de una VPN puede contribuir a proteger nuestra información y reducir nuestra exposición ante posibles ataques informáticos.

## 2. Sitio analizado

- **Nombre del sitio:** NeverSSL.
- **Dirección de acceso:** `http://neverssl.com`
- **Herramienta utilizada:** Google Chrome DevTools.
- **Pestaña utilizada:** Network (Red).
- **Protocolo identificado:** HTTP.

NeverSSL es un sitio web que permite navegar mediante HTTP sin utilizar el cifrado proporcionado por HTTPS. Por este motivo, resulta adecuado para analizar los riesgos de transmitir información sin cifrar.

## 3. Evidencia observada

Para realizar la exploración se siguieron los siguientes pasos:

1. Se ingresó al sitio web NeverSSL.
2. Se abrieron las herramientas de desarrollador mediante la tecla F12.
3. Se seleccionó la pestaña Network.
4. Se desactivó la caché y se recargó la página.
5. Se seleccionó la solicitud correspondiente a la página principal.
6. Se analizaron los datos generales y los encabezados HTTP enviados.

### 3.1. Datos de la solicitud

| Elemento | Resultado observado |
|---|---|
| URL solicitada | `http://astoundingclearbrightrainbow.neverssl.com/online/` |
| Método HTTP | GET |
| Host | `astoundingclearbrightrainbow.neverssl.com` |
| Protocolo | HTTP |
| Puerto | 80 |
| Código de estado | 200 OK |
| Ruta solicitada | `/online/` |

El método GET indica que el navegador está solicitando un recurso al servidor web. El código de estado 200 OK indica que la solicitud se procesó correctamente.

**Evidencia 1 — Datos generales de la solicitud HTTP**

![Solicitud HTTP](evidencias/01-solicitud-http.png)

### 3.2. Encabezados HTTP enviados

Durante el análisis de la solicitud se identificaron los siguientes encabezados:

| Encabezado | Valor observado |
|---|---|
| Host | `astoundingclearbrightrainbow.neverssl.com` |
| Accept-Encoding | `gzip, deflate` |
| Accept-Language | `es-ES,es;q=0.9,en;q=0.8` |
| Cache-Control | `no-cache` |
| Connection | `keep-alive` |
| Pragma | `no-cache` |
| Referer | `http://neverssl.com/` |
| Upgrade-Insecure-Requests | `1` |

También se identificó el encabezado User-Agent:

```
Mozilla/5.0 (Linux; Android 16; Pixel 10)
AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/158.0.0.0 Mobile Safari/537.36
```

El User-Agent proporciona información declarada sobre el navegador, el sistema operativo y el tipo de dispositivo. Esta información, junto con otros datos como las preferencias de idioma, puede contribuir a la construcción de la huella digital del navegador (Fingerprinting).

**Evidencia 2 — Encabezados enviados por el navegador**

![Encabezados HTTP](evidencias/02-headers-solicitud.png)

### 3.3. ¿Qué protocolo utiliza el sitio?

El sitio analizado utiliza HTTP (Hypertext Transfer Protocol).

Esto se puede comprobar observando que la URL comienza con `http://` y que la comunicación utiliza el puerto 80.

HTTP no proporciona cifrado de la información transmitida entre el navegador y el servidor. Por lo tanto, un tercero que consiga interceptar el tráfico podría leer o modificar su contenido.

En cambio, HTTPS utiliza el protocolo TLS (Transport Layer Security), que proporciona cifrado, integridad de los datos y autenticación del servidor mediante certificados digitales.

| Característica | HTTP | HTTPS |
|---|---|---|
| Cifrado de datos | No | Sí, mediante TLS |
| Confidencialidad | No garantizada | Protegida |
| Integridad de los datos | No garantizada | Protegida mediante TLS |
| Autenticación del servidor | No mediante TLS | Mediante certificados digitales |
| Puerto habitual | 80 | 443 |

### 3.4. ¿Qué información puede observarse durante la solicitud?

Durante la exploración se identificaron los siguientes elementos:

- **URL solicitada:** permite conocer el recurso exacto al que se está accediendo.
- **Host:** identifica el dominio del servidor web.
- **Método GET:** indica que se está solicitando un recurso.
- **Headers:** contienen información adicional sobre la comunicación entre el navegador y el servidor.
- **User-Agent:** proporciona información declarada sobre el navegador y el dispositivo.
- **Accept-Language:** permite conocer las preferencias de idioma.
- **Referer:** identifica la página web de referencia.

Debido a que HTTP no cifra estos datos, podrían ser observados en texto plano por un tercero que consiga capturar el tráfico.

## 4. Riesgos encontrados

Navegar mediante HTTP desde una red Wi-Fi pública representa un riesgo para la seguridad y privacidad de los usuarios.

En lugares como cafeterías, aeropuertos y hoteles, diferentes dispositivos comparten la infraestructura de red. Si un atacante consigue interceptar el tráfico, podría acceder a información transmitida sin cifrado.

### 4.1. Sniffing

Consiste en la captura y análisis de paquetes que circulan por una red.

En una comunicación HTTP, un atacante que consiga capturar dichos paquetes podría observar información como las URLs solicitadas, los encabezados HTTP y el contenido de las solicitudes y respuestas.

### 4.2. Man-in-the-Middle (MITM)

Este ataque ocurre cuando un atacante consigue posicionarse como intermediario entre el dispositivo de la víctima y el servidor de destino.

Debido a la ausencia de cifrado y protección de integridad en HTTP, el atacante podría interceptar o modificar la información transmitida.

### 4.3. Evil Twin

Consiste en crear un punto de acceso Wi-Fi falso que imita el nombre de una red legítima, por ejemplo, la red de una cafetería o aeropuerto.

Si un usuario se conecta a esta red, el atacante podría controlar parte de su comunicación e intentar interceptar información sensible.

### 4.4. Información potencialmente expuesta

Dependiendo de los datos transmitidos mediante HTTP, un atacante podría interceptar:

- URLs y páginas web visitadas.
- Información del navegador y dispositivo.
- Cookies o identificadores de sesión transmitidos sin protección.
- Credenciales de acceso.
- Datos personales o información enviada mediante formularios.

En esta práctica no se introdujeron credenciales ni datos bancarios. Estos elementos representan riesgos potenciales del protocolo HTTP y no información sensible detectada durante la exploración.

Además, utilizar el modo incógnito no evita estos ataques, ya que su función principal es proteger la privacidad local del navegador y no cifrar el tráfico de red.

## 5. ¿Cómo ayuda una VPN?

Una VPN (Virtual Private Network) es una herramienta de seguridad que permite establecer un túnel cifrado entre el dispositivo del usuario y un servidor VPN.

Su objetivo es proteger el tráfico durante su recorrido por redes potencialmente inseguras.

### 5.1. Cifrado

Al utilizar una VPN, los paquetes enviados a través del túnel se cifran antes de salir del dispositivo.

Esto impide que un atacante que intercepte el tráfico dentro de la red Wi-Fi pública pueda interpretar directamente su contenido sin disponer de las claves necesarias para descifrarlo.

### 5.2. Túnel seguro y encapsulación

La VPN encapsula los paquetes originales dentro de otros paquetes protegidos y los transporta mediante un túnel cifrado hasta el servidor VPN.

De esta manera, las solicitudes HTTP y sus encabezados quedan protegidos durante el trayecto entre el dispositivo y dicho servidor.

### 5.3. Protección del tráfico y privacidad

Una VPN correctamente configurada permite:

- Proteger el tráfico frente a posibles interceptaciones en redes Wi-Fi públicas.
- Ocultar la dirección IP pública original frente al sitio web de destino.
- Reducir la visibilidad de la actividad de navegación ante otros participantes de la red local y el proveedor de Internet.
- Disminuir el riesgo de exposición de información durante su tránsito por la red pública.

Sin embargo, una VPN no garantiza el anonimato absoluto. Si el usuario inicia sesión en una cuenta personal, el sitio web puede continuar identificándolo.

### 5.4. Simulación de tráfico en Wireshark

Si se realizara una captura de paquetes mediante Wireshark sobre la interfaz física de red, podrían observarse las siguientes diferencias entre una conexión HTTP sin VPN y una conexión protegida mediante VPN.

| Información | HTTP sin VPN | HTTP con VPN |
|---|---|---|
| Método GET | Visible | Protegido dentro del túnel |
| URL completa | Visible | Protegida dentro del túnel |
| Host HTTP | Visible | Protegido dentro del túnel |
| User-Agent | Visible | Protegido dentro del túnel |
| Encabezados HTTP | Visibles | Protegidos dentro del túnel |
| Contenido HTTP | Legible | Cifrado dentro del túnel |
| Dirección IP del servidor VPN | No aplica | Visible |
| Tamaño y tiempo de los paquetes | Visibles | Visibles |

Sin una VPN, Wireshark podría identificar directamente las solicitudes HTTP mediante el filtro:

```
http.request.method == "GET"
```

Con una VPN activa y correctamente configurada, al capturar desde la interfaz física Wi-Fi o Ethernet, las solicitudes HTTP transportadas dentro del túnel no deberían aparecer directamente en texto plano.

En su lugar, podrían observarse paquetes correspondientes a la comunicación cifrada entre el dispositivo y el servidor VPN.

Esta comparación corresponde a una simulación teórica del comportamiento esperado y no a una captura real realizada con una VPN activa.

### 5.5. VPN vs. Modo Incógnito

| Característica | Modo Incógnito | VPN |
|---|---|---|
| Evita conservar el historial local de la sesión | Sí | No |
| Elimina las cookies de la sesión al cerrarla | Sí | No |
| Cifra el tráfico de red por sí mismo | No | Sí, dentro del túnel |
| Oculta la IP pública original al sitio web | No | Sí, normalmente |
| Protección frente a interceptaciones en Wi-Fi pública | No por sí mismo | Sí, para el tráfico transportado por el túnel |

El modo incógnito protege principalmente la privacidad local, mientras que una VPN protege la comunicación durante su recorrido por la red hasta el servidor VPN.

### 5.6. Kill Switch y limitaciones

Una medida adicional de seguridad es utilizar una VPN que disponga de la función Kill Switch.

Esta función interrumpe o bloquea la comunicación de red si se pierde la conexión VPN, evitando que el tráfico salga accidentalmente sin la protección del túnel.

Es importante aclarar que una VPN no sustituye a HTTPS. Si el sitio utiliza HTTP, la información puede volver a quedar expuesta después de salir del servidor VPN.

Por esta razón, se recomienda combinar el uso de una VPN confiable con conexiones HTTPS.

## 6. Las 3 Reglas de Oro para navegar en redes Wi-Fi públicas

### Regla de Oro 1 — Utilizar siempre conexiones HTTPS

Antes de introducir contraseñas, datos personales o información bancaria, verificar que el sitio utilice HTTPS y no presente advertencias de seguridad relacionadas con su certificado digital.

Evitar enviar información sensible mediante HTTP.

### Regla de Oro 2 — Utilizar una VPN confiable con Kill Switch

Activar una VPN de buena reputación antes de navegar por una red Wi-Fi pública.

Habilitar la función Kill Switch para evitar fugas accidentales de tráfico si se interrumpe el túnel seguro.

### Regla de Oro 3 — Verificar la autenticidad de la red Wi-Fi

Confirmar con el establecimiento el nombre oficial de su red inalámbrica antes de conectarse.

Evitar redes desconocidas, desactivar la conexión automática a puntos de acceso públicos y utilizar datos móviles para realizar operaciones especialmente sensibles, como transferencias bancarias.

## 7. Conclusión

Mediante esta práctica se identificó que el sitio NeverSSL utiliza el protocolo HTTP, permitiendo observar diferentes elementos de la comunicación, como la URL solicitada, el método GET, el Host y los encabezados enviados por el navegador.

La ausencia de cifrado en HTTP representa un riesgo para la confidencialidad y la integridad de la información, especialmente cuando se utilizan redes Wi-Fi públicas.

El uso de HTTPS, una VPN confiable y buenas prácticas de seguridad permite reducir considerablemente la exposición ante amenazas como el Sniffing, los ataques Man-in-the-Middle y los puntos de acceso fraudulentos.

Finalmente, se concluye que la seguridad y la privacidad digital deben abordarse mediante diferentes capas de protección, combinando herramientas tecnológicas y un comportamiento responsable por parte del usuario.
