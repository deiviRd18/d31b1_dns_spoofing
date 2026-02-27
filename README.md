## 📌 1. Objetivo del Script
El objetivo de esta herramienta desarrollada en Python utilizando el framework **Scapy**, es demostrar la vulnerabilidad de resolución de nombres en redes locales sin controles de Capa 2. El script intercepta las consultas DNS de la víctima mediante un ataque previo de envenenamiento ARP (ARP Spoofing) y falsifica la respuesta del registro `itla.edu.do`, redirigiendo el tráfico de la víctima hacia un servidor web malicioso controlado por el auditor.

## 🗺️ 2. Topología de Red
La infraestructura simulada en GNS3 consta de los siguientes nodos:

* **Host Atacante (Kali Linux):** `20.24.20.2` (Ejecución del framework D31B1)
* **Host Víctima (Windows 10):** `20.24.20.19`
* **Gateway / Router Core (Cisco C7200):** `20.24.20.15`
* **Servidor DNS/RADIUS (Windows Server):** `20.24.20.100`
* **Switch de Acceso:** ESW1 (Módulo NM-16ESW)

## ⚙️ 3. Requisitos para utilizar la herramienta
Para replicar esta auditoría, el entorno debe contar con:
* Sistema Operativo basado en Linux (Ej. Kali Linux).
* Intérprete de **Python 3**.
* Librería **Scapy** instalada (`pip install scapy`).
* Privilegios de superusuario (`root`) para la manipulación de paquetes de Capa 2 y Capa 3.
* Servicio web local activo en la máquina atacante (Ej. Apache2 o Python HTTP Server) para recibir a la víctima.

## 🎛️ 4. Parámetros Usados
El script interactivo requiere y utiliza los siguientes parámetros durante su ejecución:
* `Interfaz_Red`: Interfaz de salida del atacante (ej. `eth0`).
* `IP_Victima`: Dirección IP del host Windows 10 a interceptar.
* `IP_Gateway`: Dirección IP del router C7200 para forjar los paquetes ARP.
* `Dominio_Target`: El dominio a falsificar (`itla.edu.do`).
* `IP_Spoof`: La dirección IP del atacante hacia donde se redirigirá el tráfico web (`20.24.20.2`).

## 📸 5. Capturas de Pantalla y Evidencias

1.  **Ejecución del Script:**
    `![Ejecución]<img width="854" height="629" alt="image" src="https://github.com/user-attachments/assets/cd14278c-f198-42b8-b97f-377aed5f938c" />

2.  **Redirección Exitosa en la Víctima:**
    `![Víctima Comprometida]<img width="1177" height="1007" alt="image" src="https://github.com/user-attachments/assets/5ff3ac83-0fc6-47bf-8f44-31e919a7feff" />

3.  **Topología en GNS3:**
    `![Topología]<img width="650" height="495" alt="image" src="https://github.com/user-attachments/assets/1d8ccc95-f29b-4e8c-a8f0-c6ab69a56e94" />


## 🛡️ 6. Medidas de Mitigación
Para asegurar la infraestructura contra este vector de ataque, se recomiendan y documentan las siguientes medidas:

1.  **Dynamic ARP Inspection (DAI) y DHCP Snooping:** Implementar estas tecnologías de Capa 2 en los switches de acceso para validar los paquetes ARP y evitar que un host falsifique la dirección MAC del Gateway. *(Hallazgo de Auditoría: Durante el laboratorio se evidenció que el módulo NM-16ESW utilizado carece de soporte de hardware para estas funciones, por lo que se recomienda actualizar la infraestructura de conmutación a un modelo multicapa moderno).*
2.  **Hardening del Gateway (C7200):** Configuración de defensas en el router mediante los comandos `no ip proxy-arp` y `no ip redirects` en la interfaz LAN para evitar redirecciones de tráfico maliciosas.
3.  **DNSSEC:** Implementar extensiones de seguridad para el DNS en el Windows Server para firmar criptográficamente las respuestas y evitar la falsificación de registros.
4.  **Aseguramiento AAA:** Restringir el acceso administrativo a los equipos de red mediante políticas RADIUS centralizadas para evitar cambios no autorizados en la configuración.

## 🎥 7. Entregables
* **Enlace al Video Demostrativo :** (https://img.youtube.com/vi/bMs6VIKwUv0/0.jpg)](https://www.youtube.com/watch?v=bMs6VIKwUv0)
