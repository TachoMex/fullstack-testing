Full-Stack Testing

Seth Reyes & Gilberto Vargas

Kueski

---

¿Cuál es el principal objetivo del QA?

---

Minimizar las perdidas debido a errores

---

¿Qué es realmente el Full-Stack Testing?

*	¿Front-end/Back-end?
*	El testing no es una fase, es un proceso.
* ¿Cómo probar la perfecta integración de aplicaciones, configuraciones e infraestructura?
* ¿Cómo aseguras la calidad en la operación de las personas?
* ¿Cómo evitas perdidas debido a errores?

---
¿Por qué es importante un QA Full-Stack?

*	Desarrollo ágil
*	Integración con el proceso
* Probar la apliación una vez que está en producción
* Probar la organización ante situaciones críticas

---
¿Qué busca el "QA Full-Stack"?

*	Conocimiento del producto
*	Prevención de defectos
* ¡Prevención de castrofes!

---
¿Cómo suelen ver el desarrollo de un producto?

*	Requerimiento
* Diseño
* Desarrollo
* Integración/Testing
* Deploy
* Mantenimiento
* Incidencias

---

Fase: Requerimiento

*	Validación de requerimientos
*	Analisis del impacto

---
Fase: Diseño/Desarrollo

*	Planeación de las pruebas
*	Diseño de las pruebas
*	Automatización

---
Fase: Integración/Testing

*	Pruebas de los componentes
* Pruebas de las características
*	Pruebas de regresión
*	UAT

---
Fase: Deploy

*	Pruebas "smoke test" automatizadas
* Staging
*	Feature follow-up
* sanity checks
	* Revisar puertos
	* Revisar accesos a bases de datos
	* Revisar accesos a otros backend
	* etc

---
Site Reliability Engineering (SRE)

- Server Reboot Engineer
- Responsables de la disponibilidad del sistema
- Mejorar la confiabilidad de los sistemas

---
SRE y el primer SRE

* Margaret Hamilton trabajaba en el progrma Apolo de la NASA y el MIT
* Tenía una hija, Lauren la cual un día llevo al trabajo
* Lauren como toda niña fue al trabajo y se fue a jugar en los simuladores
* Una de las simulaciones falló debido a una mala configuración elegida por la piloto
* Los datos de navegación de la nave fueron eliminados causando en un fracaso
* Si esto hubiera pasado en una misión real, habría causado un accidente muy grande
---
Fase: Mantenimiento

* Chaos monkey
* Tirar recursos en producción de forma ordenada
* Simulacros
* Plan de recuperación ante castrofes (DRP)

---
Chaos monkey

- Desarrollado por netflix
- Destrucción de servidores virtuales
- Probar la respuesta ante incidencias de los equipos de operación

---
Botones de autodestrucción

- ¿Qué operaciones de hacerse podrían detener la apliación?
- "No le aprietes ahí porque se rompe"
- "Sí, pero de todos modos ya los usuarios saben que no le tienen que mover a eso"

---
¿Mantenimiento antes de llegar a producción?

* Configurar de forma errónea los servicios
* Configurar usuarios con menos permisos de los necesarios
* Tirar componentes parciales dentro de un ambiente
* Relizar operaciones "peligrosas"
* Reiniciar un servidor

---
Fase: Incidencias

* Postmortems
* Mejoras a la infraestructura de monitoreo
* Agregar casos de prueba
* Analisis de la causa raíz

---
Postmortems

- Todo eventualmente se rompe
- Que algo falle es totalmente normal, no te enojes ni busques a quien echarle la culpa
- Si algo ya se rompió, documéntalo y evita que ocurra de nuevo
- Enfoca los esfuerzos en buscar la causa raíz

---
Conclusión

- Probar

---
Contacto

- Gilberto Vargas. Site Reliability Engineer @ Kueski
  - github.com/tachomex
	- https://www.linkedin.com/in/tachomex
	- gilberto@kueski.com
- Seth Reyes.  Software Development Engineer in Test @ Kueski
	- set.reyes@kueski.com


---
Objetivos del QA en cada etapa
Tabla de procesos/tecnologias/recursos
Lecturas recomendadas
Como probar front/back/integration
Librerías de Unit test
Herramientas de automation
