---
{"dg-publish":true,"permalink":"/bastionado-de-redes-y-sistemas/unidad-3-administracion-de-credenciales-de-acceso/practicas/p2-ud-3-validez-y-autenticidad-de-certificados-web/","title":"Validez y autenticidad de certificados web","tags":["bastionado-de-redes-y-sistemas","practica","pki"],"noteIcon":"","dg-note-properties":{"title":"Validez y autenticidad de certificados web","materia":"Bastionado de redes y sistemas","ciclo":"Formación Profesional","tags":["bastionado-de-redes-y-sistemas","practica","pki"],"status":"En progreso"}}
---


# 📝 Práctica: Validez y autenticidad de certificados web

> [!note] Objetivos de la Práctica
> - Inspeccionar el certificado X.509 de un servicio web e identificar sus campos.
> - Comprobar la cadena de confianza y la validez de un certificado.
> - Comparar certificados válidos e inválidos por diferentes motivos.

## 📐 Recordatorio Teórico

- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/3. Infraestructuras de clave pública (PKI)#3.6 El certificado digital X.509\|Estructura X.509]]
- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/3. Infraestructuras de clave pública (PKI)#3.8 Validación de certificados. TLS y HTTPS\|Validación de certificados]]
- [[Bastionado de redes y sistemas/Unidad 3 Administración de credenciales de acceso/Apuntes/3. Infraestructuras de clave pública (PKI)#3.8.2 Certificados válidos e inválidos\|Certificados válidos e inválidos]]

## 🛠️ Enunciado

### Paso 1. Inspección desde el navegador

1. Accede a un sitio con HTTPS (p. ej., la sede electrónica de un organismo público).
2. Comprueba que la URL empieza por `https` y pulsa el **candado** para ver el certificado.
3. Anota: **sujeto (CN)**, **emisor**, **fechas de validez**, **algoritmo de firma**, tamaño de la **clave pública** y **cadena de certificación** hasta la CA raíz.
4. Localiza la CA raíz en el almacén de certificados del navegador.

### Paso 2. Análisis con SSL Labs

Analiza el mismo dominio en <https://www.ssllabs.com/ssltest/index.html> y anota la calificación, las versiones de TLS admitidas y los avisos.

### Paso 3. Inspección desde la línea de comandos (conocimiento general, verificar)

```bash
openssl s_client -connect www.ejemplo.es:443 -servername www.ejemplo.es -showcerts </dev/null
echo | openssl s_client -connect www.ejemplo.es:443 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

### Paso 4. Comparar certificados inválidos

Visita los subdominios de prueba de <https://badssl.com> (actualizado, fuente: [badssl.com](https://badssl.com)), por ejemplo `expired`, `wrong.host`, `self-signed`, `untrusted-root` y `revoked`. Completa la tabla:

| Sitio | Mensaje del navegador | Campo o mecanismo que falla | Motivo |
| :-: | --- | --- | --- |
| expired | | | |
| wrong.host | | | |
| self-signed | | | |
| untrusted-root | | | |
| revoked | | | |

### Paso 5. Cuestiones

1. Explica cómo verifica el navegador la firma de la CA sobre el certificado.
2. ¿Por qué un certificado autofirmado cifra la comunicación pero no garantiza la autenticidad?
3. ¿Qué papel tienen las CRL y la autoridad de validación?

> [!important] Criterios de evaluación asociados
> RA3 c) Se ha comprobado la validez y la autenticidad de un certificado digital de un servicio web.
> RA3 d) Se han comparado certificados digitales válidos e inválidos por diferentes motivos.

> [!quote]- Fuentes
> - `00-Unidad 1 - Mecanismos de autenticación y gestión de credenciales.odt` (X.509, validación, SSL Labs)
> - `Bloque 1-Diseño de Planes de Securización.pdf` (comprobación del certificado en el navegador)
> - `1-PKI.pdf` (TLS)
