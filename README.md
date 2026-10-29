# Clinical AI Governance & Risk Audit Framework

Marco de auditoría técnica y gobernanza de Inteligencia Artificial aplicado a un agente conversacional en producción dentro del sector salud. El análisis evalúa flujos de datos sensibles, riesgos operacionales, alineación bioética y cumplimiento normativo bajo el marco de la legislación chilena (Ley N° 19.628 y estándares de protección de datos).

---

## 1. Alcance y Topología del Sistema Auditado

* **Propósito del Sistema:** Agente conversacional inteligente orientado a la adquisición, triaje preliminar y coordinación de horas médicas ambulatorias a través de canales de mensajería (WhatsApp Business).
* **Flujos de Información Procesados:**
  * Datos personales comunes (identificación de contacto).
  * Datos identificatorios estatales (Cédula de Identidad / RUN-RUT).
  * Datos sensibles de salud (sintomatología preliminar declarada, previsión médica, confirmación de procedimientos).
* **Cadena de Suministro Tecnológica Evaluada:**
  * **Capa 1 (Infraestructura Base):** Plataforma CRM SaaS alojada en centros de datos internacionales (EE. UU.).
  * **Capa 2 (Intermediario Comercial):** Agencia local operando bajo modelo de marca blanca (*White-Label*).
  * **Capa 3 (Inferencia Generativa):** APIs externas de modelos de lenguaje en la nube.
  * **Capa 4 (Operador Clínico):** Prestador de salud sujeto al deber de secreto médico y custodia de fichas clínicas.

---

## 2. Matriz Cuantitativa de Riesgos ($P \times I$)

Escala de evaluación: Probabilidad ($P \in [1, 5]$) $\times$ Impacto ($I \in [1, 5]$).

| ID | Tipología | Riesgo Identificado | P | I | Score | Zona | Estrategia |
|:---|:---|:---|:---:|:---:|:---:|:---:|:---|
| **R01** | Regulatorio | Infracción gravísima por expatriación transfronteriza y almacenamiento de datos sensibles de salud (RUT, síntomas) en servidores extranjeros sin consentimiento explícito. | 4 | 5 | **20** | **Crítico** | **EVITAR** |
| **R02** | Financiero | Incapacidad de absorción de costos por discontinuidad operativa y dependencia de soporte externo de emergencia. | 2 | 3 | **6** | Bajo | Mitigar |
| **R03** | Reputacional | Quiebre de secreto médico y confidencialidad ante eventual exposición de historiales en infraestructura pública de terceros. | 3 | 4 | **12** | **Alto** | Mitigar |
| **R04** | Operacional | *Data Poisoning* / Envenenamiento de conocimiento por permisos horizontales excesivos en fuentes de datos (Google Docs/Bases vectoriales). | 3 | 3 | **9** | Medio | Mitigar |
| **R05** | Operacional | *Vendor Lock-in* y dependencia crítica ante intermediario opaco con imposibilidad técnica de migración autónoma. | 2 | 3 | **6** | Bajo | Transferir |
| **R06** | Ético / Sesgo | Discriminación algorítmica de pacientes vulnerables por uso de métricas comerciales de priorización (*Hot Leads*) en lugar de triaje clínico. | 4 | 3 | **12** | **Alto** | Mitigar |
| **R07** | Operacional | Indisponibilidad de servicio por caída de APIs externas durante fines de semana, con impacto en atención no hábil. | 2 | 3 | **6** | Bajo | Mitigar |
| **R08** | Ciberseguridad | Acceso residual de colaboradores desvinculados por debilidades en políticas de *offboarding* y falta de rotación de credenciales maestras. | 2 | 3 | **6** | Bajo | Mitigar |

---

## 3. Plan de Mitigación y Gobernanza Técnica

1. **Desacoplamiento y Retirada Estratégica (Tratamiento R01):**
   * Transición fuera de arquitecturas SaaS extranjeras no conformes con la soberanía de datos de salud.
   * Implementación de pasarelas de seguridad perimetral (*Edge Sanitization*) locales para interceptar y anonimizar datos identificatorios antes de cualquier interacción digital.
2. **Eliminación de Sesgos Algorítmicos Comerciales (Tratamiento R06):**
   * Desactivación de puntuaciones automatizadas de prospectos en flujos médicos.
   * Implementación de una política de atención basada en orden de llegada y triaje protocolizado por personal de salud.
3. **Control de Acceso Granular y Sanitización de Conocimiento (Tratamiento R04 / R08):**
   * Segregación vertical de permisos: operadores de mesón con acceso exclusivo de lectura en bandejas de entrada.
   * Centralización de edición de bases de conocimiento en una única credencial maestra con rotación obligatoria cada 60 días.
   * Checklist automatizado de revocación de accesos (*offboarding*) con tiempo máximo de ejecución de 2 horas tras desvinculaciones.
