
# Informe de Auditoría de Riesgos: Agente Conversacional Clínico de IA

## INTRODUCCIÓN

El avance exponencial de la Inteligencia Artificial (IA) ha permitido a las instituciones de salud optimizar sus procesos de atención y captación de pacientes mediante herramientas conversacionales automatizadas. Sin embargo, la implementación de estos sistemas sociotécnicos no está exenta de riesgos críticos que van más allá del ámbito puramente tecnológico, impactando directamente la privacidad de los datos, la ética asistencial y el cumplimiento del marco regulatorio.

El presente informe expone una auditoría integral de gobernanza aplicada sobre el agente conversacional inteligente "System-H", un chatbot de IA actualmente en fase de despliegue operativo utilizado para el seguimiento y agendamiento autónomo de citas médicas. A través de un análisis multicapa, se examina el flujo de datos sensibles recopilados (como sintomatologías declaradas y RUTs) y se evalúa la compleja relación contractual con la cadena de suministro, la cual delega la infraestructura física en servidores extranjeros a través de una fachada de marca blanca. El objetivo de este documento es identificar, ponderar y estructurar estrategias de mitigación para los ocho riesgos principales que amenazan la continuidad operativa, la reputación de la clínica y la seguridad de sus pacientes.

---

## Sistema bajo Análisis

### 1. ¿Qué hace el sistema?
El sistema bajo análisis consiste en un agente conversacional inteligente (Chatbot de IA) diseñado para la automatización del flujo de adquisición, follow-up (seguimiento) y agendamiento automatizado de citas para una organización prestadora de servicios de salud. El sistema interactúa directamente con los prospectos (leads) a través de canales de mensajería instantánea para responder consultas médicas frecuentes basados en una base de conocimientos suministrada por el cliente, calificar el perfil del paciente y coordinar horas disponibles en la agenda de la sucursal de manera autónoma.

### 2. ¿Qué datos usa?
El sistema procesa flujos de información altamente sensibles y regulados que incluyen de manera secuencial:
* **Datos Personales Comunes:** Nombres, apellidos, números telefónicos y datos de contacto de redes sociales (Meta Ads, WhatsApp API).
* **Datos Identificatorios del Estado Chileno:** Cédula de Identidad / RUN-RUT.
* **Datos de Categoría Especial (Datos de Salud Sensibles):** Sintomatología declarada por el paciente, previsión de salud (Fonasa/Isapre), antecedentes o motivos de consulta médica y confirmación de asistencia a procedimientos clínicos.
* **Datos de Operación e Infraestructura:** Prompts maestros de la organización, vectores semánticos almacenados (Knowledge Base Vector Store) e historiales persistentes de logs de conversación y auditoría técnica.

### 3. ¿Quiénes son los usuarios y afectados?
* **Usuarios Activos (Operadores):** Personal administrativo y analistas de resolución de la organización de salud encargados de auditar, validar mediante esquemas Human-in-the-Loop o intervenir de forma manual los flujos conversacionales.
* **Afectados (Sujetos de Datos):** Los pacientes y clientes de la organización que interactúan con el agente automatizado entregando información médica privada bajo el supuesto de confidencialidad clínica tradicional.
* **Administradores de Terceros (Proveedores):** Personal técnico del intermediario comercial con permisos maestros de acceso para supervisar y aprovisionar la subcuenta.

### 4. ¿En qué fase del ciclo de vida está el sistema?
El sistema se encuentra en **Fase de Despliegue Operativo (Producción/Monitoreo)**. Ya está integrado con las APIs de producción de Meta, WhatsApp y las bases de datos de contactos del CRM central. El enfoque de auditoría actual se centra en la gobernanza post-despliegue, mitigación de riesgos de ciberseguridad, control forense de accesos y cumplimiento normativo contractual.

### 5. ¿Quién lo desarrolló y quién lo opera?
Existe un modelo desacoplado de cadena de suministro (SaaS Multicapa) que es el núcleo del riesgo analizado por esta auditoría:
* **Desarrollador de la Infraestructura y CRM Base:** Plataforma tecnológica SaaS extranjera que provee las bases de datos, los motores de workflows y aloja físicamente el software en nubes de EE. UU. (AWS/Google Cloud).
* **Proveedor Intermediario (Fachada Comercial):** Agencia local que comercializa la plataforma en Chile bajo un modelo de Marca Blanca (White Label) bajo la denominación comercial auditada, encargándose de la reventa del servicio.
* **Operador Final:** La organización o sucursal médica auditada, que asume por completo la responsabilidad de alimentar la base de conocimientos, configurar las automatizaciones y custodiar las claves de acceso de sus empleados frente a la legislación local.

---

## DIAGRAMA DE FLUJO DE DATOS DEL SISTEMA

![Diagrama de Flujo de Datos del Sistema](assets/diagrama-flujo.jpg)

*Diagrama que ilustra la expatriación transfronteriza de datos de salud (RUT, Síntomas) de pacientes hacia servidores extranjeros, representando una infracción regulatoria de máxima severidad.*

---

## MATRIZ DE RIESGO

| ID | Tipología | Riesgo Identificado | Fase / Dominio | P (1-5) | I (1-5) | Score (P×I) | Zona | Estrategia |
|:---|:---|:---|:---|:---:|:---:|:---:|:---:|:---|
| **R01** | Regulatorio | Infracción gravísima a Ley 19.628 por expatriación ilegal y almacenamiento de datos sensibles (RUT, Síntomas) en servidores extranjeros (SaaS/EE. UU.) sin consentimiento explícito del paciente. | Datos | 4 | 5 | **20** | **Crítico** | **EVITAR** |
| **R02** | Financiero | Incapacidad de absorción de costos por discontinuidad operativa y dependencia de soporte externo especializado. | Operación | 2 | 3 | **6** | BAJO | MITIGAR |
| **R03** | Reputacional | Quiebre irreversible de confianza por alucinaciones críticas o exposición pública de la expatriación de datos, vulnerando el secreto médico. | Desarrollo | 3 | 4 | **12** | **Alto** | MITIGAR |
| **R04** | Operacional | Data Poisoning / Envenenamiento de Conocimiento. Alteración de la fuente de datos por exceso de permisos horizontales, inyectando información falsa que el bot adopta como verdad. | Diseño | 3 | 3 | **9** | Medio | MITIGAR |
| **R05** | Operacional | Vendor Lock-in / Dependencia Crítica. Imposibilidad técnica y legal de migrar el bot, su lógica y los historiales médicos si el intermediario o el proveedor SaaS interrumpen su servicio. | Retiro | 2 | 3 | **6** | Bajo | TRANSFERIR |
| **R06** | Ético / Sesgo | Sesgos de Evaluación. Exclusión automatizada de pacientes vulnerables o críticos por medir "Hot Leads" con métricas comerciales en lugar de criterios clínicos de urgencia. | Desarrollo | 4 | 3 | **12** | **Alto** | MITIGAR |
| **R07** | Operacional | Indisponibilidad Operativa. Caída del bot (falla de APIs externas) los fines de semana, forzando la atención manual o pagando horas extra, con impacto reputacional. | Operación | 2 | 3 | **6** | Bajo | MITIGAR |
| **R08** | Ciberseguridad | Vulnerabilidad por Falta de Gobernanza. Acceso de ex-empleados a la base de conocimiento o plataforma por no rotar credenciales o procesos de baja débiles. | Operación | 2 | 3 | **6** | Bajo | MITIGAR |

---

## ANÁLISIS DETALLADO DE RIESGO Y MITIGACIÓN

### ID: R01
* **Riesgo Identificado:** Infracción gravísima y explícita a la legislación chilena de protección de datos personales (Ley 19.628 y su reforma de cumplimiento). El sistema opera en una "Zona Roja" legal al realizar la transferencia internacional y el almacenamiento de datos sensibles de salud (síntomas, diagnósticos, previsión, RUT y direcciones) en servidores ubicados en el extranjero (EE. UU.) sin contar con el consentimiento explícito, previo e informado de los titulares.
* **Impacto Potencial:** Aplicación de multas por parte de la autoridad fiscalizadora, las cuales, bajo el nuevo estándar regulatorio chileno alineado con normativas internacionales (como el modelo de multas del EU AI Act), pueden alcanzar sumas millonarias equivalentes a porcentajes significativos de los ingresos globales o anuales de la organización. Adicionalmente, el impacto incluye la orden regulatoria de suspensión inmediata de las operaciones del sistema de IA y el bloqueo de sus bases de datos, paralizando por completo el canal digital de adquisición de pacientes de la clínica.
* **Responsable:** Director de la Clínica.
* **Justificación del Scoring:** El score de 20 sitúa este riesgo en el nivel de máxima criticidad (Zona Roja). Se justifica porque, al operar el bot solicitando datos de salud de forma abierta y automática en WhatsApp para enviarlos fuera del país sin un consentimiento legal adecuado, la probabilidad de fiscalización o sanción es Alta (4). Esto se cruza con un Impacto Catastrófico (5), ya que la ley faculta a la autoridad a ordenar la suspensión inmediata del sistema, lo que congelaría por completo el motor digital de adquisición de pacientes de la clínica.
* **Implementación de la Estrategia (EVITAR):** Dado que la arquitectura tecnológica del proveedor subyacente (SaaS basado en EE. UU.) presenta una imposibilidad técnica estructural para garantizar el almacenamiento local y el cumplimiento de la Ley 19.628, la clínica determina que el nivel de exposición jurídica es inaceptable y ejecutará una retirada estratégica del sistema mediante el desacoplamiento de la captura y la adopción de arquitecturas locales.

---

### ID: R02
* **Riesgo Identificado:** Incapacidad de absorción de costos por discontinuidad operativa y dependencia de soporte externo especializado. Al no tener la clínica las competencias técnicas internas, cualquier fallo o quiebre con el proveedor actual obliga a una contratación de emergencia de servicios de ingeniería externos para mantener el canal de agendamiento activo.
* **Impacto Potencial:** Gasto imprevisto por concepto de "remediación técnica" y migración forzada de arquitectura (estimado en un pago único de 3 a 4 veces el costo mensual actual). Adicionalmente, un aumento del gasto operativo mensual al tener que asumir el costo de licencias directas más el pago de un servicio de soporte/mantención técnica dedicada para garantizar la continuidad del bot.
* **Responsable:** Jefe del Área Comercial.
* **Justificación del Scoring:** El score de 6 refleja que, si bien las consecuencias económicas de una migración forzada o la contratación de ingenieros externos de emergencia impactarían el presupuesto del área, la probabilidad de ocurrencia se mantiene baja (2) dado que el sistema actualmente se encuentra integrado y operativo en producción. Esto sitúa el riesgo en la zona verde (Bajo), justificando que no requiere un desembolso inmediato de capital, sino un plan preventivo de eficiencia de costos liderado por el área comercial.
* **Implementación de la Estrategia (MITIGAR):** Para mitigar la dependencia económica de soporte externo avanzado y resguardar la continuidad del canal de agendamiento sin incurrir en gastos imprevistos desmedidos, el Jefe del Área Comercial ejecutará las siguientes acciones:
  1. **Evaluación de Proveedores Locales Certificados:** Se iniciará un proceso de levantamiento y cotización de proveedores o agencias de desarrollo de software locales en Chile. El objetivo es identificar alternativas tecnológicas que ofrezcan las mismas funciones de automatización de citas, pero bajo contratos locales con soporte técnico incluido dentro de rangos presupuestarios estables y predecibles, evitando cobros imprevistos por emergencias.
  2. **Plan de Contingencia Basado en Herramientas Funcionales de Bajo Costo:** En caso de una discontinuidad del proveedor actual y ante la imposibilidad financiera de costear ingenieros especializados de emergencia, se autoriza la migración inmediata a herramientas nativas de mensajería comercial estructurada (sin Inteligencia Artificial). Este flujo simplificado operará mediante respuestas automáticas estructuradas (menús de botones) que se limitarán a capturar datos básicos como el nombre y la preferencia horaria del paciente, asegurando la continuidad de la captación de clientes de forma económica y funcional mientras se estabiliza la infraestructura.

---

### ID: R03
* **Riesgo Identificado:** Quiebre de la confianza y el secreto médico debido a la exposición pública de la expatriación ilegal de datos de salud. El riesgo radica en que se haga público que la clínica transfiere y almacena historiales médicos, RUTs, direcciones y síntomas de pacientes en servidores de corporaciones extranjeras (EE. UU.) sin el consentimiento explícito exigido legalmente, vulnerando el principio fundamental de confidencialidad clínica en el que se basa la relación médico-paciente.
* **Impacto Potencial:** Destrucción del activo más valioso de la clínica: su reputación y la confianza del público. El impacto se traducirá en la pérdida masiva de pacientes, quienes migrarán hacia competidores al no estar dispuestos a arriesgar la privacidad de sus datos de salud. Asimismo, generará un daño de marca severo e irreversible en la comunidad médica y el mercado local, haciendo inviable la continuidad comercial del modelo de adquisición.
* **Responsable:** Encargado de Experiencia al Paciente / Director Médico.
* **Justificación del Scoring:** El score de 12 se fundamenta en que la pérdida de reputación detallada en el impacto potencial es una amenaza latente de severidad alta (4) para una institución de salud. Al operar actualmente el bot de manera abierta solicitando datos personales en canales públicos de mensajería, la probabilidad de que un usuario cuestione la privacidad o se exponga este flujo transfronterizo es media (3). El puntaje en zona alta confirma que el resguardo del secreto médico no puede depender del criterio autónomo de la máquina.
* **Implementación de la Estrategia (MITIGAR):** Para mitigar la exposición de los pacientes y proteger el secreto médico, la clínica ejecutará un rediseño del alcance conversacional de la IA y un desacoplamiento del almacenamiento clínico a través de las siguientes acciones:
  1. **Minimización de Datos en el Frontend del Bot:** Se modificará el prompt maestro y la lógica del agente en mensajería para prohibir estrictamente la solicitud del RUT, dirección, previsión o cualquier descripción explícita de sintomatología médica. El bot se limitará exclusivamente a interactuar utilizando el nombre de pila del prospecto para coordinar la disponibilidad básica de la agenda administrativa.
  2. **Protocolo de Registro Clínico Interno Seguro:** La recopilación de antecedentes médicos y la apertura de la ficha clínica del paciente quedan totalmente vedadas para la inteligencia artificial. Estos datos sensibles solo se registrarán una vez que el flujo pase a una atención telefónica/presencial conducida por personal de la clínica, o mediante una integración directa del CRM con un software de gestión médica local con servidores certificados en Chile, garantizando que los registros de salud nunca abandonen las fronteras del país.

---

### ID: R04
* **Riesgo Identificado:** Data Poisoning debido a niveles de acceso excesivamente amplios en la plataforma. El riesgo se genera porque los permisos de seguridad horizontales dentro de la configuración del sistema son demasiado extensos. Esto permite que usuarios con tareas rutinarias tengan acceso automático a la base de datos y a la edición de la fuente de conocimiento (Knowledge Base). Al comprometerse una credencial rutinaria, un atacante puede alterar o inyectar información falsa directamente en el documento que alimenta las respuestas del bot.
* **Impacto Potencial:** El atacante puede modificar las ofertas y colocar precios diferentes a los originales. Al interactuar con el público por mensajería, la clínica se vería en la obligación de cumplir y respetar esos valores erróneos frente a los usuarios, asumiendo la pérdida económica.
* **Responsable:** Director de la Clínica.
* **Justificación del Scoring:** El score de este riesgo se pondera en la matriz debido a la vulnerabilidad estructural que provoca la falta de control de accesos sobre la base de conocimiento. Al no contar con barreras de seguridad intermedias, la probabilidad de que un error operativo o una filtración de credenciales rutinarias altere la fuente de información es latente. Esto se cruza con un impacto directo sobre el patrimonio de la clínica, ya que cualquier distorsión de precios comunicada por el bot obliga legalmente a la institución a respetar dichos valores comerciales ante los pacientes, validando la necesidad de aplicar controles estrictos de mitigación.
* **Implementación de la Estrategia (MITIGAR):** Para mitigar la superficie de exposición ante alteraciones no autorizadas en la base de conocimientos (Knowledge Base), el Director de la Clínica visará e implementará las siguientes directrices de control de acceso:
  1. **Restricción del Rol Maestro de Configuración:** Se centralizará el acceso global de edición de la plataforma en una única credencial de nivel administrador maestro. Ningún usuario clínico o administrativo de nivel operativo poseerá facultades para modificar de forma directa las fuentes de datos, los documentos de texto o los prompts base que alimentan las respuestas del agente.
  2. **Segregación de Permisos Verticales y Bloqueo de Fuentes:** Se exigirá la reconfiguración de los perfiles de usuario en el CRM para transitar desde un modelo de accesos horizontales masivos hacia un esquema de permisos granulares por herramienta. El personal de atención y resolución de leads solo tendrá acceso de lectura y respuesta en la bandeja de entrada, quedando técnica y estrictamente bloqueado de cualquier capacidad de edición sobre los archivos de conocimiento (como documentos compartidos o almacenes vectoriales), eliminando la posibilidad de envenenamiento interno de datos de manera rutinaria.

---

### ID: R05
* **Riesgo Identificado:** Dependencia crítica de la cadena de suministro de IA de terceros (Vendor Lock-in) y opacidad del intermediario. La clínica opera un sistema de IA cuya continuidad, APIs, infraestructura de servidores y base de datos central dependen 100% de un proveedor tecnológico extranjero, administrado a través de una fachada comercial local (Marca Blanca) que diluye la visibilidad y el control directo sobre la plataforma.
* **Impacto Potencial:** Interrupción completa del servicio de adquisición y agendamiento de pacientes si el proveedor SaaS modifica de forma unilateral sus APIs, políticas de uso, esquemas de precios o si experimenta caídas en sus servidores de EE. UU.. Adicionalmente, imposibilidad técnica de la clínica para migrar de forma autónoma el agente o los historiales médicos de los pacientes a otra infraestructura local en caso de rescisión del contrato.
* **Responsable:** Director de la Clínica.
* **Justificación del Scoring:** El score de 6 refleja que, si bien el impacto potencial describe una vulnerabilidad severa por la falta de autonomía para migrar datos de pacientes frente a un quiebre del intermediario, la probabilidad se mantiene en un nivel bajo (2) debido al respaldo tecnológico de escala global que tienen los servidores base de nubes públicas internacionales. El número justifica que el riesgo pertenece a la zona verde de la matriz: no es una amenaza inminente en la operación diaria, pero la opacidad de la marca blanca prohíbe que sea un punto ciego para la gobernanza de la clínica.
* **Implementación de la Estrategia (TRANSFERIR):** Para transferir eficazmente este riesgo, la clínica ejecutará una estrategia de exigencia contractual y de transparencia dividida en dos etapas obligatorias con el proveedor local:
  1. **Auditoría de Transparencia Tecnológica:** Se exigirá formalmente a la agencia intermediaria la entrega de una declaración técnica exhaustiva y un mapa de la arquitectura del sistema. Esto debe incluir la aclaración explícita de los subprocesadores involucrados, las ubicaciones físicas de los servidores de respaldo y las políticas de retención de datos que aplican estos terceros sobre la información de la clínica.
  2. **Cláusula de Indemnidad y Responsabilidad Civil Trasladada:** Se modificará el acuerdo de nivel de servicio (SLA) para estipular contractualmente que el intermediario local asume la responsabilidad civil, económica y administrativa ante la clínica por cualquier falla, discontinuidad, cambio de precios unilateral o quiebre operativo derivado de sus subproveedores extranjeros. Con esto, la carga financiera de una eventual parálisis del sistema queda legalmente transferida a la agencia prestadora del servicio.

---

### ID: R06
* **Riesgo Identificado:** Sesgos de evaluación. El riesgo radica en que el sistema utiliza la métrica estandarizada de "Hot Leads" del proveedor original, la cual fue diseñada para entornos comerciales generales (como la venta masiva de productos o servicios transaccionales) y no para el sector de la medicina. Al carecer de un enfoque clínico y de supervisión humana, el algoritmo clasifica la prioridad de los usuarios bajo parámetros netamente comerciales de conversión rápida, deshumanizando la interacción y siendo incapaz de ponderar la urgencia real o el estado emocional de un paciente.
* **Impacto Potencial:** Exclusión y despriorización automatizada de pacientes vulnerables o con necesidades de salud críticas. Aquellas personas que requieran atención médica de manera urgente o que se encuentren en un estado psicológico alterado o de vulnerabilidad emocional, difícilmente redactarán sus requerimientos bajo la estructura "óptima" que la máquina exige para ser etiquetados como prioritarios. Como consecuencia directa, el sistema relega a un segundo plano a clientes potenciales con necesidades de salud genuinas, retrasando la respuesta del equipo médico y afectando negativamente la calidad asistencial y el acceso equitativo a la salud.
* **Responsable:** Jefe del Área Comercial.
* **Justificación del Scoring:** El score de 6 ubica este riesgo en la zona verde (Bajo) de la matriz. Esto se justifica porque la probabilidad de que el bot discrimine de forma activa es moderada (3), ya que su configuración actual está explícitamente orientada a la conversión de ventas. Sin embargo, el impacto se mantiene controlado en un nivel bajo/moderado (2) en la matriz ejecutiva debido a que, por diseño previo, el bot no tiene la facultad autónoma de bloquear la agenda de forma definitiva, sirviendo principalmente como un embudo de priorización. No obstante, por el factor ético y asistencial en áreas sensibles como la salud mental, el área a cargo debe tomar el control de la métrica.
* **Implementación de la Estrategia (MITIGAR):** Para mitigar el sesgo en la calificación de prospectos y asegurar que la automatización no discrimine arbitrariamente a los usuarios, el Jefe del Área Comercial ejecutará las siguientes acciones directamente con el proveedor técnico:
  1. **Auditoría de Criterios de Calificación Ocultos:** Dado que se ha constatado que el sistema prioriza los leads bajo una visión estrictamente comercial, pero se desconocen las métricas, ponderaciones o palabras clave exactas en que se basa para dicha clasificación, se exigirá formalmente al proveedor la apertura técnica de esta lógica para auditar qué parámetros gatillan el sesgo.
  2. **Eliminación de la Distinción Automatizada por la IA:** Se instruirá contractualmente al proveedor la eliminación definitiva de dicha segmentación automatizada basada en criterios comerciales dentro del flujo del bot. Todas las solicitudes de contacto y agendamiento entrantes deberán ingresar a la bandeja de atención humana bajo un principio de estricta igualdad y orden de llegada, impidiendo que la inteligencia artificial decida de manera autónoma el nivel de prioridad de un paciente.

---

### ID: R07
* **Riesgo Identificado:** Indisponibilidad Operativa. Caída del bot (falla de APIs externas de LLM/CRM) los fines de semana, forzando la atención manual o pagando horas extra, con impacto reputacional.
* **Impacto Potencial:** Pérdida de potenciales pacientes que intentan agendar fuera del horario hábil y no reciben respuesta, o aumento imprevisto del gasto operacional si se debe activar personal de contingencia en días festivos o fines de semana para responder de forma manual los mensajes acumulados, pagando recargos por horas extraordinarias.
* **Responsable:** Director de la Clínica.
* **Justificación del Scoring:** El score de 6 mantiene este riesgo en la zona verde (Bajo). Se justifica porque la probabilidad de una caída total y simultánea de las infraestructuras cloud subyacentes durante el fin de semana es estadísticamente baja (2). Sin embargo, el impacto se evalúa como moderado (3) debido al factor asistencial: al atender la clínica especialidades de salud de alta sensibilidad, la desconexión o el silencio del bot ante un paciente en crisis deteriora la confianza institucional y desprotege al usuario en horarios no hábiles.
* **Implementación de la Estrategia (MITIGAR):** Para mitigar la interrupción del servicio y salvaguardar la atención de los usuarios durante los fines de semana sin incurrir en sobrecostos de personal, el Director de la Clínica coordinará las siguientes acciones de contingencia:
  1. **Configuración de Canal de Soporte Pasivo Automatizado (Failover):** Se programará una regla de contingencia en el enrutador de mensajería. En caso de que el servidor de la IA sufra una caída o no responda en un lapso de 60 segundos, el sistema activará automáticamente un mensaje estático de respaldo que informará de manera clara el estado de mantenimiento y desplegará números telefónicos de asistencia pública o de urgencia externa para pacientes que requieran contención inmediata.
  2. **Monitoreo y Protocolo de Turnos de Alerta Ética:** Se establecerá un sistema de alertas automatizado por correo o SMS dirigido al equipo de turno de la clínica en caso de desconexión del sistema. Si la indisponibilidad supera las 4 horas durante el fin de semana, se activará un protocolo de revisión manual pasiva por parte del personal de guardia para procesar exclusivamente los mensajes de pacientes que manifiesten requerimientos críticos, postergando la agenda comercial para el día lunes.

---

### ID: R08
* **Riesgo Identificado:** Vulnerabilidad por Falta de Gobernanza. Acceso de ex-empleados a la base de conocimiento o plataforma por no rotar credenciales o procesos de baja débiles.
* **Impacto Potencial:** Fuga de información confidencial de pacientes, manipulación maliciosa de la base de datos o alteración del comportamiento del bot por parte de personal desvinculado que aún conserva credenciales activas, exponiendo a la clínica a brechas de seguridad, extorsiones o espionaje comercial de la competencia.
* **Responsable:** Director de la Clínica.
* **Justificación del Scoring:** El score de 6 sitúa este riesgo en la zona verde (Bajo) de la matriz de gobernanza. La probabilidad se pondera como baja (2) debido a que la rotación de personal administrativo en una clínica local suele ser acotada y controlada. No obstante, el impacto se califica como moderado (3) dado que cualquier acceso no autorizado a datos de pacientes o a la configuración de la IA por parte de un tercero ex-colaborador constituye un quiebre de seguridad que compromete la confidencialidad de la empresa.
* **Implementación de la Estrategia (MITIGAR):** Para mitigar la superficie de exposición ante accesos históricos no autorizados, el Director de la Clínica establecerá una política estricta de gobernanza de identidades mediante las siguientes directrices:
  1. **Protocolo Mandatario de Desvinculación y Offboarding:** Se creará un checklist obligatorio de salida aplicable de forma inmediata ante la renuncia o despido de cualquier trabajador administrativo o de salud. Este protocolo exige la revocación total de accesos y la eliminación definitiva de sus perfiles de usuario en la plataforma CRM, suites comerciales y cualquier planilla o documento que sirva como fuente de conocimiento del bot en un plazo máximo de 2 horas desde su salida de la clínica.
  2. **Auditoría Bimestral y Rotación de Credenciales Maestras:** El Director supervisará de manera directa una revisión bimestral del registro de usuarios activos en la plataforma tecnológica. Adicionalmente, se implementará la política de actualización y rotación obligatoria cada 60 días de la contraseña de la credencial administradora maestra, asegurando que ningún ex-empleado pueda reingresar al sistema a través de accesos compartidos o contraseñas guardadas en dispositivos externos.

---

## CONCLUSIÓN

La auditoría realizada al sistema conversacional evidencia que, si bien la automatización ofrece ventajas comerciales evidentes en la adquisición de prospectos, genera una exposición de riesgo multidimensional que la clínica no puede ignorar. Los hallazgos demuestran que mantener la configuración actual sitúa a la institución en una "Zona Roja" regulatoria debido a la expatriación no consentida de datos de salud sensibles hacia servidores en el extranjero, vulnerando gravemente la legislación nacional (Ley 19.628) y el principio fundamental del secreto médico. Asimismo, los sesgos comerciales heredados del diseño original del software amenazan la equidad en el acceso a la salud, despriorizando potencialmente a pacientes en situaciones de vulnerabilidad asistencial o emocional.

En conclusión, la viabilidad a largo plazo de la Inteligencia Artificial en contextos clínicos depende estrictamente de una gobernanza institucional robusta y de la primacía de los criterios bioéticos sobre las métricas de conversión económica. Las estrategias propuestas en este informe —que abarcan desde la minimización de datos en el bot y la restricción vertical de accesos, hasta la transferencia contractual de responsabilidades y la planificación de una retirada estratégica del sistema— constituyen una hoja de ruta indispensable. Solo a través de la adopción inmediata de estos controles, liderados de forma transversal por la dirección clínica y el área comercial, será posible salvaguardar el patrimonio de la organización, cumplir con el estándar legal chileno y, por sobre todo, proteger la confianza y la integridad de los pacientes.
