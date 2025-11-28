# Proyecto_DNS_Spoofing


## 1. Descripción del ataque
DNS Spoofing, también conocido como DNS Cache Poisoning, es un ciberataque donde se alteran registros DNS para redireccionar a usuarios a páginas web fraudulentas. Este ataque explota vulnerabilidades en el DNS (Domain Name System) para manipular la resolución de los nombres de dominio en direcciones Ips, dirgiendo a los usuarios a sitios maliciosos en vez de los intencionados por el usuario.

## 2. Como funciona DNS Spoofing

Cuando un usuario intenta visitar una web, su dispositivo, ya sea un móvil, portátil u ordenador, envía una petición a un servidor DNS para obtener la dirección IP asociada a ese nombre de dominio. En un ataque de suplantación DNS, un atacante intercepta esta petición y responde con una dirección IP falsa, redireccionando al usuario al sitio malicioso. Esto puede ocurrir de varias formas:

1. Ataque Man-in-the-Middle (MITM attack): El atacante intercepta la comunicación entre el dispositivo del usuario y el servidor DNS, inyectando falsas respuestas de DNS
       
2. Envenenamiento de caché DNS (DNS cache poisoning): El atacante engaña al servidor DNS para que almacene registros incorrectos, los cuales son usados para responder futuras peticiones de usuarios.
       
3. Secuestro Local (Local Hijack): El atacante cambia la configuración DNS en el dispositivo del usuario o en el router para apuntar a un servidor DNS malicioso.




## 3. Consecuencias de DNS Spoofing

Entre las consecuencias incluidas están:

- Ataques de phishing: los usuarios son redirigidos a sitios web falsos que lucen idénticos al legítimo, donde puede, desde la ignorancia, introduzcan información sensible como nombres de usuarios, contraseñas, detalles de tarjetas de crédito, etc.

- Distribución de malware: los usuarios pueden ser redirigidos a sitios que automáticamente descarga e instala codigo malicioso en sus dispositivos

- Robo de datos: los atacantes pueden interceptar y robar información sensible trasnmitida entre el dispositivo del usuario y páginas fraudulentas



## 4. Medidas de protección

Para protegerse contra este ataque, se pueden implementar diversas contramedidas:

- DNSSEC (Domain Name System Security Extensions): este protocolo añade una capa de seguridad habilitando las respuestas DNS a ser verificadas por autenticación mediante firmas digitales.
      
- Encriptación de Transporte (Transport Encryption): usando el protocolo HTTPS entre otros seguros, se asegura de que los datos transmitidos entre el dispositivo del usuario y el servidor está encriptado, haciéndolo más difícil para interceptar y manipular
      
- Servidores DNS publicos: usar DNSs públicos de confianza como Cloudflare (1.1.1.1) o Google (8.8.8.8) puede proporcionar seguridad y privacidad adicional.
      
- Redes Virtuales Privadas (VPN): las VPN encriptan todo el tráfico de internet, proporcionando una capa de seguridad adicional contra el DNS spoofing


## 4. Herramientas utilizadas - Bettercap

Bettercap es un poderoso, fácilmente extensible, y framework portable escrito en Go, cuyo objetivo es ofrecer a analistas de seguridad, red teams y reverse engineers una herramienta fácil de usar, todo en uno, con todas las características que posiblemente necesiten para realizar reconocimiento y atacar redes Wifi, dispositivos bluetooth, redes IPv4 e IPv6 entre otros

<img src="https://www.bettercap.org/_astro/logo.9NeNvQAS_Z1OvlVx.webp" width="150px" heigth="auto">

## 5. Protocolos vulnerados

Los protocolos vulnerados en el DNS Spoofing incluyen el Protocolo de Resolución de Nombres (DNS), que es el sistema que traduce los nombres de dominio en direcciones IP. Los atacantes pueden manipular esta traducción para redirigir el tráfico de los usuarios hacia sitios web maliciosos. Además, el Protocolo de Resolución de Direcciones de Protocolo de Resolución (ARP), que permite a los dispositivos localizar direcciones IP a partir de nombres de dominio, también puede ser utilizado para redirigir el tráfico hacia servidores maliciosos.

## 6. Pasos importantes de la PoC (Prove of Concept) y pasos para su mitigación. 
```bash
sudo apt install bettercap
