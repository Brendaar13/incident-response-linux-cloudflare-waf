# 🛡️ Respuesta a Incidentes y Hardening Perimetral en Producción

Este repositorio documenta un caso real de **respuesta a incidentes, análisis de vulnerabilidades, erradicación de malware y endurecimiento (hardening)** llevado a cabo sobre una plataforma web en producción (entidad anonimizada por motivos de confidencialidad y seguridad operativa).

📄 **[Descargar / Ver el Informe Ejecutivo Completo (PDF)](./Informe_Ejecutivo_Incidente_Seguridad.pdf)**

---

## 📊 Impacto y Resultados Clave

* **Erradicación total de malware:** Detección, aislamiento y eliminación de *webshells* y puertas traseras (*backdoors*) sin interrumpir la disponibilidad del servicio.
* **Recuperación crítica de almacenamiento:** Resolución de un bucle de errores en PHP que redujo un archivo de logs saturado de **1.2 GB a 8.9 KB**, evitando la caída del servidor.
* **Mitigación de fuerza bruta y exposición:** Bloqueo de más de **11.000 intentos automatizados** por SSH y cierre perimetral de puertos internos expuestos (Node.js/Next.js en puerto 3000).
* **Defensa proactiva en el perímetro:** Despliegue de reglas personalizadas en **Cloudflare WAF** para frenar ataques automatizados, ejecución remota de código (RCE) y descarga de copias de seguridad.

---

## 🛠️ Stack Tecnológico y Herramientas

* **Seguridad Perimetral y Cloud:** Cloudflare (WAF, Managed Challenges, Proxy/DNS, SSL/TLS).
* **Sistemas y Paneles de Control:** Linux, Plesk, PHP (`local.ini` hardening), Monarx, Imunify360.
* **Redes y Cortafuegos:** Firewall de servidor (bloqueo de puertos no autorizados, enrutamiento estricto por HTTPS/443).
* **Tecnologías Web Auditadas:** Drupal 9, Node.js (Next.js).

---

## 🔍 Fases de la Intervención

1. **Contención y Limpieza de Malware:** Bloqueo inmediato de permisos de lectura/ejecución sobre directorios comprometidos, eliminación raíz de archivos maliciosos y auditoría antivirus.
2. **Saneamiento de Logs y Estabilización:** Corrección de rutas de sesión heredadas tras migración de panel (`ea-php80` -> `/tmp`) mediante configuración nativa en Plesk.
3. **Hardening de Red y Sistema:** Cierre del puerto público `3000` mediante regla prioritaria de firewall y endurecimiento de permisos en el sistema de archivos (`755` directorios, `644` archivos, `444` archivos críticos de arranque como `.htaccess` e `index.php`).
4. **PHP Hardening:** Deshabilitación explícita de funciones críticas susceptibles de ejecución remota de código.
5. **Reglas Proactivas en Cloudflare WAF:**
   * *Managed Challenge* obligatorio en rutas de autenticación (`/user`).
   * Bloqueo estricto de ejecución de scripts `.php` en directorios de carga (`/sites/default/files/`).
   * Bloqueo perimetral de descarga directa de archivos comprimidos (`.zip`, `.rar`, `.tar.gz`).
6. **Hoja de Ruta Preventiva:** Plan de migración de versión obsoleta (Drupal 9) y traslado de respaldos fuera del directorio público (`public_html`).

---

> **Nota de Confidencialidad:** Todos los datos identificativos, dominios, direcciones IP y rutas sensibles de la organización afectada han sido anonimizados siguiendo buenas prácticas de seguridad operativa (OpSec).
