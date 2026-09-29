# Laboratorio 1 — Bitácora de auditoría de la cadena de suministro

- **Autor/a:** ESCRIBE_AQUÍ_TU_NOMBRE_Y_APELLIDOS
- **Repositorio:** https://github.com/TU-USUARIO/reportaudit-lab
- **Sistema operativo y versión de Python usados:**

> Completa cada sección en el momento en que la guía te lo pide, no al final.
> Una bitácora escrita "de memoria" al terminar no sirve como evidencia.

---

## Parte B — Auditoría manual (antes de usar ninguna herramienta)

| # | Función | Línea | Qué sospechas | Dato de entrada (*source*) | Destino peligroso (*sink*) | Impacto para el negocio |
|---|---|---|---|---|---|---|
| 1 | obtener_reportes | app/reporte_auditoria.py | Inyección SQL (SQLi): Concatenación directa del parámetro cliente en la consulta SQL sin parametrizar. | Parámetro cliente de la URL (request.args). | cursor.execute(query) | Acceso no autorizado a datos confidenciales de todos los clientes o filtración completa de la BBDD. |
| 2 | convertir_a_pdf | app/reporte_auditoria.py | Inyección de Comandos (Command Injection): Uso de os.system o subprocess pasando un nombre de archivo sin validar mediante la shell. | Parámetro archivo de la URL (request.args). | Llamada a la shell del sistema (os.system / subprocess). | Ejecución remota de código (RCE) en el servidor Ubuntu; control total de la máquina por un atacante. |
| 3 | cargar_configuracion | app/reporte_auditoria.py | Inseguridad al deserializar YAML: Uso de yaml.load con FullLoader o Loader completo en lugar de SafeLoader. | Archivo de configuración YAML / Entrada de usuario. | yaml.load(...) | Deserialización insegura que permite ejecución arbitraria de código al cargar un YAML malicioso. |
| 4 | hash_password_legacy | app/reporte_auditoria.py | Criptografía débil: Uso de algoritmo hash obsoleto (MD5/SHA1) y sin utilizar salt. | Contraseña en texto plano. | Algoritmo Hash sin salt (hashlib.md5 / sha1) | Contraseñas vulnerables a ataques de diccionario o tablas Rainbow; filtración de credenciales. |
| 5 | Constantes del principio | app/reporte_auditoria.py | Hardcoded Secrets: Claves API, tokens o contraseñas escritas directamente en texto plano en el código fuente. | Código fuente (valores constantes). | Asignación directa a variables en el código. | Filtración de credenciales del sistema en el repositorio de Git; acceso directo a servicios de terceros. |

**Impacto en el negocio:** para cada sospecha, explica en una frase qué
consecuencia tendría para ReportAudit y sus clientes si fuera real (qué datos,
qué sistema o qué credencial quedarían expuestos).

---

## Matriz de detección (se completa a lo largo del laboratorio)

Marca ✓ (lo detectó, anota la regla) o ✗ (no lo detectó) en cada columna cuando
llegues a la parte correspondiente.

| Hallazgo | Manual (B) | SonarQube for IDE sin conexión (D) | SonarQube for IDE en Connected Mode (E) | SonarQube Cloud (F) | CodeQL (F) | Semgrep (G) | Trivy (K) |
|---|---|---|---|---|---|---|---|
| H1 Inyección SQL en `buscar_reportes_cliente` |  |  |  |  |  |  | n/a |
| H2 Inyección de comandos en `convertir_a_pdf` |  |  |  |  |  |  | n/a |
| H3 Deserialización YAML insegura en `cargar_configuracion` |  |  |  |  |  |  | n/a |
| H4 Hash MD5 en `hash_password_legacy` |  |  |  |  |  |  | n/a |
| H5 Clave de API escrita en el código |  |  |  |  |  |  |  |
| H6 Contraseña SMTP escrita en el código |  |  |  |  |  |  |  |

**Conclusión de la matriz** (Parte K): ¿alguna herramienta lo detectó todo? ¿Qué
te dice eso sobre depender de una sola herramienta?

---

## Parte J — SBOM: el iceberg medido

| Dato | Valor |
|---|---|
| Dependencias directas (`requirements.in`) |  |
| Componentes Python en el SBOM |  |
| Otros componentes que aparezcan en el SBOM (si los hay) y de dónde salen |  |
| Formato y versión de especificación del SBOM (`bomFormat`, `specVersion`) |  |

---

## Parte J — Triage de vulnerabilidades de dependencias (Grype)

| Paquete | Versión | ¿Directa o transitiva? (usa `# via`) | CVE / GHSA | Severidad | Corregida en | ¿Explotable en ReportAudit? ¿Por qué? | Decisión |
|---|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |  |

**Comparación con Dependabot** (Parte H): ¿las alertas coinciden con Grype? Explica
cualquier diferencia.

**Documento VEX:** copia `plantillas/reportaudit.openvex.json` a
`docs/evidencias/`, rellénalo, enlázalo aquí y resume en una frase la
justificación.

---

## Parte L y M — Antes y después

| Medida | Antes | Después |
|---|---|---|
| Hallazgos de Semgrep en `app/` |  |  |
| Alertas abiertas de CodeQL (Security → Code scanning) |  |  |
| Vulnerabilidades en SonarQube Cloud (rama main) |  |  |
| Security Hotspots por revisar en SonarQube Cloud |  |  |
| Vulnerabilidades de Grype sobre el SBOM |  |  |
| Alertas abiertas de Dependabot |  |  |

---

## Preguntas de comprobación (Sección 7 de la guía)

1.
2.
3.
4.
5.
6.
7.
8.
9.
10.
11.
12.
