---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-0-introduccion-al-bastionado/practicas/t1-ud-0-investigacion-sobre-el-modelo-zero-trust/","title":"Investigación sobre el modelo Zero Trust","tags":["bastionado-de-redes-y-sistemas","practica","zero-trust"],"noteIcon":"","dg-note-properties":{"title":"Investigación sobre el modelo Zero Trust","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","zero-trust"]}}
---


# 📝 Tarea 1: Investigación sobre el modelo Zero Trust

> [!note] Objetivos de la Práctica
> - Comprender el paradigma ***Zero Trust*** y sus principios.
> - Identificar modelos de referencia y servicios comerciales que lo implementan.
> - Comparar *Zero Trust* con el modelo de bastionado clásico (perimetral).
> - Seleccionar y citar correctamente fuentes de información fiables.

## 📐 Recordatorio Teórico
- [[Bastionado de redes y sistemas/Unidad 0 Introducción al bastionado/Apuntes/3. Bastionado o hardening#3.4 Zero Trust\|3.4 Zero Trust]]
- [[Bastionado de redes y sistemas/Unidad 0 Introducción al bastionado/Apuntes/3. Bastionado o hardening#3.3 Definición y objetivos del bastionado\|3.3 Definición y objetivos del bastionado]]
- [[Bastionado de redes y sistemas/Unidad 0 Introducción al bastionado/Apuntes/1. Introducción al bastionado#1.3 Tarea de la unidad\|1.3 Tarea de la unidad]]

## 🛠️ Enunciado

Busca información sobre el paradigma ***Zero Trust*** y elabora un documento que responda a estos puntos:

1. **Qué es *Zero Trust*:** origen, definición y principios básicos.
2. **Modelos de referencia** que siguen el paradigma (por ejemplo, NIST SP 800-207 o el modelo de madurez de CISA) y sus componentes o pilares.
3. **Servicios de proveedores** que lo soportan: elige al menos tres y describe sus características más representativas.
4. **Implementación:** cómo se podría implantar en una organización (por fases, qué tecnologías intervienen).
5. **Ventajas e inconvenientes** frente a los modelos de bastionado anteriores o «clásicos».
6. **Fuentes** consultadas, citadas correctamente.

> [!tip] Sugerencia
> Una **tabla comparativa** *Zero Trust* frente a modelo perimetral (confianza, perímetro, acceso, autenticación, segmentación, visibilidad, coste) ayuda mucho a responder el punto 5.

## 🔗 Información de interés

- NIST SP 800-207, *Arquitectura de confianza cero* (traducción al español): [PDF](https://dreamlab.net/wp-content/uploads/2024/11/Arquitectura_de_confianza_cero-NIST.SP_.800-207.pdf) · original en inglés: [NIST](https://csrc.nist.gov/pubs/sp/800/207/final)
- CISA, *Zero Trust Maturity Model* v2.0 (en español): [PDF](https://www.cisa.gov/sites/default/files/2024-05/zero_trust_maturity_model_v2_508%20%281%29_ES.pdf)
- Keeper Security: [Zero Trust frente a los modelos de seguridad tradicionales](https://www.keepersecurity.com/blog/es/2025/01/22/zero-trust-vs-traditional-security-models-whats-the-difference/)
- Scalefusion: [Confianza cero frente a seguridad tradicional](https://blog.scalefusion.com/es/Confianza-cero-frente-a-seguridad-tradicional/)
- Akamai: [¿Qué es Zero Trust?](https://www.akamai.com/es/glossary/what-is-zero-trust)
- Oracle: [¿Qué es la seguridad Zero Trust?](https://www.oracle.com/latam/security/what-is-zero-trust/)

> [!warning] Fuentes comerciales
> Los artículos de Keeper, Scalefusion, Akamai y Oracle son de **fabricantes** que venden soluciones *Zero Trust*: son útiles, pero pueden ser parciales. Contrasta siempre con las fuentes neutrales (NIST, CISA, CCN, INCIBE).

## ✅ Evaluación

La solución **no es única**: al ser un ejercicio de investigación, se puede desarrollar de muchas maneras.

| Apartado | Criterio | Puntuación |
| :-: | --- | :-: |
| 1 | Selección y presentación correcta de la información | 2,5 |
| 2 | Explicación del paradigma y de las características indicadas | 2,5 |
| 3 | Exposición y documentación correcta | 2,5 |
| 4 | Correcta selección de fuentes de información | 2,5 |
| | **Total** | **10** |

> [!success]- Orientación para la corrección
> Una buena respuesta debería incluir, como mínimo:
> - **Principios:** no confiar por defecto por estar «dentro» de la red; verificar explícitamente cada acceso (identidad, dispositivo, contexto); mínimo privilegio; asumir que la red ya está comprometida (*assume breach*); monitorización continua.
> - **Componentes NIST:** motor de políticas (PE), administrador de políticas (PA) y punto de aplicación de políticas (PEP).
> - **Pilares CISA:** identidad, dispositivos, redes, aplicaciones y cargas de trabajo, y datos; más visibilidad y analítica, automatización y orquestación, y gobernanza como capacidades transversales.
> - **Tecnologías:** MFA, IAM y SSO, acceso condicional, microsegmentación, ZTNA, EDR, SASE/SSE, cifrado.
> - **Ventajas:** reduce el movimiento lateral, se adapta al teletrabajo y al *cloud*, mejora la visibilidad y el control de accesos, limita el impacto de credenciales robadas.
> - **Inconvenientes:** implantación compleja y gradual (años), coste, integración con sistemas heredados, posible fricción para los usuarios, dependencia de la gestión de identidades.
>
> (Conocimiento general, verificar.)

> [!quote]- Fuentes
> - `Tarea 1 - UD 0.pdf`

🡠 [[Bastionado de redes y sistemas/Unidad 0 Introducción al bastionado/Apuntes/4. Introducción al plan director de seguridad\|4. Introducción al plan director de seguridad]] ⮅ [[Bastionado de redes y sistemas/Unidad 0 Introducción al bastionado/Prácticas/T1 - UD0 Investigación sobre el modelo Zero Trust#📝 Tarea 1: Investigación sobre el modelo Zero Trust\|#📝 Tarea 1: Investigación sobre el modelo Zero Trust]]
