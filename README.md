# 🦅 Raven Security - Proyecto Integral de Ciberseguridad y Resiliencia operativa

Este repositorio contiene la documentación, análisis y estrategias de remediación desarrolladas para el proyecto final de auditoría de seguridad de **Raven Security**. El trabajo aborda un enfoque dual: un análisis forense exhaustivo tras un incidente crítico y un plan completo de endurecimiento (*hardening*) y continuidad de negocio.

---

## 🔍 1. Resumen del Incidente (Caso de Estudio)
El 08 de octubre de 2026, la infraestructura de la organización sufrió una intrusión dirigida que comprometió el servidor central de producción (monolito Debian que aloja servicios Web WordPress, Base de Datos MySQL y servidor FTP).

### Hallazgos del Análisis Forense (Con Autopsy)
* **Vector de Entrada:** Ataque de fuerza bruta exitoso sobre el servicio SSH, explotando la habilitación del login directo de la cuenta `root` con credenciales triviales.
* **Impacto:** Exfiltración de datos financieros y manipulación del sistema de archivos aprovechando permisos globales inseguros (`CHMOD 777`) en el directorio web.
* **Persistencia:** Modificación de registros y alteración de credenciales en la base de datos para asegurar accesos secundarios.

---

## 🛠️ 2. Fase de Remediación y Hardening (Debian Bastionado)
Se ejecutó un plan de contención y aseguramiento definitivo sobre la máquina Debian para mitigar los riesgos identificados:

* **Modernización del Sistema:** Actualización integral de paquetes y Kernel de Debian para eliminar la obsolescencia de software y mitigar vulnerabilidades conocidas (RCE). Se automatizó la gestión de parches de seguridad.
* **Saneamiento Criptográfico de Identidades:** Rotación completa de credenciales del sistema y de aplicaciones. Las claves ya no se almacenan en texto plano, implementando:
  * **Sistema Operativo (Debian):** Hashes protegidos mediante el algoritmo estándar **Yescrypt (SHA-512)**.
  * **Capa Aplicativa (WordPress/DB):** Hashes basados en **MD5 con Salts dinámicos** y llaves de salado en configuración para neutralizar ataques de tablas arcoíris (*Rainbow Tables*).
* **Políticas de Privilegio Mínimo:** Restricción de accesos SSH (prohibiendo login de root y forzando llaves RSA) y corrección estricta de permisos de archivos a `644` y directorios a `755`.

---

## 🗺️ 3. Rediseño de Arquitectura de Red (Packet Tracer)
Para evitar que un compromiso en los servicios web afecte a la red interna, se rediseñó la topología utilizando una segmentación estricta controlada por un Firewall perimetral:

* **Zona Desmilitarizada (DMZ):** Aislamiento del servidor Debian (Web/FTP público), impidiendo conexiones directas desde el exterior hacia la zona corporativa.
* **LAN Interna Protegida:** Ubicación de los equipos de empleados y administración de TI, permitiendo el acceso SSH al servidor *únicamente* desde la IP asignada al administrador de sistemas.

---

## 📈 4. Plan de Continuidad de Negocio y Resiliencia (DRP)
Estrategia diseñada bajo estándares industriales para garantizar la alta disponibilidad de los servicios críticos:

* **Métricas Operativas:** Establecimiento de un **RTO < 4 horas** (Tiempo de recuperación) y un **RPO < 24 horas** (Punto de restauración de datos).
* **Estrategia de Respaldos 3-2-1:**
  * **3 copias** de seguridad de los activos de información.
  * **2 soportes** físicos diferenciados (Almacenamiento local NAS y réplica Cloud).
  * **1 copia Off-site:** Resguardo inmutable en una región de nube aislada para protección contra ataques de Ransomware.

---

## 📋 5. Entregables Incluidos
Los documentos ejecutivos y técnicos se encuentran distribuidos de la siguiente manera:
* `/docs/Informe_Forense_RavenSecurity.pdf` - Reconstrucción detallada del ataque.
* `/docs/Plan_Continuidad_Negocio_DRP.pdf` - Estrategia de resiliencia y métricas.
* `/network-topology/` - Diagrama ejecutable de Packet Tracer y capturas de la DMZ.

---
**Desarrollado por:** Julio Leandro Fraga Pirola  
*Analista de Ciberseguridad - Proyecto Final 2026*
