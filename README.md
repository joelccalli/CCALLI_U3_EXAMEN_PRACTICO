# CCALLI_U3_EXAMEN_PRACTICO – Informe de Auditoría al Despliegue DevIA360

## 1. Resumen Ejecutivo

Este informe presenta los resultados de la auditoría al proceso de despliegue automatizado del sistema DevIA360, que utiliza **Vagrant y Chef** para instalar una instancia de **WordPress en entorno local**. La evaluación identificó múltiples riesgos en cuanto a seguridad, configuración y buenas prácticas de despliegue continuo. 

Se analizaron archivos clave del proyecto como `Vagrantfile`, recetas Chef (`default.rb`, `metadata.rb`) y se verificó el acceso al sistema mediante `http://localhost:8080`. Asimismo, se recopiló evidencia técnica y se elaboró una matriz de riesgos para priorizar los hallazgos.

---

## 2. Objetivo de la Auditoría

### Objetivo General:
Evaluar las configuraciones, prácticas de seguridad y calidad del entorno automatizado de despliegue DevIA360 basado en Vagrant y Chef, identificando vulnerabilidades técnicas que comprometan su integridad, confidencialidad y disponibilidad.

### Objetivos Específicos:
1. Verificar configuraciones de red y exposición de puertos en el archivo `Vagrantfile`.
2. Identificar credenciales o datos sensibles mal gestionados en recetas Chef.
3. Comprobar la existencia o ausencia de segregación de entornos (desarrollo y producción).
4. Validar la presencia y funcionamiento de logs del sistema en `/var/log/`.
5. Documentar evidencia técnica y generar una matriz de riesgos priorizada.

---

## 3. Alcance

La auditoría está limitada al entorno local de despliegue de DevIA360 con base en:
- Archivo `Vagrantfile`.
- Archivos de configuración y recetas Chef.
- Máquina virtual en VirtualBox con acceso vía `localhost:8080`.
- Entorno operativo Linux Ubuntu provisionado por Vagrant.

---

## 4. Metodología

Se aplicó una metodología de **auditoría técnica basada en inspección manual, pruebas de ejecución y análisis de configuración**. Los pasos fueron:

- Clonación del repositorio `Chef_Vagrant_Wp`.
- Ejecución del comando `vagrant up`.
- Acceso al sistema WordPress en entorno local.
- Revisión de archivos clave (`Vagrantfile`, `attributes/default.rb`, `metadata.rb`).
- Toma de evidencias.
- Elaboración de matriz de riesgos con impacto y probabilidad estimada.

---

## 5. Hallazgos de Auditoría

### 5.1. Revisión de Configuraciones

- **Exposición de puertos sin restricción**
  - El archivo `Vagrantfile` expone el puerto 80 del huésped al 8080 del host.
  - No hay restricciones adicionales, lo que podría permitir acceso no controlado.
  - **Evidencia:** Ver Anexo C.

- **Configuraciones sin autenticación**
  - No se detecta ningún mecanismo de autenticación para el acceso a la instancia desplegada.
  - **Evidencia:** Ver Anexo C.

### 5.2. Análisis de Recetas Chef

- **Credenciales en texto plano**
  - En `attributes/default.rb` se observa el uso de contraseñas o nombres de usuario en texto plano.
  - Esto compromete la confidencialidad si el repositorio es compartido.
  - **Evidencia:** Ver Anexo D.

- **Versión de software fija**
  - En `metadata.rb` se especifican versiones de software fijas, lo que puede generar vulnerabilidades si no se actualiza periódicamente.
  - **Evidencia:** Ver Anexo E.

### 5.3. Pruebas de Seguridad

- **Ausencia de registros de logs**
  - El directorio `/var/log/` está presente pero no se generan logs relevantes del despliegue o accesos.
  - **Evidencia:** Ver Anexo F.

- **Falta de segregación de ambientes**
  - Todo el despliegue se realiza en un solo entorno sin distinción entre desarrollo y producción.
  - **Evidencia:** Ver Anexo G.

---

## 6. Análisis de Evidencias

Las evidencias fueron almacenadas en la carpeta `/evidencias/` y vinculadas como anexos al informe:

- **Anexo A** – Resultado del comando `vagrant status`.
- **Anexo B** – Acceso exitoso a WordPress vía `http://localhost:8080`.
- **Anexo C** – Fragmento del archivo `Vagrantfile`.
- **Anexo D** – Fragmento de `attributes/default.rb` con credenciales.
- **Anexo E** – Fragmento de `metadata.rb` con versión fija.
- **Anexo F** – Captura del directorio `/var/log/`.
- **Anexo G** – Evidencia de la ausencia de ambientes separados.

---

## 7. Recomendaciones

- **Encriptar o usar variables de entorno** para manejar credenciales en recetas Chef.
- **Configurar autenticación HTTP** o firewall en el entorno local para limitar acceso al puerto 8080.
- **Implementar control de versiones dinámico** y revisar actualizaciones de paquetes.
- **Agregar un sistema de logging detallado** durante el proceso de despliegue.
- **Separar ambientes dev/test/prod** en carpetas o configuraciones distintas para buenas prácticas DevOps.

---

## 8. Matriz de Riesgos

| Riesgo                         | Causa (Vínculo a Anexo)     | Impacto | Probabilidad | Nivel de Riesgo |
|-------------------------------|------------------------------|---------|--------------|-----------------|
| Credenciales sin cifrado      | `attributes/default.rb` (D) | Alto    | 90%          | Crítico         |
| Puertos sin restricciones     | `Vagrantfile` (C)           | Medio   | 80%          | Alto            |
| Sin segregación de ambientes  | Config. única (G)            | Alto    | 70%          | Alto            |
| Sin logs del sistema          | `/var/log/` vacío (F)        | Medio   | 60%          | Medio           |
| Versión fija de software      | `metadata.rb` (E)            | Bajo    | 50%          | Bajo            |

---

## 9. Conclusiones

La auditoría reveló **importantes vulnerabilidades técnicas** que deben ser corregidas antes de desplegar DevIA360 en un entorno real. La gestión inadecuada de credenciales, la falta de controles de acceso y la configuración monolítica son riesgos que comprometen la seguridad y mantenimiento del sistema.

Es recomendable implementar medidas preventivas, aplicar principios DevSecOps, y establecer un flujo seguro para el despliegue continuo.

---

## 10. Anexos

Todos los anexos están ubicados en la carpeta `/evidencias/` del repositorio:

- **Anexo A** – `vagrant_status.png`
- **Anexo B** – `wordpress_localhost.png`
- **Anexo C** – `vagrantfile_fragment.png`
- **Anexo D** – `credenciales_default_rb.png`
- **Anexo E** – `metadata_version.png`
- **Anexo F** – `log_directory.png`
- **Anexo G** – `sin_segregacion.png`

---

📌 **Autor:** Joel Robert Ccalli Chata  
🛠️ **Curso:** Auditoría de Sistemas – Unidad III  
📆 **Fecha:** Junio 2025
