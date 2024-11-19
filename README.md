# Levantamiento de Servidor HAPI FHIR con Guía de Implementación de ESAVI

Este repositorio contiene los pasos y recursos necesarios para el despliegue de un servidor HAPI FHIR configurado con la **Guía de Implementación de ESAVI (Eventos Supuestamente Atribuibles a la Vacunación o Inmunización)**. Incluye la configuración del servidor, el uso de herramientas como SUSHI para generar guías personalizadas, y el procedimiento para cargar grandes volúmenes de datos.

## Contenido del Repositorio

1. **`Levantamiento de Servidor.md`**  
   Instrucciones para clonar, configurar y ejecutar un servidor HAPI FHIR utilizando Docker. También se describe cómo incorporar la Guía de Implementación de ESAVI y configurar los parámetros en `application.yaml`.

2. **`SUSHI - Guía de Implementación.md`**  
   Guía para crear artefactos personalizados para la Guía de Implementación de ESAVI utilizando FHIR Shorthand (FSH) y SUSHI. Incluye los pasos para la instalación de SUSHI, generación de proyectos, y publicación en el servicio de integración continua de FHIR.

3. **`Volcamiento de Volúmenes de Datos.md`**  
   Detalles sobre cómo cargar grandes cantidades de datos al servidor mediante el uso de **Bundles**, específicamente los definidos en la Guía de Implementación de ESAVI.

## Requisitos Previos

- **Node.js** para instalar SUSHI (ver [Guía de Instalación](https://nodejs.org/)).
- **Docker** para ejecutar el servidor HAPI FHIR.
- Acceso a la Guía de Implementación de ESAVI, disponible en [FHIR IG Publisher](https://build.fhir.org/ig/PanAmericanHealthOrganization/ESAVI-IG-FHIR/package.tgz).

## Instalación y Configuración

1. **Clonar el Repositorio HAPI FHIR Starter**  
   ```bash
   git clone https://github.com/hapifhir/hapi-fhir-jpaserver-starter.git
   cd hapi-fhir-jpaserver-starter
   git checkout v6.2.0
   ```

2. **Configurar el Archivo `application.yaml`**  
   Ajustar los parámetros de la Guía de Implementación en `src/main/resources/application.yaml`:
   ```yaml
   implementationguides:
       opsesavi:
           url: https://build.fhir.org/ig/PanAmericanHealthOrganization/ESAVI-IG-FHIR/package.tgz
           name: uv.esavi
           version: 0.1.43
   supported_resource_types:
     - CodeSystem
     - Questionnaire
     - QuestionnaireResponse
     - StructureDefinition
     - Subscription
     - ValueSet
     - Binary
   ```

3. **Ejecutar el Contenedor Docker**  
   ```bash
   docker build -t hapi-fhir-esavi .
   docker run -d -p 8080:8080 hapi-fhir-esavi
   ```

## Uso de SUSHI para Personalización

Siga los pasos en `SUSHI - Guía de Implementación.md` para generar artefactos adicionales o personalizar la guía.

## Volcamiento de Datos

Para cargar grandes volúmenes de datos, use el recurso **Bundle** según las instrucciones en `Volcamiento de Volúmenes de Datos.md`:
```bash
POST {baseURL}/Bundle
```
