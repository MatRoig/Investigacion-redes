# Investigacion-redes

## Investigación: Arquitectura de red 🌍

### Conceptos base

| Concepto | Definición simple | Nivel técnico breve | Ejemplo real |
|-|-|-|-|
Modelo OSI | Es un estándar que divide la comunicación en 7 capas, cada una con una función específica. | Modelo de referencia ISO/IEC que separa hardware (capas 1-2), direccionamiento (capas 3), control de flujo (capas 4) y aplicaciones (capas 5-7). | Cuando navegas por una web: la capa de Aplicación es el navegador, la de Transporte verifica que lleguen los datos y la de Física son los cables o el WiFi.
Subred (Subnet) | Es una división de una red grande en redes más pequeñas para organizar y mejorar la seguridad. | Fragmento lógico de una red IP, creado mediante una máscara de subred que separa la parte de red de la parte de host. | En una empresa, la subred 192.168.1.0/24 se asigna a Recursos Humanos y la 192.168.2.0/24 a Sistemas, aislando el tráfico entre departamentos.
DNS | Es el sistema que traduce nombres de dominio (ej: google.com) a direcciones IP numéricas. | Base de datos jerárquica distribuida que resuelve nombres de dominio a direcciones IP mediante consultas a servidores autoritativos. | Al escribir youtube.com, el DNS lo convierte en 142.250.184.46 para que tu navegador pueda conectarse al servidor de YouTube.
___
**¿Qué es el modelo OSI?**

Es un modelo teórico que divide la comunicación en red en 7 capas, cada una con una responsabilidad específica. Creado por ISO para estandarizar cómo se comunican los sistemas.

**¿Para qué sirve?**

Sirve como guía de referencia para que fabricantes y desarrolladores puedan construir sistemas que interoperen. Ayuda a aislar problemas: si algo falla, sabes qué capa revisar.

**Tabla de capas OSI**
|Capa|Nombre|¿Qué hace? (simple)|
|-|-|-|
1|Física|Transmite bits (0 y 1) por cables, fibra o WiFi. Define voltajes, frecuencias y conectores físicos.
2|Enlace|Permite la comunicación entre dispositivos en la misma red local. Detecta errores y organiza las tramas.
3|Red|Encamina los datos entre redes diferentes. Aquí trabajan las direcciones IP y los routers.
4|Transporte|Garantiza que los datos lleguen completos y en orden (TCP) o los envía rápido sin verificación (UDP).
5|Sesión|Establece, mantiene y finaliza la comunicación entre dos aplicaciones. Maneja "quién habla y cuándo".
6|Presentación|Traduce los datos: comprime, cifra o convierte formatos para que la aplicación los entienda.
7|Aplicación|Es la capa que usas directamente. Aquí están los protocolos HTTP, SMTP, FTP, etc.

**Flujo simplificado (para entender, no memorizar)**

Aplicación (7) → escribes un mensaje en WhatsApp

Presentación (6) → el mensaje se cifra (si es necesario)

Sesión (5) → se abre un canal de comunicación

Transporte (4) → se parte el mensaje en paquetes, se numeran

Red (3) → se le asigna una dirección IP de destino

Enlace (2) → se prepara para enviar por la red local

Física (1) → los bits viajan por el cable/WiFi

En el destino, el proceso se invierte: de capa 1 a capa 7, "desempaquetando" hasta que la aplicación recibe el mensaje.
___
**¿Qué es DNS?**

Es el sistema que traduce nombres de dominio (google.com) a direcciones IP (142.250.184.46). Como una agenda telefónica de internet.

**¿Por qué no usamos IP directamente?**

Porque los humanos recordamos palabras fácilmente, pero los ordenadores trabajan con números. Imagina tener que memorizar 172.217.168.46 para entrar a Google, y así para cada sitio web que visitas. Es inviable.

**¿Qué pasa cuando escribes google.com en el navegador?**

*Paso a paso:*

- Usuario escribe google.com en el navegador y presiona Enter.

- Consulta al caché local → tu computadora primero revisa si ya tiene guardada esa IP de visitas recientes. Si no está...

- Pregunta al resolver DNS (generalmente el router o el servidor de tu ISP) → "¿Tienes la IP de google.com?"

- El resolver consulta a servidores DNS jerárquicamente:

- Primero pregunta al servidor raíz (le dice "pregúntale a .com")

- Luego pregunta al servidor TLD de .com (le dice "pregúntale al servidor autoritativo de google")

- Finalmente pregunta al servidor autoritativo de google.com (este sí tiene la IP exacta)

- Obtiene la IP → 142.250.184.46

- Tu navegador se conecta a esa IP y carga la página de Google
___
**¿Qué es una subred?**

Es una división de una red grande en redes más pequeñas. Como dividir un edificio grande en departamentos separados con sus propias puertas.

**¿Para qué sirve?**

- Organizar los dispositivos por función o departamento

- Mejorar seguridad (un área no puede ver el tráfico de otra sin permiso)

- Reducir tráfico (evita que los datos viajen por toda la red sin necesidad)

**¿Por qué dividir redes?**

- Porque si todos los dispositivos están en una misma red gigante:

- El tráfico se congestiona

- Cualquier dispositivo puede intentar atacar a cualquier otro

- Es más difícil gestionar quién tiene acceso a qué

**Ejemplo developer: Red local vs red en cloud**

| Escenario	| Red local (tu casa/oficina) | Red en cloud (AWS, Azure, etc.) |
|-|-|-|
Sin subredes|Todos los dispositivos (PC, móvil, smart TV, impresora) en la misma red 192.168.1.0/24. Todos se "ven" entre sí.|Tu base de datos y tu servidor web en la misma red. Si alguien vulnera el servidor web, accede directamente a la BD.
Con subredes|Red 192.168.1.0/24 para equipos de trabajo, 192.168.2.0/24 para invitados con wifi limitado.|Subred pública para el servidor web (acceso desde internet). Subred privada para la base de datos (solo accesible desde el servidor web, nunca expuesta).
___
**¿Qué pasa cuando tu app no encuentra el servidor?**

|Problema|Qué ocurre técnicamente|Relación con conceptos|
|-|-|-|
No encuentra el servidor|La URL está mal escrita, el servidor está apagado o el puerto no es el correcto. Llegaste a la IP pero no hay nada escuchando.|IP (el servidor existe pero no atiende ahí)
No resuelve el dominio|El DNS no pudo traducir el nombre a IP. Tu app se queda sin saber a dónde ir.|DNS (falló la traducción)
No puede conectarse|El servidor existe y la IP está bien, pero no se pudo establecer la conexión (firewall, red caída, CORS).|Red (el camino está bloqueado)
___
**Significado de errores**

| Error | ¿Qué significa? | Causa común |
|-------|-----------------|-------------|
| DNS not found | El nombre de dominio no pudo traducirse a IP | Dominio mal escrito, servidor DNS caído, sin conexión a internet |
| Host unreachable | La IP existe pero no hay ruta para llegar | Servidor apagado, red caída, firewall bloqueando |
| Network error | Fallo genérico de comunicación | CORS, timeout, certificado SSL inválido, pérdida de conexión |

**¿Qué revisarías primero como developer?**

1. **¿Hay internet?** → Abre cualquier página web

2. **¿Resuelve el dominio?** → `nslookup google.com` o `ping google.com`

3. **¿Llega al servidor?** → `ping [IP]` si la tienes

4. **¿El puerto está abierto?** → `telnet [IP] [puerto]` o `curl -v [URL]`

5. **¿El backend está levantado?** → Revisa logs de Spring Boot

6. **¿Es CORS?** → Mira consola del navegador (frontend)

**Orden lógico (de más simple a más complejo)**

1. Tu conexión a internet
2. DNS (nombre → IP)
3. Ruta de red (llegar al servidor)
4. Puerto (servicio escuchando)
5. Aplicación (logs del backend)
___
**¿Es problema de DNS?**
- Si el dominio no se traduce a IP → **SÍ**
- Si accedes por IP y funciona, por dominio no → **SÍ**

**¿Es problema de red?**
- Si ni por IP ni por dominio se puede acceder → **SÍ**
- Firewall, puerto cerrado, servidor apagado, cloud sin puerto expuesto

**¿Es problema de configuración?**
- Si en local funciona pero en producción no → **SÍ**
- IP mal configurada en backend, puerto incorrecto, CORS mal seteado, app escuchando en 127.0.0.1 en vez de 0.0.0.0
___
| Concepto | Analogía |
|----------|----------|
| **OSI** | Una línea de ensamblaje: cada trabajador (capa) hace una tarea específica (empaquetar, etiquetar, enviar) y al final el producto llega listo para usar. Si algo falla, sabes qué trabajador revisar. |
| **DNS** | El directorio de un edificio: buscas "Empresa XYZ" (dominio) y te dice el número de oficina (IP). No tendrías que memorizar que están en el piso 3, oficina 12. |
| **Subnet** | Los pisos de un edificio: el piso 1 es ventas, piso 2 es contabilidad. Cada piso tiene su propio acceso y los del piso 2 no pueden entrar al 1 sin autorización. Organiza y aísla. |
___
**¿Qué es un VPC en cloud?**

Es una red privada y aislada dentro de la nube (AWS, Azure, etc.) donde puedes desplegar tus recursos. Tú controlas las IPs, subredes, firewalls y rutas como si fuera tu propio datacenter virtual.

**¿Qué es una IP privada vs pública?**

- **IP privada:** Es interna, solo visible dentro de tu red (ej: 10.0.1.5). Se usa para comunicación entre servicios internos como tu base de datos y tu backend. No es accesible desde internet.
- **IP pública:** Es única a nivel mundial, accesible desde cualquier lugar con internet. Se asigna a elementos que deben ser visibles desde fuera, como un load balancer o un servidor web.

**¿Qué es un load balancer?**

Es un componente que actúa como "repartidor de tráfico". Recibe las peticiones de los usuarios y las distribuye entre múltiples servidores para evitar sobrecargas, dar alta disponibilidad (si un servidor falla, otro sigue atendiendo) y gestionar certificados SSL.
___
## Investigación: Comunicación en Redes 🌐

| Concepto | Definición simple | Nivel técnico breve | Ejemplo real |
|----------|------------------|---------------------|--------------|
| **TCP** | Protocolo que garantiza que los datos lleguen completos y en orden. | Orientado a conexión, establece handshake (SYN, SYN-ACK, ACK), verifica entrega y reenvía paquetes perdidos. | Cuando cargas una página web, TCP asegura que lleguen todos los archivos (HTML, CSS, JS) sin errores. |
| **UDP** | Protocolo que envía datos rápidamente sin verificar si llegaron. | No orientado a conexión, sin control de errores ni orden. Más rápido pero menos confiable. | Videollamadas y streaming: si se pierde un paquete, no notas un fotograma perdido, pero la fluidez se mantiene. |
| **Puerto** | Número que identifica un proceso o servicio específico en un servidor. | Rango 0-65535. Los puertos conocidos (0-1023) son estándar (80 HTTP, 443 HTTPS, 22 SSH). | Tu app Spring Boot escucha en puerto 8080. Cuando haces `localhost:8080`, el puerto le dice al sistema a qué proceso enviar la petición. |
| **HTTP** | Protocolo de comunicación entre navegador y servidor web. | Sin estado (stateless), basado en petición-respuesta. Métodos: GET, POST, PUT, DELETE. Códigos de estado: 200 OK, 404 Not Found, 500 Error. | Al enviar un formulario, tu navegador hace una petición HTTP POST al servidor con los datos. |
___

#### ¿Cuál es la principal diferencia entre TCP y UDP?**

TCP **garantiza la entrega** de los datos; UDP **no garantiza nada**, solo los envía sin verificar.

#### ¿Cuál es más confiable? ¿por qué?
TCP. Porque confirma que cada paquete llegó y si no, lo reenvía.

#### ¿Cuál es más rápido? ¿por qué?
UDP. Porque no espera confirmaciones ni reenvía paquetes perdidos.

| Protocolo | Característica clave | Uso típico |
|-----------|---------------------|------------|
| **TCP** | Orientado a conexión, confiable, ordena paquetes | Web (HTTP), correo (SMTP), FTP, SSH |
| **UDP** | No orientado a conexión, sin confirmación, más rápido | Streaming, videollamadas, DNS, juegos online |
___
#### ¿Qué es un puerto en una red?
Es un número que identifica un proceso o servicio específico dentro de un servidor. Es como el número de apartamento en un edificio (IP).

#### ¿Por qué una aplicación necesita un puerto?
Porque una misma IP puede tener múltiples servicios funcionando. El puerto permite que el sistema sepa a qué aplicación entregar cada petición.

#### ¿Qué significan estos puertos?

| Puerto | Uso |
|--------|-----|
| 80 | HTTP estándar (web sin cifrar) |
| 443 | HTTPS estándar (web cifrada con SSL) |
| 3000 | Puerto común para desarrollos frontend (React, Node) |
| 5432 | Puerto por defecto de PostgreSQL |

#### ¿Qué pasa cuando ejecutas npm run dev?

1. Levanta un servidor de desarrollo (React, Vite, etc.)
2. El servidor se "engancha" al puerto 3000 (o el que esté configurado)
3. Tu navegador accede a `http://localhost:3000`
4. El sistema operativo sabe que todo lo que llegue al puerto 3000 debe enviarlo a ese servidor
___
#### ¿Qué es HTTP?
Es el protocolo que usan navegadores y servidores para comunicarse en la web. Funciona por peticiones y respuestas, sin memoria entre una y otra (stateless).

#### ¿Cómo funciona una petición HTTP?
1. El cliente (frontend) envía una petición con método, URL, headers y opcionalmente un cuerpo (body)
2. El servidor (backend) procesa y responde con un código de estado, headers y opcionalmente datos

#### ¿Qué significan GET, POST, PUT, DELETE?

| Método | Qué hace | Ejemplo |
|--------|----------|---------|
| **GET** | Obtener datos | Obtener lista de usuarios |
| **POST** | Crear un nuevo recurso | Registrar un nuevo usuario |
| **PUT** | Actualizar un recurso completo | Modificar todos los datos de un usuario |
| **DELETE** | Eliminar un recurso | Borrar un usuario |

#### Flujo HTTP completo

| Paso | Quién | Qué hace |
|------|-------|---------|
| 1 | Frontend | El usuario hace clic en "Ver usuarios". React prepara una petición GET a `http://localhost:8080/api/usuarios` |
| 2 | Request | La petición viaja por internet con método, URL, headers y datos |
| 3 | Backend | Spring Boot recibe la petición, el controlador la procesa y consulta a la base de datos |
| 4 | Response | El backend responde con código 200 OK y los datos en formato JSON |
| 5 | Frontend | React recibe la respuesta, actualiza el estado y muestra los usuarios en pantalla |
___
#### ¿Qué pasa cuando haces fetch("http://localhost:3000/api")?

1. Tu navegador arma una petición **GET** a la dirección indicada
2. `localhost` se resuelve a `127.0.0.1` (tu propia máquina)
3. La petición se envía al **puerto 3000**
4. Busca si hay algún proceso escuchando en ese puerto
5. Si hay un servidor (React, Node, etc.), recibe la petición y responde
6. Si no hay nada, falla con error de conexión

#### ¿Qué protocolo se usa?
**HTTP**. El prefijo `http://` indica explícitamente el protocolo.

#### ¿Qué puerto?
**3000**. Es el número después de los dos puntos (`:3000`).

#### ¿Qué tipo de comunicación?
**TCP**. HTTP se construye sobre TCP porque necesita que los datos lleguen completos y en orden.

#### Ejemplo práctico

| Situación | Resultado |
|-----------|----------|
| Servidor React corriendo en puerto 3000 | ✅ Fetch exitoso, obtiene respuesta |
| Servidor apagado o puerto incorrecto | ❌ `ERR_CONNECTION_REFUSED` |
| Backend en puerto 8080 pero fetch apunta a 3000 | ❌ Error porque no hay servidor en 3000 |
| Frontend y backend en distintos puertos | ⚠️ Posible error CORS si backend no lo permite |
___
#### ¿Qué significa cada error?

| Error | ¿Qué significa? | Causa común |
|-------|-----------------|-------------|
| **Port already in use** | El puerto ya está ocupado por otro proceso | Dos aplicaciones intentando usar el mismo puerto (ej: dos Spring Boot en 8080) |
| **Connection refused** | El puerto está cerrado o no hay nada escuchando | El servidor no está levantado, el puerto es incorrecto o firewall bloquea |
| **Timeout** | La petición tardó demasiado y se canceló | Servidor lento, red inestable, servidor no responde, o el tiempo de espera es muy corto |

#### ¿Qué revisarías como developer?

| Error | Revisar |
|-------|---------|
| **Port already in use** | 1. `lsof -i :8080` (mac/linux) o `netstat -ano \| findstr :8080` (windows) para ver qué proceso usa el puerto <br> 2. Cambiar el puerto en `application.properties`: `server.port=8081` |
| **Connection refused** | 1. ¿El servidor está corriendo? <br> 2. ¿El puerto es correcto? <br> 3. `curl http://localhost:8080/api/test` para probar |
| **Timeout** | 1. ¿El servidor responde lento? Revisar logs <br> 2. ¿Hay conexión a internet? <br> 3. Aumentar timeout en cliente o investigar por qué el servidor tarda |

#### Comandos útiles (para copiar y pegar)

```bash
# Ver qué proceso usa un puerto (mac/linux)
lsof -i :8080

# Ver qué proceso usa un puerto (windows)
netstat -ano | findstr :8080

# Probar si un puerto responde
curl -v http://localhost:8080

# Probar conectividad básica
ping localhost
```
___

#### Escenario: frontend no se conecta al backend

| Posible causa | ¿Qué revisar? | ¿Cómo detectarlo? |
|---------------|---------------|-------------------|
| **Problema de puerto** | El backend escucha en un puerto diferente al que llama el frontend | Ver `fetch` apunta a `3000` pero backend está en `8080`. Probar `curl localhost:8080` |
| **Problema de protocolo** | Frontend llama con `http` pero backend espera `https` o viceversa | Revisar URL del fetch. Si es `https` y backend no tiene SSL → error de certificado o conexión rechazada |
| **Problema de red** | Backend no está levantado, firewall bloquea, o están en distintas máquinas sin acceso | `ping` a la IP, `curl` al puerto, verificar que backend esté corriendo |

#### Diagnóstico rápido (orden lógico)
¿Backend está corriendo?
→ Revisar consola/logs de Spring Boot

¿Puerto correcto?
→ curl http://localhost:8080/api/test
→ Si falla → backend no escucha ahí

¿Frontend apunta al puerto correcto?
→ Revisar fetch("http://localhost:8080/api/...")

¿Es CORS?
→ Abrir consola navegador (F12) → pestaña Network
→ Si hay error CORS, aparece en consola

¿Backend acepta peticiones desde otro origen?
→ Spring Boot: agregar @CrossOrigin o configurar CORS globalmente

text

#### Ejemplo concreto

**Síntoma:** `fetch("http://localhost:3000/api/usuarios")` falla

**Diagnóstico:**
```bash
# Paso 1: ¿Algo escucha en puerto 3000?
curl http://localhost:3000
# Resultado: curl: (7) Failed to connect to localhost port 3000
```
Conclusión: No hay servidor en puerto 3000. El frontend está llamando al puerto equivocado. Debería llamar al backend en 8080.

Solución: Cambiar fetch a http://localhost:8080/api/usuarios
___
| Concepto | Analogía |
|----------|----------|
| **TCP** | Llamada telefónica: marcas, la otra persona contesta "hola", hablan, se despiden. Todo llega en orden y si no escuchaste algo, pides que te lo repitan. |
| **UDP** | Lanzar un mensaje con una botella al mar: lo envías sin saber si llegará, no hay respuesta, pero es rápido. Si se pierde, no te enteras. |
| **Puerto** | Número de departamento en un edificio. La dirección (IP) es el edificio, el puerto dice a qué puerta llamar. |
| **HTTP** | El idioma que hablan cliente y servidor. Ambos acuerdan frases como "dame esto" (GET) o "toma esto" (POST) y respuestas como "todo bien" (200) o "no existe" (404). |
___
#### ¿Qué es HTTPS?
Es HTTP con una capa de seguridad (SSL/TLS) que cifra la comunicación. La "S" significa seguro. Evita que alguien pueda leer o modificar los datos mientras viajan.
markdown
#### ¿Qué es REST?
Es un estilo de arquitectura para construir APIs. Usa HTTP y sus métodos (GET, POST, PUT, DELETE) de forma estándar. Cada recurso tiene su propia URL y se opera con el método correspondiente.
markdown
#### ¿Qué es un endpoint?
Es una URL específica donde una API ofrece un recurso. Ejemplo: `GET /api/usuarios` es un endpoint que devuelve usuarios. Es el "punto de entrada" a una funcionalidad.
markdown

#### Relación entre conceptos
- HTTPS → protocolo seguro

- REST → estilo de diseño

- Endpoint → URL concreta dentro de una API REST
___
## Investigación: Seguridad 🔐

| Concepto | Definición simple | Nivel técnico breve | Ejemplo real |
|----------|------------------|---------------------|--------------|
| **Permissions** | Reglas que determinan qué puede hacer cada usuario en un sistema. | Control de acceso basado en roles (RBAC) o políticas. Define acciones permitidas (leer, escribir, eliminar) sobre recursos. | En una app, un usuario normal puede ver su perfil (permiso de lectura), pero solo un administrador puede eliminar cuentas (permiso de escritura/eliminación). |
| **Password segura** | Contraseña difícil de adivinar, con longitud y variedad de caracteres. | Combina mayúsculas, minúsculas, números y símbolos. Mínimo 12 caracteres. No incluye palabras comunes ni datos personales. | `G7#kL9$mQ2@x` en lugar de `123456` o `contraseña`. Se recomienda usar gestores de contraseñas. |
| **Vulnerabilidad** | Punto débil en un sistema que puede ser explotado para causar daño. | Fallo de diseño, configuración o código que permite a un atacante comprometer la confidencialidad, integridad o disponibilidad. | Una API que no valida datos de entrada permite inyección SQL. El atacante podría robar toda la base de datos. |
| **Intrusión** | Acceso no autorizado a un sistema o red. | Evento donde un atacante evade controles de seguridad. Puede ser externo (hacker) o interno (empleado malicioso). | Alguien adivina la contraseña de un administrador y entra al panel de control de un servidor. |
| **Seguridad de red** | Conjunto de medidas para proteger la infraestructura de red contra accesos no autorizados. | Incluye firewalls, VLANs, cifrado, control de acceso, segmentación de red y monitoreo de tráfico. | En una empresa, la red de empleados está separada de la red de invitados. Firewalls bloquean puertos no esenciales y todo el tráfico está cifrado con VPN. |
___
#### ¿Qué significa Autenticación?
Es el proceso de **verificar quién eres**. Demostrar tu identidad con credenciales (usuario/contraseña, huella, etc.).

#### ¿Qué significa Autenticación?
Es el proceso de **verificar quién eres**. Demostrar tu identidad con credenciales (usuario/contraseña, huella, etc.).

#### ¿Cuál es la diferencia entre ambas?

| Concepto | Pregunta que responde | Momento |
|----------|----------------------|---------|
| **Autenticación** | ¿Quién eres? | Primero |
| **Autorización** | ¿Qué puedes hacer? | Después |

#### Ejemplo developer: Login → ¿qué es?
**Autenticación.** El usuario envía usuario/contraseña, el sistema valida que existan y sean correctos, y devuelve un token (JWT) o sesión.

#### Ejemplo developer: Roles (admin/user) → ¿qué es?
**Autorización.** El usuario ya autenticado tiene un rol. Admin puede eliminar usuarios; user solo puede ver su perfil.

#### Relación con APIs y sistemas con usuarios

**Flujo completo:**

1. Usuario envía credenciales → POST /api/login

2. Servidor autentica → devuelve token JWT

3. Usuario guarda token (localStorage, cookie)

4. En cada petición → envía token en header Authorization

5. Servidor valida token y verifica permisos (roles)

6. Si tiene permiso → ejecuta acción
Si no → responde con 403 Forbidden
___
#### ¿Qué hace que una contraseña sea segura?

| Característica | Ejemplo |
|----------------|---------|
| Larga (mínimo 12 caracteres) | `J#9kL$2mQp@7` |
| Combina mayúsculas, minúsculas, números y símbolos | No usar palabras del diccionario |
| No contiene datos personales | Evitar nombres, fechas de nacimiento |
| Única para cada servicio | No reutilizar la misma contraseña |

#### ¿Por qué NO se deben guardar contraseñas en texto plano?
Si un atacante accede a la base de datos, obtiene todas las contraseñas directamente. Los usuarios repiten contraseñas, por lo que también quedarían expuestos otros servicios.

#### ¿Qué es hashing?
Es una función matemática que transforma una contraseña en un código único e irreversible. No se puede "deshashear" para obtener la original. Al hacer login, se hashea la ingresada y se compara con el hash guardado.

#### Ejemplo developer: ¿Cómo se manejan contraseñas en backend?

// ✅ Guardar solo el hash
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}

// Registrar
usuario.setPassword(passwordEncoder.encode(contraseñaIngresada));

// Login
if (passwordEncoder.matches(contraseñaIngresada, usuario.getPassword())) {
    // autenticado
}

Registro: contraseña → BCrypt.hash() → hash guardado en BD
Login: contraseña ingresada → BCrypt.matches() → compara con hash guardado
___
#### ¿Qué es una vulnerabilidad?
Es una debilidad o fallo en un sistema (código, configuración, diseño) que puede ser aprovechada por un atacante para causar daño, robar datos o tomar control.

#### 3 tipos comunes de vulnerabilidades

| Vulnerabilidad | ¿Qué es? | Ejemplo |
|----------------|----------|---------|
| **SQL Injection** | Insertar código SQL malicioso en campos de entrada para manipular la base de datos | Un formulario de login recibe `' OR '1'='1` como contraseña y devuelve todos los usuarios, permitiendo acceso sin credenciales |
| **XSS (Cross-Site Scripting)** | Insertar código JavaScript malicioso que se ejecuta en el navegador de otros usuarios | Un comentario contiene `<script>alert('robo cookie')</script>`. Al cargar la página, se ejecuta en cada visitante |
| **Exposición de datos** | Información sensible queda accesible sin protección adecuada | API devuelve todos los usuarios con contraseñas en la respuesta, o backup de base de datos en URL pública |

#### ¿Cómo una mala validación rompe una app?

La mala validación permite que datos maliciosos entren al sistema y provoquen comportamientos no deseados.

**Ejemplo concreto:**

| Escenario | Sin validación | Con validación |
|-----------|----------------|----------------|
| Campo edad acepta texto | Usuario envía "abc" → app crash o comportamiento erróneo | Rechaza con error: "Edad debe ser número" |
| Comentario con script | Se ejecuta en navegadores de otros usuarios (XSS) | Se escapa el HTML, se muestra como texto plano |
| Input sin límite | Usuario envía 10,000 caracteres → base de datos llena o app lenta | Limita a 200 caracteres |

Mala validación → Datos corruptos → Error interno → App rompe o queda en estado inconsistente
___
#### ¿Qué es una intrusión?
Es el acceso no autorizado a un sistema, red o aplicación. Ocurre cuando un atacante evade los controles de seguridad y entra donde no debería.

#### ¿Qué busca un atacante?

| Objetivo | Qué quiere |
|----------|------------|
| Robo de datos | Información personal, credenciales, tarjetas de crédito, propiedad intelectual |
| Control del sistema | Tomar el servidor para usarlo en ataques más grandes (botnet) o cifrar datos y pedir rescate (ransomware) |
| Daño | Eliminar información, dejar la app inoperativa, dañar reputación |
| Punto de entrada | Usar el sistema comprometido para atacar otros sistemas internos |

#### ¿Qué pasa si una app es comprometida?

| Consecuencia | Impacto |
|--------------|---------|
| Fuga de datos | Multas legales (GDPR, etc.), demandas, pérdida de confianza de usuarios |
| Servicio caído | Pérdida de ingresos, clientes insatisfechos |
| Reputación dañada | Difícil de recuperar, pérdida de clientes |
| Costos de recuperación | Investigación, parches, notificaciones a afectados |
| Acceso privilegiado | Atacante puede moverse a otros sistemas internos |

#### Ejemplo: Robo de datos

**Escenario:** App con SQL Injection en login

1. Atacante envía `' OR '1'='1` en campo usuario
2. Query devuelve todos los usuarios
3. Atacante inicia sesión como el primer usuario (admin)
4. Explora API, encuentra endpoint `/api/usuarios`
5. Descarga toda la base de datos (nombres, emails, contraseñas hasheadas)
6. Vende los datos en la dark web o usa contraseñas para acceder a otros servicios

#### Ejemplo: Acceso no autorizado

**Escenario:** Una aplicación de correo electrónico

**La falla:**
Un usuario normal inicia sesión en su cuenta de correo. Al ver la URL del navegador, nota que su bandeja de entrada está en:
`https://app.com/inbox?user=123`

Decide cambiar el número por `124` y presiona Enter. El sistema le muestra la bandeja de entrada de otro usuario sin pedir permiso.

**¿Qué pasó?**
- El sistema solo verificó que el usuario estuviera autenticado (haber iniciado sesión)
- Pero **no verificó** que fuera el dueño de la cuenta que intentaba ver
- Confió ciegamente en el número que el usuario envió en la URL

**Consecuencia:**
El atacante puede recorrer todos los números y descargar los correos de miles de usuarios, incluyendo información personal, contraseñas olvidadas o datos bancarios.

**Cómo evitarlo:**
- Nunca confiar en identificadores que el usuario puede modificar
- El sistema debe comparar: "¿El usuario autenticado es el mismo que el dueño del recurso que pide?"
- Si no coinciden, rechazar el acceso
___
#### ¿Qué protege la seguridad de red?
Protege los datos mientras viajan entre dispositivos, controla quién puede acceder a la red y previene ataques externos o internos.

#### ¿Qué es un firewall?
Es un filtro que controla el tráfico de red. Decide qué puertos y direcciones pueden entrar o salir. Actúa como un portero que solo deja pasar lo autorizado.

#### ¿Qué es HTTPS y por qué es importante?
Es HTTP con cifrado SSL/TLS. Protege los datos mientras viajan por internet. Sin HTTPS, cualquier persona en la misma red puede ver las contraseñas y datos que envía el usuario.

#### ¿Por qué usar HTTPS en APIs?

Porque sin HTTPS, las contraseñas, tokens y datos personales viajan como texto visible. Cualquier persona en la misma red WiFi (como un café, aeropuerto o universidad) puede capturarlos y usarlos para robar cuentas o información.
___
#### ¿Qué errores comunes cometen los developers en seguridad?

| Error | ¿Qué pasa? |
|-------|-----------|
| No validar inputs | SQL Injection, XSS, datos corruptos, app crash |
| Guardar contraseñas en texto plano | Si la BD es robada, todas las contraseñas quedan expuestas |
| No proteger rutas con autorización | Usuarios comunes acceden a endpoints de admin |
| Exponer información en errores | Stack traces con nombres de tablas, rutas internas o credenciales |
| Hardcodear secretos en código | Claves API, contraseñas de BD en GitHub expuestas |
| No usar HTTPS en producción | Credenciales y tokens viajan en texto plano, cualquier atacante en la red los captura |
| Confiar en datos del frontend | Validar solo en frontend pero no en backend |

#### ¿Qué pasaría si no validas inputs?

- SQL Injection: atacante roba toda la BD
- XSS: scripts maliciosos se ejecutan en navegadores de usuarios
- Datos corruptos: edad = "texto" rompe cálculos
- Buffer overflow: campos sin límite saturan memoria o BD

#### ¿Qué pasaría si no proteges rutas?

- Usuario normal accede a /admin → borra usuarios, ve datos sensibles
- Atacante descubre endpoints ocultos → explota funcionalidades no autorizadas
- Cualquier usuario autenticado modifica datos de otros usuarios

#### ¿Qué pasaría si no usas HTTPS?

- En WiFi público: atacante captura token JWT y suplanta al usuario
- Credenciales de login viajan en texto plano → robo de cuentas
- Datos sensibles (tarjetas, documentos) interceptados
- Red corporativa: empleados maliciosos capturan tráfico de otros
___
#### Caso práctico real

**Escenario:**

> “Un usuario logra acceder a información que no le corresponde.”

#### ¿Qué falló?
Falló el **control de acceso**. El sistema permitió que un usuario accediera a recursos que no estaban autorizado a ver. Esto puede deberse a falta de validación de permisos en el backend o exposición de identificadores que pueden ser manipulados.

#### ¿Es problema de permisos, vulnerabilidad o intrusión?

| Tipo | Aplica | Explicación |
|------|--------|-------------|
| **Permisos** | ✅ SÍ | El sistema no verificó que el usuario tuviera permiso para acceder a ese recurso específico |
| **Vulnerabilidad** | ✅ SÍ | Falta de validación en el código es una falla explotable (IDOR - Insecure Direct Object References) |
| **Intrusión** | ❌ NO | No hubo una "entrada forzada", el usuario actuó dentro del sistema usando una brecha de diseño |

#### ¿Cómo lo evitarías como developer?

1. Nunca confiar en IDs enviados por el cliente sin verificar propiedad

2. Usar autorización basada en roles

3. Usar identificadores no secuenciales o encriptados

4. Validar siempre en backend, nunca solo en frontend

5. Principio de mínimo privilegio
___
| Concepto | Analogía |
|----------|----------|
| **Permissions** | Los **niveles de acceso** en un edificio: el conserje puede entrar a todas las oficinas, un empleado solo a su piso, un visitante solo al lobby. |
| **Password** | La **llave** de tu departamento. Si es débil o la compartes, cualquiera puede entrar. Una buena llave es única y difícil de copiar. |
| **Vulnerabilidad** | Una **ventana rota o cerradura defectuosa**. No es el robo en sí, pero es el punto débil que permite que ocurra. |
| **Intrusión** | Un **ladrón que entra por la ventana rota**. Ya ocurrió el acceso no autorizado. Alguien está dentro donde no debería. |
| **Seguridad de red** | El **sistema de seguridad del edificio**: cámaras, alarmas, guardias en la entrada, puertas blindadas. Todo lo que protege el perímetro y controla quién entra y sale. |
___
#### ¿Qué es JWT?
JSON Web Token. Es un estándar para crear tokens que contienen información (claims) firmada digitalmente. El token es autosuficiente: no necesita guardarse en servidor porque lleva los datos del usuario y la firma que valida que no fue alterado.

#### ¿Qué es autenticación con tokens?
Es un sistema sin estado (stateless). El usuario se autentica una vez, recibe un token, y lo envía en cada petición. El servidor solo valida la firma del token sin necesidad de guardar sesiones. Es común con JWT.

#### ¿Qué es rate limiting?
Es limitar la cantidad de peticiones que un usuario o IP puede hacer en un tiempo determinado. Previene ataques de fuerza bruta, sobrecarga del servidor y abusos. Ejemplo: máximo 100 peticiones por minuto.
