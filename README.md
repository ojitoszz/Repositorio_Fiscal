# Caso de Estudio: Plataforma Segura de Gestión Documental y Fiscal

Este repositorio documenta el diseño, la arquitectura y el desarrollo de una plataforma robusta para la gestión segura de documentos críticos, con un enfoque en el cumplimiento fiscal y la interoperabilidad empresarial.

## 1. El Problema de Negocio

Las firmas de auditoría y los corporativos manejan un volumen masivo de documentos fiscales sensibles que deben ser almacenados de forma segura, ser accesibles y tener una trazabilidad impecable. Los sistemas tradicionales carecen de seguridad y flujos de trabajo eficientes.

Además, un reto clave es la integración con los sistemas de identidad existentes en las empresas. La plataforma debe conectarse de forma segura con proveedores como Microsoft Entra ID (antes Azure AD), Active Directory o cuentas de Google, para una experiencia de usuario unificada.

## 2. Mi Rol: Arquitecto de Solución y Líder Técnico

Como arquitecto y líder técnico del proyecto, mis responsabilidades abarcan todo el ciclo de vida de la solución:

-   **Análisis y Diseño:** Traducir los requerimientos de negocio (seguridad, cumplimiento, interoperabilidad) en una arquitectura técnica funcional y escalable.
-   **Liderazgo Técnico:** Definir el stack tecnológico, seleccionar los patrones de diseño adecuados y supervisar las buenas prácticas de desarrollo.
-   **Implementación:** Desarrollar los componentes clave del sistema.
-   **DevOps y Seguridad:** Diseñar e implementar la infraestructura de despliegue, el CI/CD y el monitoreo de seguridad.

## 3. La Solución Propuesta: Arquitectura Evolutiva y Resiliente

Diseñé una arquitectura híbrida y modular que combina un núcleo monolítico con microservicios. El diseño se centra en la seguridad proactiva, la flexibilidad y el uso de patrones de diseño probados.

### 3.1. Desafíos de Arquitectura Clave

-   **Federación de Identidad:** Para resolver la necesidad de múltiples proveedores de autenticación, se utilizará el patrón de diseño **Strategy**. Se creará una capa de abstracción que permitirá agregar o quitar proveedores (Entra ID, Google, etc.) sin modificar el núcleo de la aplicación.
-   **Almacenamiento Desacoplado:** El sistema se diseñará para poder almacenar archivos tanto en un servidor local seguro como en servicios de nube de terceros (SharePoint, Google Drive), adaptándose a las políticas de seguridad de cada cliente.

### 3.2. Diagrama de Arquitectura (Versión 1.0)

Este diagrama muestra la visión general de los componentes del sistema, sus interacciones y las tecnologías implicadas.



**Componentes Principales:**

-   **Monolito (Core - Laravel/PHP):** Gestiona la lógica de negocio, roles y permisos (RBAC), y la API principal, cuya seguridad se manejará con **Laravel Passport o Sactum (servidor OAuth2)**.
-   **Capa de Autenticación (Strategy Pattern):** Módulo abstracto para conectarse con Microsoft Entra ID, Google, y la gestión de usuarios nativa.
-   **Microservicio de OCR (Python/Tesseract):** Encargado de extraer texto de documentos escaneados.
-   **Base de Datos (PostgreSQL):** Almacena la metadata de los documentos y los logs de auditoría.
-   **Stack de Seguridad:** **CrowdSec (Inteligencia de Amenazas)** para bloquear IPs maliciosas de forma proactiva y **Wazuh + Elasticsearch** para el análisis de logs y detección de comportamientos anómalos (SIEM).

### 3.3. Escalabilidad y Alta Disponibilidad

-   **Balanceo de Carga para Base de Datos (Diseño Teórico):** Aunque la implementación inicial usará una sola instancia de BD, la arquitectura se diseña para una futura escalabilidad. Se planteará un esquema de **replicación primario-réplica**, donde las escrituras van al nodo primario y las lecturas se distribuyen entre las réplicas. **Este escenario se simulará localmente con Docker Compose** para validar la configuración.
-   **Estrategia de Respaldos:** Se implementará un **servidor de respaldos dedicado para el monolito y la base de datos**. Se configurarán copias de seguridad incrementales diarias y completas semanales para garantizar la recuperación ante desastres.

### 3.4. Estrategia de Pruebas y Calidad

Para garantizar el correcto funcionamiento de los permisos, se definirán **perfiles de usuario de prueba (ej. Administrador, Auditor, Usuario Básico)**. Se crearán tests automatizados para verificar que cada rol solo pueda acceder a las funciones y datos permitidos, asegurando la robustez del sistema de RBAC.

## 4. Stack Tecnológico

| Dominio | Tecnología | Propósito |
| :--- | :--- | :--- |
| **Backend (Monolito)** | PHP, Laravel, **Laravel Passport** | Lógica de negocio, API RESTful y servidor OAuth2 |
| **Bases de Datos** | PostgreSQL, MySQL | Persistencia de datos relacionales |
| **Infraestructura** | Docker, Traefik | Contenerización y Reverse Proxy |
| **CI/CD** | GitHub Actions, **Dockploy** | **Scripts de despliegue basados en contenedores** |
| **Seguridad** | **CrowdSec, Wazuh, Elasticsearch** | Inteligencia de Amenazas y SIEM |

## 5. Hoja de Ruta del Desarrollo (Serie Tutorial)

Este proyecto se desarrollará de forma pública como una serie de tutoriales. Aquí se enlazarán los videos a medida que se publiquen.

## 6. Resultados y Logros Esperados

-   **Centralización:** Unificar el 100% de la documentación fiscal en una única plataforma.
-   **Trazabilidad:** Implementar un registro inmutable de cada acción sobre un documento.
-   **Eficiencia:** Reducir el tiempo de búsqueda y recuperación de documentos.
-   **Seguridad Proactiva:** Mitigar el riesgo de brechas de seguridad y ataques conocidos.
-   **Interoperabilidad:** Garantizar una integración fluida con los sistemas de identidad empresarial.
