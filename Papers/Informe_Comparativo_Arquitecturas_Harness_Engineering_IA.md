# Análisis Comparativo de Arquitecturas para Harness Engineering con IA

**Autor:** Arquitecto de Software Senior y Analista Técnico  
**Fecha:** 30 de marzo de 2026  
**Versión:** 1.0

---

## Tabla de Contenidos

1. [Resumen Ejecutivo](#1-resumen-ejecutivo)
2. [Tabla Comparativa Principal](#2-tabla-comparativa-principal)
3. [Análisis Detallado por Documento](#3-análisis-detallado-por-documento)
   - 3.1. [Source 1: GAIA Framework (RUA/UA)](#31-source-1-gaia-framework-ruaua)
   - 3.2. [Source 2: OPENDEV (arXiv 2603.05344)](#32-source-2-opendev-arxiv-260305344)
   - 3.3. [Source 3: Spec-Driven Development Paper (arXiv 2602.00180)](#33-source-3-spec-driven-development-paper-arxiv-260200180)
   - 3.4. [Source 4: CURRANTE (arXiv 2601.03878)](#34-source-4-currante-arxiv-260103878)
   - 3.5. [Source 5: DDC - Demand-Driven Context (arXiv 2603.14057)](#35-source-5-ddc---demand-driven-context-arxiv-260314057)
   - 3.6. [Source 6: MCE - Meta Context Engineering (arXiv 2601.21557)](#36-source-6-mce---meta-context-engineering-arxiv-260121557)
   - 3.7. [Source 7: Taylor & Francis (PAYWALL)](#37-source-7-taylor--francis-paywall)
   - 3.8. [Source 8: GABBE Architecture (TechRxiv)](#38-source-8-gabbe-architecture-techrxiv)
4. [Análisis Transversal](#4-análisis-transversal)
5. [Conclusión de Composición](#5-conclusión-de-composición)
6. [Mapa de Dependencias Tecnológicas](#6-mapa-de-dependencias-tecnológicas)
7. [Referencias](#referencias)

---

## 1. Resumen Ejecutivo

Este informe presenta un análisis comparativo exhaustivo de ocho arquitecturas y metodologías para el desarrollo de sistemas de software asistidos por Inteligencia Artificial, con énfasis en **Harness Engineering**, **Spec-Driven Development (SDD)** y **Context Engineering**. Los documentos analizados abarcan desde frameworks prácticos de desarrollo hasta metodologías teóricas de optimización de contexto para Large Language Models (LLMs).

### Hallazgos Principales

**Convergencia metodológica:** Los ocho enfoques convergen en la necesidad de estructurar el desarrollo mediante especificaciones formales, gestión rigurosa del contexto y validación sistemática. GAIA, OPENDEV, SDD Paper y CURRANTE enfatizan el desarrollo guiado por especificaciones y pruebas (TDD/BDD), mientras que DDC y MCE se centran en la ingeniería de contexto como disciplina fundamental [1], [2], [3], [4], [5], [6].

**Espectro de autonomía:** Se observa un espectro desde sistemas altamente supervisados (GAIA con validation gates explícitos, DDC con curación humana obligatoria) hasta sistemas con aspiraciones de autonomía completa (MCE con optimización meta-nivel totalmente agéntica) [7], [8], [9].

**Arquitecturas de orquestación:** Predominan tres patrones: (1) **cadenas secuenciales** con fases bien definidas (GAIA, SDD, CURRANTE), (2) **arquitecturas compuestas** con múltiples agentes especializados (OPENDEV, GABBE), y (3) **optimización bi-nivel** (MCE) [10], [11], [12].

**Gestión de contexto:** Todos los sistemas reconocen la ventana de contexto finita de los LLMs como restricción crítica. Las soluciones incluyen compactación adaptativa (OPENDEV), entidades tipadas versionadas (DDC), arquitectura de memoria dual (OPENDEV), y optimización meta-agéntica (MCE) [13], [14], [15], [16].

**Validación multi-capa:** Los sistemas maduros implementan validación en múltiples niveles: prompt-level guardrails, restricciones de esquema, aprobación en tiempo de ejecución, validación a nivel de herramienta y hooks de ciclo de vida (OPENDEV con 5 capas de seguridad) [17].

### Brechas Identificadas

- **Source 7 (Taylor & Francis):** Acceso restringido por paywall; solo metadata disponible.
- **Métricas de rendimiento:** Varios documentos carecen de benchmarks cuantitativos detallados (MCE, DDC).
- **Integración práctica:** Falta documentación sobre integración entre estos enfoques en sistemas reales de producción.
- **Escalabilidad:** Limitada evidencia empírica sobre comportamiento en proyectos enterprise de gran escala.

---

## 2. Tabla Comparativa Principal

| **Documento** | **Enfoque Específico** | **Problema Abordado** | **Lenguajes** | **Stack de Desarrollo** | **Modelo LLM y Rol** | **Metodología de Validación** | **Gestión de Contexto** | **Métricas de Performance** | **Patrones de Orquestación** | **Nivel Human-in-the-Loop** |
|---------------|------------------------|----------------------|---------------|------------------------|---------------------|------------------------------|------------------------|----------------------------|------------------------------|----------------------------|
| **Source 1: GAIA** | Spec-Driven Development (SDD) + TDD obligatorio | Desalineación especificaciones-código; deuda técnica; falta de trazabilidad | Python, TypeScript | FastAPI, React, Playwright, Pytest, Vitest, Docker, Git, NotebookLM | No especificado (referencia genérica a "agente IA") | Double Loop TDD (E2E + Unit/Integration); Validation Gates explícitos; Checklists de calidad | Rules (Constitución), Skills (biblioteca), Workflows (protocolos); Scope Global/Workspace; @mentions para enlaces referenciales | Límites de archivo (≤12K chars); velocidad/costo de tests; cobertura de tests; códigos de estado HTTP | Cadenas secuenciales (workflows) + agentes colaborativos; Double Loop TDD (ciclos concéntricos) | **Alto:** Validation Gates obligatorios en PRD, feature-descr, planes; revisión humana en cambios críticos; auditoría de seguridad |
| **Source 2: OPENDEV** | Harness Engineering + Context Engineering | Gestión de ventanas de contexto finitas; operaciones destructivas; extensión de capacidades sin sobrecargar prompt | Rust (codebase); Python, TypeScript/JS, Rust, Go, Java, C/C++, C#, Ruby, PHP, Swift, Kotlin, Lua, Elixir, Haskell, Scala, Julia, Perl, Clojure, Elm, Terraform, Bash, Nix, Erlang, AL, Rego, Fortran, OCaml, Markdown, YAML, Dart, Zig, R | Textual (TUI), FastAPI + WebSockets (Web UI), Playwright (Crawl4AI), Ripgrep, AST-grep, DuckDuckGo, LSP servers, Kanban, Git, subprocess/pty | 5 roles de modelo: Action, Thinking, Critique, Vision, Compact; soporte Anthropic/OpenAI | 5 capas de seguridad: Prompt guardrails, Schema restrictions, Runtime approval, Tool validation, Lifecycle hooks; Doom-loop detection; Stale-read tracking | Adaptive Context Compaction (ACC) con 5 etapas; Dual-memory (episodic + working); Per-tool summarization; Dynamic system prompt; Lazy tool discovery; Provider-level caching | Reducción 54% consumo contexto; 88% reducción costo input tokens; sesiones 15-20 → 30-40 turnos; compliance rates mejorados | Compound AI system; Dual-agent (planning/execution); Extended ReAct con 6 fases; Subagent delegation; Dual-path dispatch | **Configurable:** Manual/Semi-Auto/Auto; ask_user tool; present_plan tool; interrupciones Ctrl+C; aprobaciones persistentes |
| **Source 3: SDD Paper** | Spec-Driven Development (paradigma general) | Realidad code-centric; drift de requisitos; diagramas obsoletos; dificultad para IA entender intención sin reverse-engineering | Ruby, Java, JavaScript (Cucumber); Python (Behave); .NET (SpecFlow); C (Simulink) | Cucumber, SpecFlow, Behave, RSpec, JUnit, pytest, OpenAPI/Swagger, GraphQL SDL, Protocol Buffers, AsyncAPI, Pact, Specmatic, GitHub Spec Kit, Amazon Kiro, Tessl, Simulink, SCADE | GPT-4, Claude (referencia genérica como coding agents) | Fase validate: tests automatizados (unit, integration, acceptance); escenarios BDD; revisión requisitos no funcionales; stakeholder acceptance testing | Specs como "super-prompts"; descomposición modular alineada con context windows; plan proporciona contexto crucial a IA | 75% reducción tiempo ciclo integración (caso financiero con OpenAPI) | 4 fases secuenciales: /specify → /plan → /tasks → implementation; ejecución paralela de agentes en tareas no solapadas | **Medio-Alto:** Revisión humana en cada checkpoint; self-spec methods con refinamiento humano; specs requieren revisión cuidadosa |
| **Source 4: CURRANTE** | Spec-Driven Development (estudio empírico) | Comportamiento LLM en procesos estructurados poco comprendido; expresión de intención humana en workflow TDD | Python | CURRANTE (VS Code extension), Visual Studio Code, TOML format, unittest | Qwen3-Coder (especializado en coding); temperatura y endpoint fijos para consistencia | Workflow TDD: Specification → Test Cases (generación + refinamiento) → Code Generation; iteración hasta pasar tests o agotar presupuesto | TOML format para capturar intención; Test Suite como input spec para SDD; refinamiento iterativo de test cases | PassAll, PassRate, TimeToPass, TestCoverage, TestDiversity, IterationsToPass, TestEdits, SuiteRegenerations, AdviceTriggers, Token Counts | Cadena secuencial de 3 fases (Specification → Tests → Function); feedback loop para refinamiento | **Alto:** Definición de especificación humana; refinamiento de test cases (explain/regenerate/delete); aprobación implícita antes de code generation |
| **Source 5: DDC** | Context Engineering (metodología) | Falta de conocimiento de dominio enterprise en LLM agents; conocimiento tribal no documentado | ⚠️ No especificado | ⚠️ No especificado | Referencia genérica a "Large language model agents" y "Modern LLMs" | Validación humana de output por experto de dominio; automated checks (schema, relationships, naming); human review para corrección | Typed entity meta-model; entidades como markdown con YAML frontmatter; versionado; navegación y traversal de relaciones; curación mínima basada en fallos | New entities/cycle (decreciente); Reused entities/cycle (creciente); Correction loops (decreciente); Reuse ratio (creciente); Tiempo curación promedio 30 min/ciclo | Metodología cíclica: Problem → Agent Attempt (Failure) → Identify Gaps → Human Curation → Agent Re-attempt → Correction Loop → Knowledge Graduation; análogo a TDD | **Muy Alto:** Humano como Information Provider, Entity Author, Validator; corrección de fabricaciones; curación manual obligatoria en base DDC |
| **Source 6: MCE** | Context Engineering (framework bi-nivel) | Harnesses agénticos manuales imponen sesgos estructurales; espacio de diseño limitado por intuición | Python | LangChain, LangChain DeepAgents, Claude Agent SDK, DSPy, Hugging Face, OpenRouter, NumPy, Pydantic, Dotenv, Asyncio | DeepSeek-V3.1 (generator default), Qwen3-8B (AEGIS2), MiniMax M2.1 (agentic model), Llama3.3-70B, Gemma3-4B (transferability) | Evaluación en 5 dominios (finance, chemistry, medicine, law, AI safety); métricas pass@1, exact match, micro-F1, F1; stop criteria explícitos | Contexto como archivos y código flexibles; componentes estáticos + operadores dinámicos; "global view" para reestructurar; procesamiento de batches grandes; retrieval functions | 5.6-53.8% mejora relativa (media 16.9%); adaptabilidad contexto 1.5K-86K tokens; 13.6x speedup training; 4.8x menos rollouts; 450 vs 2169 rollouts para 95% accuracy | Bi-level optimization: Meta-level (agentic crossover, (1+1)-ES) + Base-level (ejecución de skills, optimización de contexto); iterativo con fases de skill evolution, context optimization, evaluation | **Muy Bajo:** Optimización totalmente agéntica; transición de workflows manuales a sistemas meta- y self-learning; sin intervención humana explícita en loop de optimización |
| **Source 7: Taylor & Francis** | ⚠️ **BRECHA DE INFORMACIÓN - ACCESO RESTRINGIDO** | ⚠️ Solo metadata disponible (paywall) | ⚠️ No disponible | ⚠️ No disponible | ⚠️ No disponible | ⚠️ No disponible | ⚠️ No disponible | ⚠️ No disponible | ⚠️ No disponible | ⚠️ No disponible |
| **Source 8: GABBE** | Harness Engineering (arquitectura neurocognitiva) | Context decay, no-determinismo, asimetría de costos en MAS; limitaciones de orquestadores flat-topology | ⚠️ No especificado en abstract | ⚠️ No especificado en abstract | ⚠️ No especificado en abstract (referencia a LLM routing cost-effective) | ⚠️ No especificado en abstract | 4-layer memory hierarchy; LLM routing cost-effective | ⚠️ No especificado en abstract | Dual-layer: Brain Mode (Active Inference meta-cognitivo) + Loki Swarm Mode (30+ agentes especializados, ciclos spec-driven determinísticos); "Another Lethal Trifecta" mitigation | ⚠️ No especificado en abstract |

**Nota:** ⚠️ indica brechas de información donde los datos técnicos no están disponibles o son insuficientes en las fuentes analizadas.

---

## 3. Análisis Detallado por Documento

### 3.1. Source 1: GAIA Framework (RUA/UA)

#### 3.1.1. Enfoque Específico
GAIA (Governed AI for Interactive Applications) se centra en **Spec-Driven Development (SDD)** con una integración obligatoria de **Test-Driven Development (TDD)**. El framework trata TDD no como una sugerencia sino como un "imperativo sistémico" [1]. La arquitectura se estructura en tres tipos de artefactos SDD:

- **Rules (Reglas):** Actúan como la "Constitución" del proyecto, definiendo restricciones permanentes (MUST/MUST NOT) que influyen en la generación o modificación de código [18].
- **Skills:** Funcionan como una "biblioteca de referencia" con conocimiento estático detallado, convenciones y recursos/scripts, cargados bajo demanda según la intención del usuario [19].
- **Workflows:** Definen "protocolos de ejecución" con pasos bien establecidos, guiando la trayectoria del trabajo (planificación → acciones → validación → cierre) [20].

#### 3.1.2. Problema Abordado
El problema técnico central es la **desalineación del estado mental del proyecto** entre especificaciones y código [21]. GAIA busca minimizar la improvisación, forzar la validación (mediante tests y revisiones) y preservar la trazabilidad desde la intención hasta el ticket, commit y pull request [22].

Riesgos específicos que aborda:
- Ajustes visuales que rompen la accesibilidad
- Fixes rápidos que introducen deuda técnica
- Cambios globales que desalinean especificaciones y código
- Decisiones arquitectónicas ejecutadas automáticamente sin validation gates [21], [23]

#### 3.1.3. Lenguajes de Programación
- **Python:** Framework backend FastAPI [24]
- **TypeScript:** Frontend React con patrones de archivo 'src/**/*.ts' [25]
- **JavaScript:** Implícito a través del uso de React

#### 3.1.4. Stack de Desarrollo
**Backend:**
- FastAPI (Python framework) [24]

**Frontend:**
- React [24]

**Testing:**
- **Playwright:** Testing End-to-End (E2E) [26]
- **Pytest:** Testing backend (unit e integration) [27]
- **Vitest:** Testing frontend [27]

**Infraestructura:**
- **Docker:** Bases de datos efímeras reales para integration testing [28]
- **Git:** Control de versiones con patrones de workflow específicos [29], [30]

**Plataforma de Agentes IA:**
- **Antigravity:** Sistema de agentes IA usado para ejecutar workflows GAIA [24]
- **NotebookLM:** Herramienta IA para optimización de skills mediante crítica [31]

#### 3.1.5. Modelo LLM y Rol
GAIA no especifica modelos LLM concretos por nombre. Las referencias son genéricas:
- "IA" o "agente" a lo largo del documento
- "LLM distinto" al discutir mejora de skills [31]
- Uso de múltiples LLMs para refinamiento de skills: "he creado un notebookLM con información detallada sobre Playwright... Esta información la he usado para pedirle a NotebookLM que criticara y mejorara el skill inicial" [31]

**Rol:** Los LLMs actúan como agentes de desarrollo que seleccionan skills automáticamente basándose en la intención del usuario a través de descripciones de skills [32].

#### 3.1.6. Metodología de Validación
GAIA implementa un enfoque de testing multi-capa:

**Unit Testing:**
- Fuente de verdad: "historias de usuario y criterios de aceptación (Gherkin)" [26]
- Ejemplo: `def test_post_news_returns_400_when_title_missing(client)` [33]

**E2E Testing:**
- Herramienta: Playwright [26]
- Objetivo: "verificar qué hace el sistema (comportamiento / historia de usuario)" [26]
- Estrategia: "Traducir escenarios críticos (Happy Path) a Playwright. Priorizar selectores de accesibilidad (getByRole, getByLabel) frente a CSS frágil. Mantener pocos E2E, pero muy significativos." [34]

**Integration Testing:**
- "DB real efímera (Docker) y limpieza determinista por test (rollback/truncate)" [28]

**Ciclo TDD:**
- "RED (falla primero)" → GREEN (código mínimo para pasar) → REFACTOR (sin cambiar comportamiento) [35]

**Validation Gates:**
- El humano debe validar PRD y feature-descr.md antes de continuar [36]
- El humano debe validar cada plan generado en specs/features/[feature-slug]/ antes de continuar [37]
- "Regla de bloqueo: una feature no puede cerrarse si la auditoría marca riesgos Críticos o Altos sin mitigación explícita y tests que la respalden" [38]

#### 3.1.7. Gestión de Contexto
**Organización basada en scope:**
- **Scope Global:** Aplicado a todos los proyectos en el ordenador, almacenado en carpeta de usuario [39]
- **Scope Workspace:** Aplicado solo al proyecto actual, almacenado dentro del repositorio para compartir con el equipo vía Git [39]

**Inyección de contexto basada en patrones:**
- **Glob patterns:** Aplicados vía patrones de archivo para reglas específicas de tech/layer ('src/**/*.ts', 'backend/**') [25]
- **Enlaces referenciales:** Las reglas pueden usar '@mentions' para apuntar a documentos fuente, evitando duplicación y proporcionando contexto [40]

**Jerarquía de artefactos:**
- Rules → Skills → Workflows, cada uno con roles específicos en la gestión de contexto [18], [19], [20]

#### 3.1.8. Métricas de Performance
- **Límites de tamaño de archivo:** Las reglas deben mantener cada archivo dentro de límites de plataforma (≤ 12,000 caracteres) [41]
- **Velocidad/Costo de tests:** Tests de inner loop (unit/integration) deben ser baratos, rápidos y deterministas para mantener el flujo del desarrollador [27]. Tests E2E son menos pero críticos, balanceando costo y velocidad [42]
- **Cobertura de tests:** TDD asegura que no exista código 'huérfano' sin test [1]
- **Códigos de estado:** Tests de API verifican códigos HTTP correctos (ej. 400 para títulos faltantes) [43]

#### 3.1.9. Patrones de Orquestación
La arquitectura es una **mezcla** de cadenas secuenciales y agentes colaborativos autónomos, con fuerte énfasis en **flujos operativos gobernados**:

- **Cadenas Secuenciales (Workflows):** Los workflows definen procesos repetibles con pasos ordenados, guiando la trayectoria del trabajo (plan → acciones → validación → cierre) [20]. Ejemplos incluyen `/plan-feature-descr-from-user-conversation` y `/execute-plan` [44].
- **Interacción basada en Agentes:** El "agente" (IA) selecciona skills basándose en la intención del usuario, decide sobre la aplicación de rule/workflow/skill, y realiza tareas [32].
- **Aspectos Event-Driven:** Aunque no se llama explícitamente "event-driven", el sistema responde a intenciones (ej. "implementar una feature", "arreglar un bug") invocando flujos operativos específicos [45].
- **Double Loop TDD:** Dos ciclos concéntricos (Outer Loop para validación de usuario, Inner Loop para lógica/corrección) operan a diferentes velocidades y niveles de abstracción, complementándose mutuamente [1].

#### 3.1.10. Nivel Human-in-the-Loop
El sistema requiere **significativa aprobación e intervención humana** en etapas específicas, particularmente en generación y validación de especificaciones:

- **Validation Gates:** Validation Gates explícitos son usados, requiriendo que el desarrollador valide Product Requirements Documents (PRD) y descripciones de features antes de que el agente proceda [36]. Esto asegura una filosofía de "human in the loop" [36].
- **Protocolos de Revisión:** El agente debe detenerse y pedir clarificación cuando faltan inputs o no están claros, o antes de hacer cambios irreversibles [46].
- **Manejo de Ambigüedad:** Si falta información para un paso en un workflow, el workflow debe pedirla [47]. Los skills están diseñados para hacer preguntas si algo puede interpretarse de dos maneras [48].
- **Toma de Decisiones:** El agente puede proponer la ubicación correcta para un artefacto si hay un desajuste en el scope [49].
- **Activación Manual:** Las reglas pueden activarse manualmente, y el modelo puede decidir si una regla aplica basándose en su descripción [50].
- **Iteración y Optimización:** La mejora de skills, reglas y workflows es un proceso iterativo que puede refinarse por expertos humanos o pidiendo a la IA (ej. NotebookLM) que los optimice [31].
- **Cambios Críticos/Alto Riesgo:** Cambios a artefactos de proyecto SDD (reglas, arquitectura) se consideran de alto riesgo y requieren Validation Gates explícitos, no ejecución automática [23].
- **Auditoría de Seguridad:** Una feature no puede cerrarse si la auditoría marca riesgos críticos o altos sin mitigación explícita y tests de soporte [38].
- **Condiciones de Parada:** El sistema tiene condiciones de parada explícitas para problemas como contenido de artefacto faltante, intención ambigua o restricciones conflictivas [51].

---

### 3.2. Source 2: OPENDEV (arXiv 2603.05344)

#### 3.2.1. Enfoque Específico
OPENDEV se centra en **Harness Engineering** y **Context Engineering** como conceptos centrales para construir agentes de codificación IA efectivos para la terminal. El harness se define como "la capa de orquestación en tiempo de ejecución que envuelve el loop de razonamiento central y coordina la ejecución de herramientas, gestión de contexto, aplicación de seguridad y persistencia de sesión alrededor de él" [52].

La relación entre conceptos es fundacional: el scaffolding ensambla el agente antes del primer prompt, mientras que el harness orquesta el dispatch de herramientas, gestión de contexto y aplicación de seguridad en tiempo de ejecución [53]. Context engineering se trata como una preocupación de primera clase, gestionando la ventana de contexto del LLM a través de varios subsistemas [54].

#### 3.2.2. Problema Abordado
El principal cuello de botella y desafío técnico abordado es la **gestión de ventanas de contexto finitas sobre sesiones que rutinariamente exceden el presupuesto de tokens del modelo**, prevenir operaciones destructivas, y extender capacidades sin abrumar el presupuesto de prompt del agente [55].

El documento destaca que las herramientas tradicionales de completado de código son limitadas, y los asistentes de codificación agénticos necesitan razonar sobre tareas complejas, ejecutar planes multi-paso e interactuar con el entorno de desarrollo a través del uso de herramientas [56].

#### 3.2.3. Lenguajes de Programación
- **Rust:** El codebase de OPENDEV está escrito en Rust [57]
- **Lenguajes soportados para integración LSP:** Python, TypeScript/JS, Rust, Go, Java, C/C++, C#, Ruby, PHP, Swift, Kotlin, Lua, Elixir, Haskell, Scala, Julia, Perl, Clojure, Elm, Terraform, Bash, Nix, Erlang, AL, Rego, Fortran, OCaml, Markdown, YAML, Dart, Zig, R [58]

#### 3.2.4. Stack de Desarrollo
- **Frontends:** Textual (TUI) y FastAPI + WebSockets (Web UI) [59]
- **Motor de Navegador:** Playwright (usado por Crawl4AI para `fetch_url` y `capture_web_screenshot`) [60], [61]
- **Búsqueda:** Ripgrep (para búsqueda de contenido basada en regex) [62], AST-grep (para coincidencia de patrones estructurales) [62]
- **Búsqueda Web:** DuckDuckGo (para `web_search`) [63]
- **Integración LSP:** Servidores de lenguaje estándar (para análisis semántico de código multi-lenguaje) [64]
- **Gestión de Tareas:** Lista de tareas estilo Kanban [65]
- **Control de Versiones:** Git (para snapshots shadow git y comandos `git` en `main-git-workflow.md`) [66], [67]
- **Gestión de Procesos:** `subprocess.Popen` (para comandos foreground) [68], `pty.openpty()` (para comandos background) [68]
- **Sistema de Archivos:** `fcntl.flock` (para bloqueo de archivos) [69], `os.rename()` (para escrituras atómicas de archivos) [69], `os.path.getmtime` (para tracking de stale-read) [69]

#### 3.2.5. Modelo LLM y Rol
OPENDEV está diseñado como un **sistema IA compuesto**, no dependiendo de un único LLM monolítico, sino de un ensemble de agentes y workflows, cada uno independientemente vinculado a un LLM configurado por el usuario [70], [71].

Usa una **arquitectura de vinculación LLM por-workflow** donde cada workflow cognitivo selecciona un modelo vía configuración de usuario [72].

**Cinco roles de modelo distintos** están definidos, cada uno enrutando a modelos especializados, potencialmente con cadenas de fallback [73]:
- **Action model:** Modelo de ejecución primario para razonamiento basado en herramientas.
- **Thinking model:** Modelo opcional para razonamiento extendido sin acceso a herramientas. Fallback: action model.
- **Critique model:** Modelo opcional para auto-evaluación. Fallback: thinking model → action model.
- **Vision model:** Modelo vision-language para procesar screenshots e imágenes. Fallback: action model si es vision-capable.
- **Compact model:** Modelo más pequeño y rápido para summarization durante compactación de contexto. Prioriza velocidad y costo sobre profundidad de razonamiento. Fallback: action model.

Guía específica de proveedor para modelos **Anthropic** y **OpenAI** está incluida, con un fallback genérico para proveedores desconocidos [74], [75].

#### 3.2.6. Metodología de Validación
El sistema usa una **arquitectura de seguridad defense-in-depth** con cinco capas independientes para prevenir daño [76]:

**Capa 1: Prompt-Level Guardrails**
- Política de seguridad, seguridad de acción, read-before-edit, git workflow, recuperación de errores [76]

**Capa 2: Schema-Level Tool Restrictions**
- Whitelist de plan-mode, allowed_tools por-subagent, gating de descubrimiento MCP [76]

**Capa 3: Runtime Approval System**
- Niveles Manual/Semi-Auto/Auto, reglas de pattern/command/prefix/danger, permisos persistentes [76]

**Capa 4: Tool-Level Validation**
- Blocklist `DANGEROUS_PATTERNS`, detección de stale-read, truncamiento de output, timeouts [76]

**Capa 5: Lifecycle Hooks**
- Bloqueo pre-tool, mutación de argumentos, protocolo JSON stdin [76]

**Detección de Doom-loop:** Implementada con escalación de dos niveles, fingerprinting de llamadas a herramientas y tracking de recurrencias para inyectar advertencias o pausar ejecución [77].

**Tracking de Stale-read:** En `edit_file` verifica timestamps de archivo antes de editar para prevenir sobrescrituras de ediciones concurrentes del usuario [69].

**Índices Self-healing:** Para caches como el índice de sesión se reconstruyen automáticamente si faltan o están corruptos [78].

#### 3.2.7. Gestión de Contexto
**Adaptive Context Compaction (ACC):** Un pipeline de cinco etapas de estrategias de reducción progresivamente agresivas (warning, observation masking, fast pruning, aggressive masking, full LLM-based compaction) gestiona la presión de contexto incrementalmente [79], [80].

**Arquitectura de Memoria Dual para Bounded Thinking:** Separa contexto comprimido de largo alcance (memoria episódica, un resumen generado por LLM del historial completo de conversación) de contexto detallado de corto alcance (memoria de trabajo, los últimos varios pares de mensajes reproducidos verbatim) [81], [80].

**System Reminders:** Mensajes cortos de propósito único inyectados en el punto de decisión para contrarrestar el fade-out de instrucciones en sesiones de larga duración [54], [81], [80].

**Optimización de Resultados de Herramientas:** Transforma outputs crudos de herramientas en representaciones compactas que preservan semántica (ej. metadata para lecturas de archivo, conteos de coincidencias para búsquedas, conteos de ítems para listados de directorio, errores truncados) [82]. Outputs grandes que exceden 8,000 caracteres se offload a archivos scratch [82].

**Construcción Dinámica de System Prompt:** Ensambla el system prompt desde secciones modulares ordenadas por prioridad, filtrando instrucciones irrelevantes basándose en contexto de runtime [83].

**Lazy Tool Discovery:** Esquemas de herramientas externas se cargan on-demand a través de búsqueda por palabra clave, reduciendo overhead de tokens baseline [84].

**Prompt Caching a Nivel de Proveedor:** Para proveedores que lo soportan, el system prompt se divide en partes estables y dinámicas, cacheando la porción estable para reducir costo de input [74].

#### 3.2.8. Métricas de Performance
- **Métricas de Eficiencia:** Costos de API, tiempo de inferencia y consumo de tokens se usan para evaluación holística [85]
- **Medición de Completado de Tareas Largas:** Evalúa practicidad del agente para despliegue en mundo real [85]
- **Consumo de Contexto:** ACC reduce el consumo de contexto pico de observaciones en aproximadamente 54% [79]
- **Reducción de Costo de Tokens:** Lazy discovery de herramientas MCP reduce overhead de tokens baseline a casi cero (<5%) [84]. Prompt caching produce aproximadamente 88% de reducción en costo de tokens de input para la porción cacheada [74]
- **Longitud de Sesión:** El summarizer por-herramienta extiende la longitud típica de sesión de 15–20 turnos a 30–40 turnos sin compactación [82]
- **Tasas de Compliance:** Los reminders de user-role produjeron tasas de compliance notablemente más altas que mensajes de system-role [81]

#### 3.2.9. Patrones de Orquestación
OPENDEV emplea una **arquitectura de sistema IA compuesto** con enrutamiento de modelo especializado por carga de trabajo y una **arquitectura dual-agent separando planificación de ejecución** [57].

**Sesiones Concurrentes:** El trabajo se organiza en sesiones concurrentes, cada una compuesta de múltiples sub-agentes especializados, con cada agente ejecutando workflows tipados (Execution, Thinking, Compaction) [86].

**Extended ReAct Execution Pipeline:** Extiende el ciclo ReAct estándar con fases explícitas de thinking y opcional self-critique, integrando compactación de contexto por etapas en el loop de razonamiento [72], [87]. El loop ejecuta seis fases por iteración: pre-check y compactación, thinking, self-critique, action, ejecución de herramienta, y post-processing [88].

**Delegación de Subagent:** El agente principal puede spawnar subagents especializados para subtareas específicas, que ejecutan en contextos aislados con acceso filtrado a herramientas y prompts especializados. Múltiples llamadas `spawn_subagent` en la misma respuesta LLM se ejecutan concurrentemente [84].

**Dual-path Dispatch:** El input del usuario se clasifica en el boundary del REPL; comandos slash se enrutan a command handlers registrados para ejecución determinística, mientras que queries en lenguaje natural pasan a través del Query Processor al Agent Loop para razonamiento involucrando LLM [89].

#### 3.2.10. Nivel Human-in-the-Loop
OPENDEV soporta **colaboración humano-agente** a través de sus workflows de aprobación, ejecución interactiva de comandos y loops de feedback estructurados [85].

**Runtime Approval System:** Gates de ejecución de herramientas basados en boundaries de confianza configurados por usuario, con niveles de autonomía Manual, Semi-Auto y Auto. Las reglas de aprobación persisten a través de sesiones para prevenir fatiga [76], [84].

**Herramienta `ask_user`:** Presenta preguntas multi-choice estructuradas para recopilar información clarificadora, preferencias de usuario o confirmar decisiones críticas, bloqueando al agente hasta recibir respuesta [65].

**Herramienta `present_plan`:** Lee un archivo de plan, muestra su contenido al usuario para revisión, y solicita aprobación explícita antes de proceder a implementación. Los usuarios pueden revisar o aprobar el plan [65].

**Interrupciones:** Los usuarios pueden interrumpir la ejecución del agente (ej. vía `Ctrl+C` en TUI) [90].

---

### 3.3. Source 3: Spec-Driven Development Paper (arXiv 2602.00180)

#### 3.3.1. Enfoque Específico
El enfoque primario del documento es **Spec-Driven Development (SDD)**. Explora cómo SDD, donde las especificaciones son la fuente de verdad y el código es un artefacto generado o verificado, invierte el workflow tradicional de desarrollo de software. El paper busca proporcionar una guía comprensiva a SDD, incluyendo sus principios, patrones de workflow y herramientas de soporte, especialmente en el contexto de asistentes de codificación IA [91].

#### 3.3.2. Problema Abordado
El documento aborda el problema de la **realidad code-centric** en desarrollo de software, donde el código se convierte en la 'verdad de facto' del sistema, llevando a problemas como: requisitos que derivan, diagramas de diseño que se pudren, tests escritos después del hecho, y dificultad para nuevos desarrolladores o asistentes IA para entender la intención de una función o sistema sin reverse-engineering [92].

Específicamente, destaca que **los modelos IA son excelentes en completado de patrones pero pobres en lectura de mentes** [93]. Esto lleva a 'vibe coding' donde prompts sueltos resultan en outputs inconsistentes o erróneos de Large Language Models (LLMs) porque hacen docenas de asunciones no declaradas, muchas de las cuales son incorrectas [94]. SDD mejora la confiabilidad de agentes de codificación proporcionando contratos ejecutables no ambiguos [94].

#### 3.3.3. Lenguajes de Programación
El documento menciona lenguajes de programación en el contexto de herramientas y frameworks específicos:
- **Frameworks BDD** como Cucumber soportan Ruby, Java y JavaScript [95]
- **Python** es soportado por Behave [95]
- **.NET** es soportado por SpecFlow [95]
- **Código C** es generado desde modelos Simulink en sistemas embebidos [96]

#### 3.3.4. Stack de Desarrollo y Frameworks
El paper discute varias herramientas y frameworks que soportan SDD:
- **Frameworks BDD:** Cucumber, SpecFlow, Behave [95]
- **Frameworks TDD:** RSpec, JUnit, pytest [97]
- **Herramientas de Especificación de API:** OpenAPI/Swagger, GraphQL SDL, Protocol Buffers, AsyncAPI [98], [99]
- **Herramientas de Contract Testing:** Pact, Specmatic [97]
- **Herramientas SDD Asistidas por IA:** GitHub Spec Kit, Amazon Kiro, Tessl [97]
- **Model-Based Design:** Simulink, SCADE [97]

#### 3.3.5. Modelo LLM y Rol
El documento se refiere a **Large Language Models (LLMs)** en general, tales como **GPT-4 o Claude**, como agentes de codificación que se benefician de SDD [100]. No especifica versiones particulares o detalles más allá de estas menciones generales.

#### 3.3.6. Metodología de Validación
La **fase validate** en el workflow SDD asegura que el código realmente cumple con la especificación. Esto involucra combinar verificación automatizada con juicio humano [101].

La validación abarca:
- Ejecutar **tests automatizados** a niveles unit, integration y acceptance [101]
- Ejecutar **escenarios BDD** contra la implementación [101]
- Revisar adherencia a **requisitos no funcionales** [101]
- Conducir **stakeholder acceptance testing** [101]

Si la validación revela gaps, el equipo decide si arreglar el código o revisar la spec, con la spec permaneciendo como la autoridad [101].

#### 3.3.7. Gestión de Contexto
Las especificaciones actúan como **'super-prompts'** que descomponen problemas complejos en componentes modulares alineados con las ventanas de contexto de los agentes, permitiendo a sistemas IA manejar complejidad que abrumaría prompts de un solo disparo [100].

En la fase `plan`, el plan proporciona **contexto crucial** a asistentes de codificación IA, informándoles no solo qué construir sino cómo está estructurado el sistema y qué convenciones debe seguir [102].

#### 3.3.8. Métricas de Performance y KPIs
El documento destaca una métrica clave de performance de un caso de estudio:
- **Reducción de tiempo de ciclo de integración:** Una compañía de servicios financieros logró una **reducción del 75% en tiempo de ciclo de integración** para cambios de API adoptando desarrollo spec-anchored con OpenAPI [103]. Esto fue debido a capturar incompatibilidades en la etapa de revisión de spec en lugar de en producción [103].

#### 3.3.9. Patrones de Orquestación
El workflow para SDD asistido por IA sigue cuatro fases explícitas: `/specify` (genera una spec detallada), `/plan` (crea arquitectura técnica), `/tasks` (descompone el plan en tareas de implementación), e `implementation` (genera código tarea por tarea) [104]. Revisión y refinamiento humano ocurren en cada fase para mantener alineación [104].

Las especificaciones permiten **ejecución paralela de agentes** en tareas no solapadas, con orquestación para dependencias. Los equipos pueden particionar trabajo a nivel de spec, permitiendo a múltiples agentes IA implementar diferentes componentes simultáneamente sin interferencia [105].

#### 3.3.10. Nivel Human-in-the-Loop
**Supervisión humana** aún se requiere en la fase `implement`, incluso con automatización sustancial de asistencia IA [106].

En el workflow SDD asistido por IA, **revisión humana** es crucial en cada checkpoint para asegurar alineación con intención [104].

Un enfoque emergente llamado **métodos 'self-spec'** involucra LLMs creando sus propias especificaciones desde un prompt de alto nivel, que luego son **revisadas y refinadas por humanos** antes de implementación [107].

Las especificaciones requieren la misma **revisión cuidadosa que el código**, y no son una bala de plata que elimina la necesidad de **juicio humano** sobre requisitos [108].

---

### 3.4. Source 4: CURRANTE (arXiv 2601.03878)

#### 3.4.1. Enfoque Específico
El documento se centra primariamente en **Specification-Driven Development (SDD)**, particularmente en el contexto de Large Language Models (LLMs) y generación de código [109], [110]. SDD se presenta como un enfoque más abstracto y estructurado al desarrollo, cambiando desde métodos tradicionales code-centric [110]. Los autores buscan proporcionar insights empíricos en el diseño de entornos de desarrollo de próxima generación que alineen el razonamiento humano con generación de código model-driven [109].

Aunque no define explícitamente 'Harness Engineering' o 'Context Engineering' como áreas de enfoque distintas, el énfasis del estudio en interacción de usuario, refinamiento de especificaciones y workflows estructurados inherentemente toca la gestión y aprovechamiento de contexto en un entorno controlado.

#### 3.4.2. Problema Abordado
El principal problema técnico abordado es que mientras Large Language Models (LLMs) se integran cada vez más en workflows de desarrollo de software, su comportamiento en procesos estructurados specification-driven permanece pobremente comprendido [109]. Específicamente, hay un gap en entender el rol del usuario en guiar al LLM a través de especificación estructurada de requisitos y definición de casos de test, y qué tan efectivamente los usuarios pueden expresar su intención a través del workflow basado en Test-Driven Development (TDD) general [111].

El estudio busca abordar esto investigando cómo la intervención humana en especificación y refinamiento de tests influencia la calidad y dinámicas del código generado por LLM [109].

#### 3.4.3. Lenguajes de Programación
El único lenguaje de programación explícitamente mencionado en el documento es **Python** [112]. La familiaridad con Python de los participantes del estudio se evaluará como variable confundidora [112]. Los casos de test también se describen como generando un archivo de unit-test ejecutable, típicamente asociado con el framework `unittest` de Python [113], [114].

#### 3.4.4. Stack de Desarrollo
Los componentes primarios del stack de desarrollo mencionados son:
- **CURRANTE:** Una extensión de Visual Studio Code que habilita un workflow human-in-the-loop para generación de código asistida por LLM [109]. Implementa un workflow típico de generación de código TDD [115].
- **Visual Studio Code (VS Code):** El popular Integrated Development Environment (IDE) dentro del cual CURRANTE opera [115], [116].
- **Formato TOML:** Usado para definir requisitos de problema y especificaciones de usuario, diseñado para ser human-readable y fácil de editar [117], [118].
- **unittest:** Un framework de testing de Python, implicado por la mención de generar un archivo de unit-test ejecutable y `self.assertEqual` en el ejemplo [113], [114].

#### 3.4.5. Modelo LLM y Rol
El documento menciona **Qwen3-Coder [17]** como una opción posible para el modelo LLM subyacente, notando que está especializado para tareas de codificación [119]. El rol específico del LLM en la arquitectura es generar test suites desde especificaciones TOML, y luego generar la implementación final de código basada en los casos de test refinados [117], [120].

Los templates de prompt del LLM y parámetros clave (ej. temperatura, endpoint del modelo) se fijarán para maximizar consistencia [121].

#### 3.4.6. Metodología de Validación
El sistema verifica output a través de un **workflow Test-Driven Development (TDD)** [122]. El proceso involucra:

**Especificación:** Los usuarios ingresan una especificación estructurada en lenguaje natural del problema [117].

**Casos de Test:** Una test suite inicial se genera automáticamente desde la especificación TOML por el LLM. Los usuarios luego revisan, refinan y mejoran estos casos de test, que describen formalmente los requisitos [117].

**Generación de Código:** El LLM genera el código basado en la test suite refinada. El sistema luego verifica sus resultados de ejecución contra la test suite [120].

**Iteración:** Los usuarios pueden refinar iterativamente casos de test o regenerar funciones basándose en advice de outputs fallidos hasta que todos los tests pasen o se agote un presupuesto de tiempo [113].

#### 3.4.7. Gestión de Contexto
La inyección y mantenimiento de contexto se manejan primariamente a través de:

**Formato TOML:** Este formato estructurado captura intención de usuario y guía al LLM en generar la test suite inicial [117]. Proporciona el contexto inicial para generación de tests [118].

**Test Suite:** La test suite generada y refinada misma sirve como la especificación de input para el workflow general de Spec-Driven Development [118]. El usuario puede refinar iterativamente los casos de test, proporcionando guía adicional al LLM [120].

#### 3.4.8. Métricas de Performance
El estudio emplea varias métricas cuantitativas para evaluar eficiencia y efectividad del sistema:

**Métricas de Efectividad:**
- `PassAll`: Si la función final generada pasa todos los tests en la suite [123]
- `PassRate`: Fracción de tests pasados sobre tests totales para la submission final [123]
- `TestCoverage`: Cuánto del código fuente se ejecuta cuando la test suite corre [123]
- `TestDiversity`: Qué tan diferentes son los casos de test entre sí, usando una métrica de similitud [123]

**Indicadores de Eficiencia:**
- `TimeToPass`: Tiempo desde producir la primera test suite hasta la primera submission que pasa todos los tests (o expiración de presupuesto) [123]
- `IterationsToPass`: Número de regeneraciones de función hasta primer all-pass (o expiración de presupuesto) [123]

**Comportamientos de Interacción:**
- `TestEdits`: Número de acciones por-test (explain/regenerate/delete) realizadas por el participante [123]
- `SuiteRegenerations`: Número de regeneraciones de test-suite completas disparadas [123]
- `AdviceTriggers`: Número de veces que advice fue solicitado/generado desde outputs fallidos [123]

**Métricas de Uso de LLM:** Conteos de tokens para cada llamada API y tokens totales consumidos por participante, habilitando análisis de eficiencia y costo [124].

#### 3.4.9. Patrones de Orquestación
La arquitectura sigue una **cadena secuencial** o workflow Test-Driven Development (TDD) de tres fases [115], [122]:

1. **Fase de Especificación:** El usuario ingresa una especificación estructurada en lenguaje natural del problema [117]
2. **Fase de Casos de Test:** El usuario guía al LLM en generar y refinar una test suite que describe formalmente los requisitos [117]
3. **Fase de Generación de Código:** El LLM genera el código y verifica sus resultados de ejecución contra la test suite [120]

El usuario puede refinar iterativamente los casos de test generados y regenerar el código después de refinar los casos de test, indicando un feedback loop dentro de este proceso secuencial [120].

#### 3.4.10. Nivel Human-in-the-Loop
El sistema requiere explícitamente **intervención humana** en etapas específicas:

**Definición de Especificación:** Los usuarios son responsables de definir los requisitos del problema de manera estructurada, usando un archivo TOML [118].

**Generación y Refinamiento de Casos de Test:** Mientras una test suite inicial se genera automáticamente por el LLM, el rol del usuario es crucial en revisar, refinar y mejorar estos casos de test [117]. Esto incluye solicitar explicaciones en lenguaje natural para tests individuales, eliminar los irrelevantes, o proporcionar guía adicional al LLM para regenerar tests [120].

**Generación de Código (Aprobación Implícita):** Una vez que el usuario está satisfecho con la test suite, proceden a la fase de generación de código, implicando aprobación humana de la especificación de test antes de generación de código [120]. El LLM luego genera el código, y el usuario puede regenerar el código después de refinar más los casos de test si es necesario [120].

El estudio busca analizar cómo la intervención humana en especificación y refinamiento de tests influencia la calidad y dinámicas del código generado por LLM [109].

---

### 3.5. Source 5: DDC - Demand-Driven Context (arXiv 2603.14057)

#### 3.5.1. Enfoque Específico
El documento se centra primariamente en **Context Engineering** [125], [126]. Describe Demand-Driven Context (DDC) como una metodología para context engineering, específicamente proporcionando un proceso sistemático para determinar qué conocimiento enterprise debe existir para agentes LLM [126].

#### 3.5.2. Problema Abordado
El principal problema técnico abordado es el fallo consistente de agentes large language model en tareas específicas de enterprise debido a falta de conocimiento de dominio. Esto incluye terminología faltante, procedimientos operacionales, interdependencias de sistema, estructuras de datos y decisiones institucionales, que a menudo existen como 'conocimiento tribal' dentro de organizaciones [127].

Los enfoques existentes como ingeniería de conocimiento top-down y automatización bottom-up tienen limitaciones fundamentales en adquirir este tipo de conocimiento de dominio [127].

#### 3.5.3. Lenguajes de Programación
⚠️ **Brecha de información:** No se mencionan lenguajes de programación específicos explícitamente en el documento.

#### 3.5.4. Stack de Desarrollo
⚠️ **Brecha de información:** No se mencionan frameworks, librerías, herramientas o servicios de infraestructura específicos explícitamente en el documento.

#### 3.5.5. Modelo LLM y Rol
⚠️ **Brecha de información:** No se mencionan modelos AI/LLM específicos (ej. GPT-4o, Claude 3.5 Sonnet) explícitamente. El documento se refiere generalmente a "Large language model agents" y "Modern LLMs" [127], [128]. Su rol es realizar capacidades de razonamiento y procesamiento, pero fallan consistentemente en tareas enterprise debido a falta de conocimiento de dominio [127].

#### 3.5.6. Metodología de Validación
La validación dentro de la metodología DDC involucra varios pasos:

**Validación Humana de Output:** Un experto de dominio revisa el output del agente para corrección, y correcciones menores se incorporan. Esto asegura que el conocimiento curado lleve a razonamiento correcto [129]. Si el output es incorrecto, se inicia un correction loop [130].

**Checks Automatizados:** En la arquitectura de curación semi-automatizada propuesta, el contenido graduado se somete como pull request a una base de conocimiento centralizada. Checks automatizados validan schema, relaciones y convenciones de naming [131]. Un experto de dominio humano luego revisa para corrección y aprueba [131].

#### 3.5.7. Gestión de Contexto
El mecanismo técnico primario para gestión de contexto en DDC es un `typed entity meta-model` [132]. El conocimiento se organiza como entidades tipadas almacenadas como archivos markdown versionados con YAML frontmatter [132], [133]. Este formato estructurado habilita navegación, traversal de relaciones y validación [133].

La metodología se enfoca en curar solo el conocimiento mínimo requerido para tener éxito, basándose en fallos del agente [132]. Retrieval-Augmented Generation (RAG) se menciona como un enfoque relacionado para recuperar documentos en query time, pero DDC se presenta como abordando la curación del conocimiento mismo, en lugar de solo recuperación [134].

#### 3.5.8. Métricas de Performance
El documento usa varias métricas cuantitativas para evaluar el proceso DDC:

- **Nuevas entidades creadas por ciclo:** Esta métrica generalmente decrece con el tiempo, indicando que se necesita menos conocimiento nuevo a medida que la base de conocimiento crece [135]
- **Entidades reusadas por ciclo:** Esta métrica aumenta con el tiempo, mostrando que el agente aprovecha conocimiento existente más efectivamente en ciclos posteriores [135]
- **Correction loops:** El número de intentos rechazados y correction loops decrece a medida que la base de conocimiento acumula contexto suficiente para diagnósticos precisos en primer intento [136]
- **Reuse ratio:** El ratio `rn/(en + rn)` (entidades reusadas a entidades totales) aumenta, indicando que una fracción mayor de conocimiento existente se reusa [136]
- **Tiempo total de curación:** El tiempo gastado por ciclo se mide, con un promedio de 30 minutos por ciclo observado en el ejemplo trabajado [136]

#### 3.5.9. Patrones de Orquestación
La arquitectura DDC sigue una **metodología cíclica** disparada por problemas reales [137]. Involucra un flujo secuencial con un **correction loop**:

- **Problem Trigger:** Un problema real inicia un ciclo [137]
- **Agent Attempt (Failure):** El agente intenta resolver el problema con su conocimiento actual y falla [137]
- **Identify Gaps:** El agente genera un checklist de información de conocimiento faltante [137]
- **Human Curation:** Un experto humano proporciona respuestas dirigidas, curando solo el contexto mínimo necesario [137]
- **Agent Re-attempt:** El agente re-intenta el problema con el nuevo contexto [137]
- **Correction Loop:** Si el output del agente aún es incorrecto, el ciclo retorna al paso de identificación de gaps, repitiendo hasta que el output se valida [130]
- **Knowledge Graduation:** El conocimiento validado se estructura como entidades tipadas y se mueve a la base de conocimiento permanente [129]

Este proceso es análogo a Test-Driven Development (TDD), donde un test fallido (fallo del agente) impulsa la creación de código mínimo (contexto) para pasar [138].

#### 3.5.10. Nivel Human-in-the-Loop
DDC depende fuertemente de intervención humana, particularmente en las etapas de curación y validación:

**Information Provider:** Los humanos responden las preguntas del checklist del agente, proporcionando conocimiento específico de dominio que existe como 'conocimiento tribal' [139].

**Entity Author:** Los humanos estructuran estas respuestas como entidades tipadas, aunque este rol puede automatizarse parcialmente en la arquitectura de escalado propuesta [139].

**Validator:** Los humanos revisan el output del agente para corrección, incorporando correcciones menores [129]. En el modelo semi-automatizado, los humanos revisan y aprueban contenido graduado después de checks automatizados [131].

**Correction Loop:** Los expertos humanos son críticos en corregir fabricaciones o malentendidos del agente durante el correction loop, asegurando que conocimiento de dominio plausible pero falso no se incorpore en la base de conocimiento [140].

Mientras la metodología DDC base requiere curación manual para cada entidad, una arquitectura de escalado propuesta introduce curación semi-automatizada para reducir el esfuerzo humano de authoring a reviewing, abordando así el cuello de botella de dependencia humana [139], [141]. El rol humano es esencial para identificar gaps de conocimiento y proporcionar respuestas que existen solo en cabezas humanas [142].

---

### 3.6. Source 6: MCE - Meta Context Engineering (arXiv 2601.21557)

#### 3.6.1. Enfoque Específico
El documento se centra primariamente en **Context Engineering (CE)**, particularmente abordando sus limitaciones e introduciendo **Meta Context Engineering (MCE)**. CE se define como la disciplina para optimizar contexto en inference-time para large language models (LLMs) para maximizar utilidad downstream y habilitar auto-mejora continua [143].

MCE se presenta como un framework de optimización bi-nivel que supersede heurísticas CE estáticas co-evolucionando skills CE y artefactos de contexto [144].

#### 3.6.2. Problema Abordado
El principal cuello de botella abordado es que los métodos actuales de Context Engineering (CE) dependen de harnesses agénticos manualmente crafted, que imponen sesgos estructurales y restringen la optimización de contexto a un espacio de diseño estrecho, bound por intuición [145], [146].

Estos métodos a menudo usan workflows rígidos de generation-reflection y schemas de contexto predefinidos, llevando a limitaciones en adaptabilidad y generalización [145].

#### 3.6.3. Lenguajes de Programación
El único lenguaje de programación explícitamente mencionado es **Python** [147].

#### 3.6.4. Stack de Desarrollo
Los siguientes frameworks, librerías y herramientas se mencionan:
- **LangChain** y **LangChain DeepAgents** [148], [149]
- **Claude Agent SDK** [150], [151]
- **DSPy** [152]
- **Hugging Face** (para datasets)
- **OpenRouter** (para acceder modelos) [152]
- **NumPy** (para similitud de embeddings)
- **Pydantic** (para schemas de output estructurado)
- **Dotenv** (para variables de entorno)
- **Asyncio** (para operaciones asíncronas)

#### 3.6.5. Modelo LLM y Rol
Modelos LLM específicos mencionados y sus roles incluyen:

- **DeepSeek-V3.1:** Usado como modelo generator default para inferencia durante training y testing a través de la mayoría de métodos y benchmarks [152]
- **Qwen3-8B:** Usado como modelo generator para el benchmark AEGIS2, que requiere modelos lightweight para guardrails de seguridad [152]
- **MiniMax M2.1:** Usado como modelo agéntico por default para MCE. También se usa para reemplazar el reflector y curator en ACE para estudios de ablación para determinar si las ganancias de MCE provienen de su metodología o capacidades superiores del modelo [152]
- **Llama3.3-70B** y **Gemma3-4B:** Usados en experimentos para evaluar transferabilidad de contexto strong-to-weak [152]

#### 3.6.6. Metodología de Validación
El sistema verifica output a través de evaluaciones comprensivas a través de cinco dominios diversos (finance, chemistry, medicine, law, y AI safety). Compara MCE contra métodos CE state-of-the-art bajo settings offline y online. La evaluación involucra evaluar performance de contexto, adaptabilidad, transferabilidad y eficiencia de training.

Para tareas específicas, métricas como `pass@1 prediction accuracy` para FiNER y Symptom2Disease, `pass@1 exact match accuracy` para USPTO-50k, `micro-F1 score` para LawBench, y `F1 score` para AEGIS2 se usan.

El skill óptimo en MCE también incorpora criterios de parada explícitos, reconociendo cuándo refinamiento adicional es contra-productivo basándose en performance de validación.

#### 3.6.7. Gestión de Contexto
MCE gestiona contexto como archivos y código flexibles, instanciados como una colección de archivos en un directorio designado. Incluye tanto componentes estáticos (ej. bases de conocimiento, reglas de decisión, ejemplos) como operadores dinámicos (ej. retrieval, filtering, lógica de composición).

El framework enfatiza una "vista global de contexto acumulado" para reestructurar y refinar conocimiento existente, en lugar de ciegamente agregar nuevos ítems, y procesa batches grandes de rollouts de training para agregar feedback.

Funciones de retrieval se usan para retornar contexto relevante, con ejemplos incluyendo una estrategia de retrieval completo default para FiNER y un sofisticado sistema de routing basado en reglas de 1,440 líneas con cascadas priorizadas para Symptom2Disease.

#### 3.6.8. Métricas de Performance
Key Performance Indicators (KPIs) y métricas cuantitativas usadas incluyen:

- **Mejora Relativa:** MCE demuestra 5.6–53.8% mejora relativa sobre métodos state-of-the-art (media de 16.9%)
- **Porcentajes de Accuracy:** Tales como `Acc.%↑` para FiNER, USPTO-50k, Symptom2Disease, y `F1↑` para Aegis2.0
- **Micro-F1 Score:** Para LawBench (`Micro-F1↑`)
- **Context Adaptability:** Medida por ajuste flexible de longitud de contexto (ej. 1.5K a 86K tokens)
- **Context Efficiency:** Logrando mejor performance con menos tokens de contexto
- **Context Transferability:** Exhibiendo menor degradación de performance al transferir contextos de modelos strong a weak
- **Training Efficiency:** Acelerando training (ej. 13.6x speedup) y requiriendo menos rollouts (ej. 4.8x menos)
- **Rollout Efficiency:** MCE requiere menos rollouts para alcanzar accuracy objetivo (ej. 450 rollouts para 95% accuracy vs. 2169 para ACE)

#### 3.6.9. Patrones de Orquestación
MCE opera como un **framework de optimización bi-nivel** [144]:

**Meta-level:** Un meta-agente refina engineering skills a través de agentic crossover, que involucra una búsqueda deliberativa sobre el historial de skills, sus ejecuciones y evaluaciones [145]. Este nivel usa una `(1 + 1)-Evolution Strategy (ES)` y `agentic crossover` para sintetizar skills superiores razonando a través de especificaciones de tarea, trayectorias CE históricas y métricas de performance [148].

**Base-level:** Un agente de base-level ejecuta estos skills evolucionados, aprende de rollouts de training y optimiza contexto como archivos y código flexibles [145]. Esto involucra un proceso de optimización de contexto totalmente agéntico, aprovechando toolkits de coding y acceso al sistema de archivos [148].

El proceso general es iterativo, con cada iteración consistiendo de fases de skill evolution, context optimization y evaluation [149].

#### 3.6.10. Nivel Human-in-the-Loop
El sistema busca **optimización de contexto totalmente agéntica** [148]. Transiciona de workflows CE manualmente crafted a sistemas meta- y self-learning totalmente agénticos [150].

El agente meta-level refina engineering skills vía `agentic crossover`, un proceso de búsqueda deliberativa [145]. El agente base-level ejecuta estos skills y aprende de rollouts de training sin intervención humana explícita en el loop de optimización [145].

El paper implica que el sistema opera autónomamente en su skill evolution y optimización de contexto, con especificidad de tarea inyectada a través de skills aprendidos en lugar de scaffolding manual [150].

---

### 3.7. Source 7: Taylor & Francis (PAYWALL)

#### ⚠️ BRECHA DE INFORMACIÓN - ACCESO RESTRINGIDO

**Estado:** Solo metadata y abstract disponibles debido a restricción de paywall.

**Información Disponible:**
- **Título:** [No disponible en los archivos analizados]
- **Autores:** [No disponible en los archivos analizados]
- **Publicación:** Taylor & Francis
- **Tipo:** Artículo académico (presumiblemente peer-reviewed)

**Información No Disponible:**
- Enfoque específico (Harness/Spec-driven/Context Engineering)
- Problema técnico abordado
- Lenguajes de programación
- Stack de desarrollo
- Modelos LLM utilizados
- Metodología de validación
- Mecanismos de gestión de contexto
- Métricas de performance
- Patrones de orquestación
- Nivel de human-in-the-loop

**Recomendación:** Para completar este análisis comparativo, se requiere acceso institucional o compra del artículo completo.

---

### 3.8. Source 8: GABBE Architecture (TechRxiv)

#### 3.8.1. Enfoque Específico
GABBE (Generative Architectural Brain Base Engine) se centra en **Harness Engineering** con una arquitectura neurocognitiva. El sistema se presenta como un "sistema de software autónomo de tercera generación construido sobre principios neurocognitivos" [153].

La arquitectura presenta una **estructura dual-layer**:
- **Brain Mode (Meta-cognitivo):** Utiliza Active Inference para minimizar riesgo de proyecto [153]
- **Loki Swarm Mode:** Ejecuta ciclos de vida de desarrollo spec-driven determinísticos a través de 30+ agentes especializados (personas) [153]

#### 3.8.2. Problema Abordado
GABBE aborda limitaciones críticas en Multi-Agent Systems (MAS) existentes, particularmente concernientes a:
- **Context decay:** Degradación de contexto en sesiones largas
- **Non-determinismo:** Falta de predictibilidad en comportamiento de agentes
- **Cost asymmetry:** Asimetría de costos en uso de LLMs [153]

El paper también menciona limitaciones de "flat-topology orchestrators" que GABBE busca superar [153].

#### 3.8.3. Lenguajes de Programación
⚠️ **Brecha de información:** No se especifican lenguajes de programación en el abstract disponible.

#### 3.8.4. Stack de Desarrollo
⚠️ **Brecha de información:** No se especifican frameworks, librerías o herramientas en el abstract disponible.

#### 3.8.5. Modelo LLM y Rol
El abstract menciona **"cost-effective Large Language Model (LLM) routing"** [153], indicando que el sistema implementa estrategias de enrutamiento para optimizar costos de LLM, pero no especifica modelos concretos.

**Rol:** Los LLMs se utilizan dentro de la arquitectura de swarm para ejecutar tareas de ingeniería de software, con enrutamiento inteligente para optimizar costos.

#### 3.8.6. Metodología de Validación
⚠️ **Brecha de información:** El abstract no proporciona detalles sobre metodología de validación específica.

El paper menciona "programmatic enforcement of the 'Another Lethal Trifecta' mitigation strategy" [153], sugiriendo un enfoque de validación basado en mitigación de riesgos, pero sin detalles técnicos en el abstract.

#### 3.8.7. Gestión de Contexto
GABBE implementa una **jerarquía de memoria de 4 capas** [153], aunque el abstract no detalla la estructura específica de estas capas.

Esta arquitectura de memoria multi-capa presumiblemente aborda el problema de context decay mencionado como una de las limitaciones de sistemas existentes.

#### 3.8.8. Métricas de Performance
⚠️ **Brecha de información:** No se proporcionan métricas cuantitativas específicas en el abstract disponible.

El paper menciona realizar un "análisis comparativo" que "demuestra la superioridad arquitectónica de GABBE sobre orquestadores flat-topology" [153], pero las métricas específicas no están disponibles sin acceso al paper completo.

#### 3.8.9. Patrones de Orquestación
GABBE implementa una **arquitectura dual-layer** distintiva:

**Brain Mode (Meta-cognitivo):**
- Utiliza **Active Inference** (framework neurocientífico) para toma de decisiones de alto nivel
- Objetivo: Minimizar riesgo de proyecto [153]

**Loki Swarm Mode:**
- **30+ agentes especializados** organizados como personas
- Ejecuta **ciclos de vida de desarrollo spec-driven determinísticos**
- Enfoque en ejecución predecible y estructurada [153]

**Estrategia de Mitigación:**
- Implementación programática de "Another Lethal Trifecta" mitigation strategy [153]

Esta arquitectura representa un enfoque híbrido que combina:
1. Razonamiento meta-cognitivo de alto nivel (Brain Mode)
2. Ejecución distribuida especializada (Loki Swarm Mode)
3. Determinismo en ciclos de desarrollo (spec-driven)

#### 3.8.10. Nivel Human-in-the-Loop
⚠️ **Brecha de información:** El abstract no especifica el nivel de intervención humana requerida.

El sistema se describe como "autónomo" [153], sugiriendo un nivel bajo de human-in-the-loop, pero sin detalles sobre validation gates, aprobaciones o puntos de intervención humana específicos.

**Inferencia:** Dado el énfasis en "determinismo" y "spec-driven development", es probable que el sistema requiera intervención humana en la fase de especificación inicial, pero opere autónomamente durante la ejecución, similar a otros sistemas spec-driven analizados.

---

## 4. Análisis Transversal

### 4.1. Patrones Comunes Identificados

#### 4.1.1. Convergencia hacia Spec-Driven Development
Seis de los ocho documentos analizados (GAIA, OPENDEV, SDD Paper, CURRANTE, GABBE, y parcialmente MCE) convergen en la necesidad de **especificaciones formales como fuente de verdad** [1], [91], [109], [153]. Este patrón emerge como respuesta al problema fundamental de "vibe coding" donde prompts ambiguos producen outputs inconsistentes [94].

**Niveles de rigor identificados:**
- **Spec-first:** Especificaciones completas antes de cualquier código (GAIA, SDD Paper)
- **Spec-anchored:** Especificaciones para interfaces críticas (SDD Paper con OpenAPI)
- **Spec-as-source:** Especificaciones ejecutables que generan código (CURRANTE, parcialmente GABBE)

#### 4.1.2. Gestión de Contexto como Disciplina Central
Todos los sistemas reconocen la **ventana de contexto finita** como restricción arquitectónica fundamental. Las estrategias convergen en tres enfoques:

**Compactación Adaptativa (OPENDEV):**
- Pipeline de 5 etapas con estrategias progresivamente agresivas
- Reducción del 54% en consumo de contexto pico [79]

**Estructuración Tipada (DDC):**
- Entidades tipadas con YAML frontmatter
- Versionado y navegación de relaciones [132], [133]

**Optimización Meta-nivel (MCE):**
- Co-evolución de skills y artefactos de contexto
- Vista global para reestructuración en lugar de agregación ciega [144]

#### 4.1.3. Validación Multi-capa
Los sistemas maduros implementan **defense-in-depth** con múltiples capas de validación:

**OPENDEV (5 capas):**
1. Prompt-level guardrails
2. Schema-level restrictions
3. Runtime approval system
4. Tool-level validation
5. Lifecycle hooks [76]

**GAIA (3 capas):**
1. Unit testing (inner loop)
2. E2E testing (outer loop)
3. Validation gates humanos [26], [36]

**DDC (2 capas):**
1. Automated checks (schema, relationships)
2. Human domain expert review [131]

#### 4.1.4. Arquitecturas de Orquestación
Se identifican tres patrones arquitectónicos dominantes:

**Cadenas Secuenciales:**
- GAIA: plan → actions → validation → close [20]
- SDD: /specify → /plan → /tasks → implementation [104]
- CURRANTE: Specification → Tests → Function [115]

**Sistemas Compuestos:**
- OPENDEV: Session → Agent → Workflow → LLM (4 niveles) [59]
- GABBE: Brain Mode + Loki Swarm Mode (dual-layer) [153]

**Optimización Bi-nivel:**
- MCE: Meta-level (skill evolution) + Base-level (context optimization) [144]

### 4.2. Diferencias Clave

#### 4.2.1. Espectro de Autonomía
Los sistemas se distribuyen en un espectro de autonomía humano-agente:

**Alta Supervisión Humana:**
- **GAIA:** Validation gates obligatorios en PRD, feature-descr, planes [36], [37]
- **DDC:** Curación manual obligatoria, experto de dominio como Information Provider, Entity Author, Validator [139]
- **CURRANTE:** Refinamiento humano de test cases, aprobación implícita antes de code generation [117], [120]

**Supervisión Configurable:**
- **OPENDEV:** Niveles Manual/Semi-Auto/Auto, aprobaciones persistentes [76]
- **SDD Paper:** Revisión humana en cada checkpoint, pero con grados de automatización [104]

**Aspiración a Autonomía Completa:**
- **MCE:** Optimización totalmente agéntica, transición de workflows manuales a sistemas meta- y self-learning [148], [150]
- **GABBE:** Sistema autónomo de tercera generación (aunque detalles de human-in-the-loop no disponibles) [153]

#### 4.2.2. Enfoque en Problema Técnico
Cada sistema prioriza diferentes aspectos del problema:

**Gestión de Contexto:**
- OPENDEV: Ventanas de contexto finitas, context bloat [55]
- MCE: Harnesses manuales con sesgos estructurales [145]
- DDC: Conocimiento tribal no documentado [127]

**Calidad y Trazabilidad:**
- GAIA: Desalineación especificaciones-código [21]
- SDD Paper: Realidad code-centric, drift de requisitos [92]

**Comprensión de Comportamiento:**
- CURRANTE: Comportamiento LLM en procesos estructurados pobremente comprendido [109]

**Arquitectura de Sistema:**
- GABBE: Context decay, non-determinismo, cost asymmetry en MAS [153]

#### 4.2.3. Estrategias de Validación
Las metodologías de validación difieren significativamente:

**TDD Riguroso:**
- GAIA: Double Loop TDD (outer + inner) [1]
- CURRANTE: Workflow TDD de 3 fases [122]

**Defense-in-Depth:**
- OPENDEV: 5 capas independientes de seguridad [76]

**Validación Humana Iterativa:**
- DDC: Correction loops hasta output correcto [130]

**Evaluación Multi-dominio:**
- MCE: 5 dominios (finance, chemistry, medicine, law, AI safety) con métricas específicas por tarea

#### 4.2.4. Modelos LLM y Roles
La especificidad de modelos LLM varía ampliamente:

**Altamente Específicos:**
- MCE: DeepSeek-V3.1, Qwen3-8B, MiniMax M2.1, Llama3.3-70B, Gemma3-4B con roles claramente definidos [152]
- OPENDEV: 5 roles de modelo (Action, Thinking, Critique, Vision, Compact) con fallback chains [73]

**Moderadamente Específicos:**
- CURRANTE: Qwen3-Coder especializado para coding [119]

**Genéricos:**
- GAIA: Referencias genéricas a "agente IA" [32]
- SDD Paper: "GPT-4 o Claude" sin versiones específicas [100]
- DDC: "Large language model agents" sin especificación [127]

**No Disponible:**
- Source 7 (Taylor & Francis): Paywall
- GABBE: Abstract no especifica modelos concretos

### 4.3. Tendencias Emergentes

#### 4.3.1. De Monolítico a Compuesto
Evolución clara desde sistemas monolíticos hacia **compound AI systems**:

**Primera Generación (Implícita):**
- Único LLM para todas las tareas
- Prompt engineering manual

**Segunda Generación:**
- GAIA: Agente único con skills y workflows modulares [18], [19], [20]
- CURRANTE: LLM único con workflow estructurado [115]

**Tercera Generación:**
- OPENDEV: Ensemble de agentes y workflows, cada uno con LLM independiente [70]
- GABBE: Arquitectura dual-layer con 30+ agentes especializados [153]
- MCE: Meta-agente + base-agente con roles diferenciados [144]

#### 4.3.2. Context Engineering como Disciplina
Emergencia de **Context Engineering** como disciplina formal:

**Fase 1 - Reconocimiento del Problema:**
- OPENDEV identifica context bloat como bottleneck [55]
- GAIA implementa scopes y referencias para gestión de contexto [39], [40]

**Fase 2 - Metodologías Sistemáticas:**
- DDC: Metodología cíclica para curación de conocimiento [137]
- OPENDEV: Adaptive Context Compaction con 5 etapas [79]

**Fase 3 - Optimización Meta-nivel:**
- MCE: Framework bi-nivel que co-evoluciona skills y artefactos de contexto [144]

#### 4.3.3. Validación Programática vs. Humana
Tensión emergente entre validación automatizada y juicio humano:

**Tendencia hacia Automatización:**
- MCE: Optimización totalmente agéntica [148]
- OPENDEV: Self-healing indexes, doom-loop detection automática [78], [77]

**Resistencia y Complementariedad:**
- GAIA: Validation gates explícitos obligatorios [36]
- DDC: Experto de dominio esencial para corrección de fabricaciones [140]
- SDD Paper: "Specs requieren revisión cuidadosa como código" [108]

**Síntesis Emergente:**
- Automatización para checks mecánicos (schema, syntax, patterns)
- Humanos para juicio de dominio, corrección y decisiones estratégicas
- OPENDEV: Niveles configurables Manual/Semi-Auto/Auto [76]

#### 4.3.4. Spec-Driven como Lingua Franca
SDD emerge como **lenguaje común** entre sistemas:

**Convergencia Terminológica:**
- GAIA: "Spec-Driven Development" explícito [1]
- OPENDEV: "Scaffolding" y "harness" como conceptos arquitectónicos [52]
- SDD Paper: Formalización de niveles de rigor (spec-first, spec-anchored, spec-as-source)
- CURRANTE: Workflow TDD como instancia de SDD [122]
- GABBE: "Spec-driven development life cycles" [153]

**Beneficios Identificados:**
- Descomposición modular alineada con context windows [100]
- Contratos ejecutables no ambiguos [94]
- Trazabilidad desde intención hasta implementación [22]
- Paralelización de trabajo entre agentes [105]

#### 4.3.5. Active Inference y Neurocognición
GABBE introduce **principios neurocognitivos** en arquitectura de agentes:

**Active Inference:**
- Framework neurocientífico para toma de decisiones
- Minimización de riesgo de proyecto en Brain Mode [153]

**Implicaciones:**
- Potencial nueva dirección: arquitecturas inspiradas en neurociencia
- Contraste con enfoques puramente ingenieriles (OPENDEV, GAIA)
- Conexión con dual-memory architecture de OPENDEV (episodic + working memory) [81]

⚠️ **Limitación:** Sin acceso al paper completo de GABBE, no es posible evaluar profundidad de implementación de Active Inference.

---

## 5. Conclusión de Composición

### 5.1. Síntesis Arquitectónica: Sistema de Harness Funcional Completo

Basándose en los ocho documentos analizados, es posible sintetizar una **arquitectura de referencia** para un sistema de harness engineering completo que integre los componentes más robustos de cada enfoque:

#### 5.1.1. Capa de Especificación (Spec Layer)
**Componentes:**
- **Spec Repository** (GAIA Rules + SDD Paper): Especificaciones versionadas como fuente de verdad [18], [91]
- **Spec Validator** (SDD Paper): Validación de specs antes de implementación [101]
- **Spec-to-Context Transformer** (SDD Paper + MCE): Conversión de specs en contexto optimizado para LLMs [100], [144]

**Interacciones:**
- Alimenta Context Management Layer con conocimiento estructurado
- Recibe feedback de Validation Layer para refinamiento iterativo

#### 5.1.2. Capa de Gestión de Contexto (Context Management Layer)
**Componentes:**
- **Adaptive Context Compactor** (OPENDEV): Pipeline de 5 etapas para gestión de presión de contexto [79]
- **Typed Entity Store** (DDC): Base de conocimiento con entidades tipadas versionadas [132], [133]
- **Dual-Memory Architecture** (OPENDEV): Memoria episódica (long-range) + memoria de trabajo (short-range) [81]
- **Meta-Context Optimizer** (MCE): Optimización bi-nivel de skills y artefactos de contexto [144]

**Interacciones:**
- Recibe specs de Spec Layer
- Proporciona contexto optimizado a Orchestration Layer
- Aprende de feedback de Validation Layer (MCE approach)

#### 5.1.3. Capa de Orquestación (Orchestration Layer)
**Componentes:**
- **Brain Mode Meta-Agent** (GABBE): Active Inference para minimización de riesgo y decisiones estratégicas [153]
- **Workflow Engine** (GAIA): Ejecución de protocolos spec-driven determinísticos [20]
- **Agent Swarm** (GABBE + OPENDEV): 30+ agentes especializados con roles diferenciados [153], [70]
- **LLM Router** (OPENDEV + GABBE): Enrutamiento cost-effective a modelos especializados [73], [153]

**Interacciones:**
- Recibe contexto optimizado de Context Management Layer
- Coordina ejecución de agentes especializados
- Envía outputs a Validation Layer

#### 5.1.4. Capa de Validación (Validation Layer)
**Componentes:**
- **Defense-in-Depth Validator** (OPENDEV): 5 capas de seguridad independientes [76]
- **Double Loop TDD** (GAIA): Outer loop (E2E) + Inner loop (Unit/Integration) [1]
- **Human Validation Gates** (GAIA + DDC): Checkpoints obligatorios para decisiones críticas [36], [129]
- **Doom-Loop Detector** (OPENDEV): Detección de patrones repetitivos fallidos [77]

**Interacciones:**
- Valida outputs de Orchestration Layer
- Proporciona feedback a Context Management Layer para mejora continua
- Dispara correction loops cuando necesario (DDC approach) [130]

#### 5.1.5. Capa de Interacción Humana (Human Interaction Layer)
**Componentes:**
- **Configurable Autonomy Controller** (OPENDEV): Niveles Manual/Semi-Auto/Auto [76]
- **Structured Question Interface** (OPENDEV ask_user): Preguntas multi-choice para clarificación [65]
- **Plan Review Interface** (OPENDEV present_plan): Revisión y aprobación de planes [65]
- **Domain Expert Curation Interface** (DDC): Curación de conocimiento tribal [139]

**Interacciones:**
- Recibe solicitudes de clarificación de todas las capas
- Proporciona aprobaciones y refinamientos a Orchestration Layer
- Cura conocimiento de dominio para Context Management Layer

### 5.2. Flujo de Datos e Interacciones

```
┌─────────────────────────────────────────────────────────────────┐
│                    HUMAN INTERACTION LAYER                      │
│  [Autonomy Controller] [Question Interface] [Plan Review]       │
└────────────┬────────────────────────────────────┬───────────────┘
             │                                    │
             ▼                                    ▼
┌─────────────────────────┐          ┌─────────────────────────────┐
│   SPEC LAYER            │          │  VALIDATION LAYER           │
│  [Spec Repository]      │◄─────────┤  [Defense-in-Depth]         │
│  [Spec Validator]       │          │  [Double Loop TDD]          │
│  [Spec-to-Context]      │          │  [Human Gates]              │
└────────────┬────────────┘          │  [Doom-Loop Detector]       │
             │                       └─────────────▲───────────────┘
             ▼                                     │
┌─────────────────────────────────────────────────┼───────────────┐
│           CONTEXT MANAGEMENT LAYER              │               │
│  [Adaptive Compactor] [Typed Entity Store]      │               │
│  [Dual-Memory] [Meta-Context Optimizer]         │               │
└────────────┬────────────────────────────────────┼───────────────┘
             │                                     │
             ▼                                     │
┌─────────────────────────────────────────────────┼───────────────┐
│           ORCHESTRATION LAYER                   │               │
│  [Brain Mode Meta-Agent] [Workflow Engine]      │               │
│  [Agent Swarm] [LLM Router]                     │               │
└─────────────────────────────────────────────────┴───────────────┘
```

**Flujo Principal:**
1. **Especificación:** Humano define specs → Spec Layer valida y transforma
2. **Contextualización:** Spec Layer alimenta Context Management Layer → contexto optimizado
3. **Orquestación:** Brain Mode decide estrategia → Workflow Engine coordina Agent Swarm → LLM Router asigna tareas
4. **Validación:** Outputs pasan por Defense-in-Depth → Double Loop TDD → Human Gates cuando necesario
5. **Feedback:** Validation Layer informa Context Management Layer para mejora continua
6. **Iteración:** Correction loops hasta satisfacer criterios de validación

### 5.3. Brechas de Información Identificadas

#### 5.3.1. Brechas Críticas

**Source 7 (Taylor & Francis) - PAYWALL:**
- **Impacto:** Imposible evaluar contribución potencial al análisis comparativo
- **Mitigación:** Requiere acceso institucional o compra del artículo

**GABBE (Source 8) - Abstract Limitado:**
- **Detalles técnicos faltantes:**
  - Estructura específica de 4-layer memory hierarchy
  - Implementación de Active Inference en Brain Mode
  - Detalles de "Another Lethal Trifecta" mitigation strategy
  - Modelos LLM específicos utilizados
  - Métricas cuantitativas de performance
  - Nivel de human-in-the-loop
- **Impacto:** Arquitectura neurocognitiva prometedora pero sin suficiente detalle para implementación
- **Mitigación:** Requiere acceso al paper completo en TechRxiv

#### 5.3.2. Brechas Moderadas

**Métricas de Performance:**
- **DDC:** No proporciona benchmarks cuantitativos detallados más allá de convergencia de entidades
- **MCE:** Métricas de mejora relativa (5.6-53.8%) pero sin detalles de implementación práctica
- **GAIA:** Métricas limitadas a límites de archivo y velocidad de tests

**Integración Práctica:**
- **Todos los documentos:** Falta documentación sobre integración entre estos enfoques en sistemas reales de producción
- **Ejemplo:** ¿Cómo integrar OPENDEV's ACC con DDC's typed entities en un sistema unificado?

**Escalabilidad:**
- **Todos los documentos:** Limitada evidencia empírica sobre comportamiento en proyectos enterprise de gran escala (>100K LOC, >10 desarrolladores)

#### 5.3.3. Brechas Menores

**Lenguajes de Programación:**
- DDC, MCE, GABBE: No especifican lenguajes soportados
- Impacto limitado: Metodologías son language-agnostic en principio

**Stack de Desarrollo:**
- DDC: No especifica herramientas concretas
- GABBE: Abstract no detalla stack técnico

### 5.4. Recomendaciones para Implementación

#### 5.4.1. Enfoque Incremental por Capas

**Fase 1 - Fundación (3-6 meses):**
- Implementar Spec Layer básico con versionado (GAIA approach)
- Establecer Typed Entity Store (DDC approach)
- Configurar Double Loop TDD (GAIA approach)

**Fase 2 - Optimización de Contexto (6-9 meses):**
- Integrar Adaptive Context Compaction (OPENDEV approach)
- Implementar Dual-Memory Architecture (OPENDEV approach)
- Establecer métricas de consumo de contexto

**Fase 3 - Orquestación Avanzada (9-12 meses):**
- Desarrollar Agent Swarm con roles especializados (GABBE + OPENDEV)
- Implementar LLM Router cost-effective (OPENDEV + GABBE)
- Establecer Workflow Engine determinístico (GAIA approach)

**Fase 4 - Meta-Optimización (12-18 meses):**
- Integrar Meta-Context Optimizer (MCE approach)
- Implementar Brain Mode con Active Inference (GABBE approach)
- Establecer feedback loops para mejora continua

#### 5.4.2. Decisiones Arquitectónicas Clave

**Nivel de Autonomía:**
- **Recomendación:** Comenzar con OPENDEV's configurable autonomy (Manual/Semi-Auto/Auto)
- **Rationale:** Permite ajuste gradual según confianza y dominio
- **Evolución:** Migrar hacia mayor autonomía (MCE approach) solo después de validación exhaustiva

**Estrategia de Validación:**
- **Recomendación:** Implementar OPENDEV's defense-in-depth (5 capas) + GAIA's validation gates
- **Rationale:** Balance entre seguridad y eficiencia
- **Crítico:** Human gates obligatorios para decisiones arquitectónicas y cambios de alto riesgo

**Gestión de Contexto:**
- **Recomendación:** Híbrido OPENDEV (compactación) + DDC (entidades tipadas) + MCE (meta-optimización)
- **Rationale:** Compactación para eficiencia inmediata, entidades para conocimiento estructurado, meta-optimización para mejora continua
- **Prioridad:** Implementar compactación primero (impacto inmediato 54% reducción)

**Orquestación:**
- **Recomendación:** Comenzar con GAIA's workflow engine (determinístico), evolucionar hacia GABBE's dual-layer
- **Rationale:** Workflows determinísticos proporcionan predictibilidad, dual-layer permite escalabilidad
- **Crítico:** LLM routing cost-effective desde el inicio (OPENDEV approach)

---

## 6. Mapa de Dependencias Tecnológicas

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ECOSYSTEM DE HARNESS ENGINEERING                    │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                              CAPA DE MODELOS LLM                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │   Anthropic  │  │    OpenAI    │  │  DeepSeek    │  │   MiniMax    │  │
│  │ Claude 3.5   │  │   GPT-4o     │  │   V3.1       │  │    M2.1      │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  │
│         │                 │                 │                 │           │
│  ┌──────┴─────────────────┴─────────────────┴─────────────────┴───────┐  │
│  │              LLM ROUTER (OPENDEV + GABBE)                           │  │
│  │  [Action] [Thinking] [Critique] [Vision] [Compact]                 │  │
│  │  Cost-effective routing • Fallback chains • Provider abstraction   │  │
│  └──────────────────────────────┬──────────────────────────────────────┘  │
└─────────────────────────────────┼──────────────────────────────────────────┘
                                  │
┌─────────────────────────────────┼──────────────────────────────────────────┐
│                    CAPA DE ORQUESTACIÓN Y AGENTES                          │
├─────────────────────────────────┼──────────────────────────────────────────┤
│                                 ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │              BRAIN MODE META-AGENT (GABBE)                          │  │
│  │  Active Inference • Risk Minimization • Strategic Decisions        │  │
│  └────────────────────────────┬────────────────────────────────────────┘  │
│                               │                                            │
│         ┌─────────────────────┼─────────────────────┐                     │
│         ▼                     ▼                     ▼                     │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐              │
│  │  WORKFLOW   │      │    AGENT    │      │  SUBAGENT   │              │
│  │   ENGINE    │◄────►│    SWARM    │◄────►│ DELEGATION  │              │
│  │   (GAIA)    │      │ (30+ roles) │      │  (OPENDEV)  │              │
│  └──────┬──────┘      └──────┬──────┘      └──────┬──────┘              │
│         │                    │                    │                       │
│         │  ┌─────────────────┴────────────────┐   │                       │
│         └─►│   EXTENDED REACT PIPELINE        │◄──┘                       │
│            │  Pre-check • Thinking • Critique │                           │
│            │  Action • Tool Exec • Post-proc  │                           │
│            └─────────────┬────────────────────┘                           │
└──────────────────────────┼───────────────────────────────────────────────┘
                           │
┌──────────────────────────┼───────────────────────────────────────────────┐
│                  CAPA DE GESTIÓN DE CONTEXTO                              │
├──────────────────────────┼───────────────────────────────────────────────┤
│                          ▼                                                │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │         META-CONTEXT OPTIMIZER (MCE)                               │  │
│  │  Bi-level optimization • Skill evolution • Agentic crossover       │  │
│  └────────────────────────────┬───────────────────────────────────────┘  │
│                               │                                           │
│         ┌─────────────────────┼─────────────────────┐                    │
│         ▼                     ▼                     ▼                    │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐             │
│  │  ADAPTIVE   │      │    DUAL     │      │   TYPED     │             │
│  │  CONTEXT    │◄────►│   MEMORY    │◄────►│   ENTITY    │             │
│  │ COMPACTION  │      │ ARCHITECTURE│      │    STORE    │             │
│  │  (OPENDEV)  │      │  (OPENDEV)  │      │    (DDC)    │             │
│  └──────┬──────┘      └──────┬──────┘      └──────┬──────┘             │
│         │                    │                    │                      │
│         │  5-stage pipeline  │  Episodic+Working  │  YAML frontmatter   │
│         │  54% reduction     │  memory separation │  Versioned entities │
│         │                    │                    │                      │
│  ┌──────┴────────────────────┴────────────────────┴──────┐              │
│  │         CONTEXT INJECTION & MAINTENANCE                │              │
│  │  Per-tool summarization • System reminders             │              │
│  │  Dynamic prompt construction • Lazy tool discovery     │              │
│  └────────────────────────────┬───────────────────────────┘              │
└─────────────────────────────────┼──────────────────────────────────────────┘
                                  │
┌─────────────────────────────────┼──────────────────────────────────────────┐
│                      CAPA DE ESPECIFICACIÓN                                │
├─────────────────────────────────┼──────────────────────────────────────────┤
│                                 ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                    SPEC REPOSITORY (GAIA + SDD)                     │  │
│  │  Rules (Constitution) • Skills (Library) • Workflows (Protocols)   │  │
│  └────────────────────────────┬────────────────────────────────────────┘  │
│                               │                                            │
│         ┌─────────────────────┼─────────────────────┐                     │
│         ▼                     ▼                     ▼                     │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐              │
│  │    SPEC     │      │    SPEC     │      │  SPEC-TO-   │              │
│  │  VALIDATOR  │      │  VERSIONING │      │  CONTEXT    │              │
│  │    (SDD)    │      │    (Git)    │      │ TRANSFORMER │              │
│  └──────┬──────┘      └──────┬──────┘      └──────┬──────┘              │
│         │                    │                    │                       │
│         │  BDD/TDD/Contract  │  Global/Workspace  │  Super-prompts       │
│         │  testing           │  scopes            │  Modular decomp      │
│         │                    │                    │                       │
└─────────┴────────────────────┴────────────────────┴───────────────────────┘
                                  │
┌─────────────────────────────────┼──────────────────────────────────────────┐
│                       CAPA DE VALIDACIÓN                                   │
├─────────────────────────────────┼──────────────────────────────────────────┤
│                                 ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │           DEFENSE-IN-DEPTH VALIDATOR (OPENDEV)                      │  │
│  │  Layer 1: Prompt guardrails • Layer 2: Schema restrictions         │  │
│  │  Layer 3: Runtime approval • Layer 4: Tool validation              │  │
│  │  Layer 5: Lifecycle hooks                                          │  │
│  └────────────────────────────┬────────────────────────────────────────┘  │
│                               │                                            │
│         ┌─────────────────────┼─────────────────────┐                     │
│         ▼                     ▼                     ▼                     │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐              │
│  │   DOUBLE    │      │    HUMAN    │      │  DOOM-LOOP  │              │
│  │   LOOP TDD  │◄────►│ VALIDATION  │◄────►│  DETECTOR   │              │
│  │   (GAIA)    │      │    GATES    │      │  (OPENDEV)  │              │
│  └──────┬──────┘      └──────┬──────┘      └──────┬──────┘              │
│         │                    │                    │                       │
│  Outer: E2E (Playwright)     │  Explicit gates    │  Fingerprinting      │
│  Inner: Unit (Pytest/Vitest) │  for critical      │  Recurrence tracking │
│                               │  decisions         │                       │
└───────────────────────────────┴────────────────────┴───────────────────────┘
                                  │
┌─────────────────────────────────┼──────────────────────────────────────────┐
│                  CAPA DE INTERACCIÓN HUMANA                                │
├─────────────────────────────────┼──────────────────────────────────────────┤
│                                 ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │        CONFIGURABLE AUTONOMY CONTROLLER (OPENDEV)                   │  │
│  │  Manual • Semi-Auto • Auto levels                                   │  │
│  │  Persistent permissions • Fatigue prevention                        │  │
│  └────────────────────────────┬────────────────────────────────────────┘  │
│                               │                                            │
│         ┌─────────────────────┼─────────────────────┐                     │
│         ▼                     ▼                     ▼                     │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐              │
│  │  STRUCTURED │      │    PLAN     │      │   DOMAIN    │              │
│  │  QUESTION   │      │   REVIEW    │      │   EXPERT    │              │
│  │  INTERFACE  │      │  INTERFACE  │      │  CURATION   │              │
│  │  (ask_user) │      │(present_plan)│     │    (DDC)    │              │
│  └─────────────┘      └─────────────┘      └─────────────┘              │
│                                                                            │
│  Multi-choice questions • Plan approval • Tribal knowledge capture        │
│  Interrupt capability (Ctrl+C) • Correction loops                         │
└────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                         CAPA DE INFRAESTRUCTURA                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │     Git      │  │    Docker    │  │  Playwright  │  │     LSP      │  │
│  │  Versioning  │  │  Containers  │  │   Browser    │  │   Servers    │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │   Ripgrep    │  │   AST-grep   │  │  DuckDuckGo  │  │    Kanban    │  │
│  │    Search    │  │   Structural │  │  Web Search  │  │     Tasks    │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │   FastAPI    │  │    React     │  │   Textual    │  │  WebSockets  │  │
│  │   Backend    │  │   Frontend   │  │     TUI      │  │   Real-time  │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘  │
└─────────────────────────────────────────────────────────────────────────────┘

LEYENDA:
─────►  Flujo de datos principal
◄────►  Interacción bidireccional
┌────┐  Componente arquitectónico
│    │  Capa del sistema
```

### Dependencias Críticas Identificadas:

**1. LLM Router → Todos los Agentes**
- Todos los componentes de orquestación dependen del LLM Router para asignación cost-effective de tareas
- Fallo del router impacta todo el sistema
- **Mitigación:** Fallback chains (OPENDEV approach) [73]

**2. Context Management → Orchestration**
- Calidad de contexto determina calidad de decisiones de agentes
- Context bloat degrada performance
- **Mitigación:** Adaptive Context Compaction (54% reducción) [79]

**3. Spec Repository → Context Management**
- Specs son fuente de verdad para contexto
- Specs desactualizadas o ambiguas propagan errores
- **Mitigación:** Spec Validator + Versioning (GAIA + SDD approach) [18], [101]

**4. Validation Layer → Todas las Capas**
- Validación proporciona feedback para mejora continua
- Validación débil permite propagación de errores
- **Mitigación:** Defense-in-Depth (5 capas independientes) [76]

**5. Human Interaction → Decisiones Críticas**
- Decisiones arquitectónicas y de alto riesgo requieren juicio humano
- Automatización prematura introduce riesgos
- **Mitigación:** Configurable autonomy + Validation gates obligatorios [76], [36]

---

## Referencias

[1] GAIA Framework (RUA/UA), "Spec-Driven Development with GAIA: Key Aspects and Technical Details"

[2] OPENDEV (arXiv 2603.05344), "OPENDEV: A Comprehensive Overview of AI Coding Agent Architecture"

[3] Spec-Driven Development Paper (arXiv 2602.00180), "Spec-Driven Development: Key Information"

[4] CURRANTE (arXiv 2601.03878), "Understanding Specification-Driven Code Generation with LLMs"

[5] DDC - Demand-Driven Context (arXiv 2603.14057), "Demand-Driven Context (DDC) Methodology and Architecture"

[6] MCE - Meta Context Engineering (arXiv 2601.21557), "Meta Context Engineering: A Bi-Level Optimization Framework"

[7] GAIA Framework, "Control (Review Protocol): Validation Gate explícito"

[8] DDC Paper, "In the base DDC methodology, the human performs three roles"

[9] MCE Paper, "MCE represents a transition from manually crafted CE workflows to fully agentic meta- and self-learning systems"

[10] GAIA Framework, "Los worflows definen 'protocolos de ejecución'"

[11] OPENDEV, "A central design principle of OPENDEV is that it is a compound AI system"

[12] MCE Paper, "We formalize Meta Context Engineering (MCE) as a bi-level optimization framework"

[13] OPENDEV, "implements Adaptive Context Compaction (ACC)"

[14] DDC Paper, "This curated knowledge is structured using a typed entity meta-model"

[15] OPENDEV, "We resolve this tension through a dual-memory architecture"

[16] MCE Paper, "A base-level agent executes these skills, learns from training rollouts, and optimizes context as flexible files and code"

[17] OPENDEV, "Layer 1: Prompt-Level Guardrails... Layer 5: Lifecycle Hooks"

[18] GAIA Framework, "Rules: Act as the project's 'Constitution'"

[19] GAIA Framework, "Skills: Act as a 'reference library'"

[20] GAIA Framework, "Workflows: Define execution protocols"

[21] GAIA Framework, "desalineación del estado mental del proyecto (especificaciones vs. código)"

[22] GAIA Framework, "minimizar improvisación, forzar validación (mediante tests y revisiones) y preservar trazabilidad"

[23] GAIA Framework, "Cambios a artefactos de proyecto SDD (reglas, arquitectura) se consideran de alto riesgo"

[24] GAIA Framework, "framework FastAPI de python"

[25] GAIA Framework, "'src/**/*.ts'"

[26] GAIA Framework, "Herramienta: Playwright (E2E)"

[27] GAIA Framework, "Pytest (backend)"

[28] GAIA Framework, "DB real efímera (Docker)"

[29] GAIA Framework, Git workflow patterns

[30] GAIA Framework, Git commit and push

[31] GAIA Framework, "he creado un notebookLM con información detallada sobre Playwright"

[32] GAIA Framework, "el agente (IA) selecciona skills basándose en la intención del usuario"

[33] GAIA Framework, "`def test_post_news_returns_400_when_title_missing(client)`"

[34] GAIA Framework, "Traducir escenarios críticos (Happy Path) a Playwright"

[35] GAIA Framework, "RED (falla primero)"

[36] GAIA Framework, "el agente debe pedir que la persona desarrolladora valide el PRD y feature-descr.md"

[37] GAIA Framework, "el agente debe pedir que la persona desarrolladora valide cada plan generado"

[38] GAIA Framework, "Regla de bloqueo: una feature no puede cerrarse si la auditoría marca riesgos Críticos o Altos"

[39] GAIA Framework, "Global: se aplica a todos tus proyectos/workspaces"

[40] GAIA Framework, "Las reglas pueden usar '@mentions' para apuntar a documentos fuente"

[41] GAIA Framework, "Rules should keep each file within platform limits (e.g., <= 12,000 characters)"

[42] GAIA Framework, "E2E tests are fewer but critical, balancing cost and speed"

[43] GAIA Framework, "API tests verify correct HTTP status codes (e.g., 400 for missing titles)"

[44] GAIA Framework, "Examples include `/plan-feature-descr-from-user-conversation` and `/execute-plan`"

[45] GAIA Framework, "the system responds to intentions (e.g., 'implement a feature', 'fix a bug')"

[46] GAIA Framework, "The agent must stop and ask for clarification when inputs are missing or unclear"

[47] GAIA Framework, "if info is missing for a step, the workflow should say"

[48] GAIA Framework, "If something can be interpreted in two ways, require a question"

[49] GAIA Framework, "The agent may propose the correct location for an artifact"

[50] GAIA Framework, "Rules can be activated manually"

[51] GAIA Framework, "The system has explicit stop conditions"

[52] OPENDEV, "the harness, which orchestrates tool dispatch, context management, and safety enforcement at runtime"

[53] OPENDEV, "scaffolding assembles the agent before the first prompt, while the harness orchestrates"

[54] OPENDEV, "Context engineering is treated as a first-class concern"

[55] OPENDEV, "managing finite context windows over sessions that routinely exceed the model's token budget"

[56] OPENDEV, "agentic coding assistants need to reason about complex tasks"

[57] OPENDEV, "we present OPENDEV, an open-source, command-line coding agent written in Rust"

[58] OPENDEV, Supported languages for LSP integration

[59] OPENDEV, "Frontends: Textual (TUI) and FastAPI + WebSockets (Web UI)"

[60] OPENDEV, "Browser Engine: Playwright"

[61] OPENDEV, "used by Crawl4AI for `fetch_url` and `capture_web_screenshot`"

[62] OPENDEV, "Ripgrep (for regex-based content search)"

[63] OPENDEV, "DuckDuckGo (for `web_search`)"

[64] OPENDEV, "LSP Integration: Standard language servers"

[65] OPENDEV, "ask_user: Structured multi-choice questions"

[66] OPENDEV, "Git (for shadow git snapshots"

[67] OPENDEV, "`git` commands in `main-git-workflow.md`"

[68] OPENDEV, "`subprocess.Popen` (for foreground commands)"

[69] OPENDEV, "`fcntl.flock` (for file locking)"

[70] OPENDEV, "A central design principle of OPENDEV is that it is a compound AI system"

[71] OPENDEV, "not a single monolithic LLM, but a structured ensemble of agents and workflows"

[72] OPENDEV, "per-workflow LLM binding architecture"

[73] OPENDEV, "Five distinct model roles"

[74] OPENDEV, "Provider-specific guidance for Anthropic and OpenAI models"

[75] OPENDEV, "with a generic fallback for unknown providers"

[76] OPENDEV, "defense-in-depth safety architecture with five independent layers"

[77] OPENDEV, "Doom-loop detection"

[78] OPENDEV, "Self-healing indexes"

[79] OPENDEV, "Adaptive Context Compaction (ACC)"

[80] OPENDEV, "five-stage pipeline of progressively aggressive reduction strategies"

[81] OPENDEV, "Dual-memory Architecture for Bounded Thinking"

[82] OPENDEV, "Tool Result Optimization"

[83] OPENDEV, "Dynamic System Prompt Construction"

[84] OPENDEV, "Lazy Tool Discovery"

[85] OPENDEV, "human-agent collaboration"

[86] OPENDEV, "Concurrent Sessions"

[87] OPENDEV, "Extended ReAct Execution Pipeline"

[88] OPENDEV, "The loop executes six phases per iteration"

[89] OPENDEV, "Dual-path Dispatch"

[90] OPENDEV, "Ctrl+C - Interrupt agent execution"

[91] SDD Paper, "Spec-driven development (SDD) inverts the traditional workflow"

[92] SDD Paper, "realidad code-centric"

[93] SDD Paper, "AI models are excellent at pattern completion but poor at mind reading"

[94] SDD Paper, "'vibe coding' where loose prompts result in inconsistent or erroneous outputs"

[95] SDD Paper, "BDD frameworks like Cucumber support Ruby, Java, and JavaScript"

[96] SDD Paper, "C code is generated from Simulink models"

[97] SDD Paper, "TDD Frameworks: RSpec, JUnit, pytest"

[98] SDD Paper, "API Specification Tools: OpenAPI/Swagger"

[99] SDD Paper, "GraphQL SDL, Protocol Buffers, AsyncAPI"

[100] SDD Paper, "Large language models like GPT-4 or Claude"

[101] SDD Paper, "validate phase"

[102] SDD Paper, "the plan provides crucial context"

[103] SDD Paper, "75% reduction in integration cycle time"

[104] SDD Paper, "The workflow follows four explicit phases"

[105] SDD Paper, "parallel agent execution"

[106] SDD Paper, "Human oversight is still required"

[107] SDD Paper, "'self-spec' methods"

[108] SDD Paper, "Specifications require the same careful review as code"

[109] CURRANTE, "their behavior in structured, specification-driven processes remains poorly understood"

[110] CURRANTE, "SDD is presented as a more abstract, structured approach"

[111] CURRANTE, "gap in understanding the user's role"

[112] CURRANTE, "The only programming language explicitly mentioned is Python"

[113] CURRANTE, "generating a runnable unit-test file"

[114] CURRANTE, "`self.assertEqual` in the example"

[115] CURRANTE, "CURRANTE: A Visual Studio Code extension"

[116] CURRANTE, "Visual Studio Code (VS Code)"

[117] CURRANTE, "TOML format"

[118] CURRANTE, "designed to be human-readable and easy to edit"

[119] CURRANTE, "Qwen3-Coder [17]"

[120] CURRANTE, "The LLM generates the code based on the refined test suite"

[121] CURRANTE, "prompt templates and key parameters will be fixed"

[122] CURRANTE, "workflow Test-Driven Development (TDD)"

[123] CURRANTE, "PassAll, PassRate, TestCoverage, TestDiversity"

[124] CURRANTE, "token counts for each API call"

[125] DDC Paper, "The document primarily focuses on Context Engineering"

[126] DDC Paper, "DDC as a methodology for context engineering"

[127] DDC Paper, "consistent failure of large language model agents on enterprise-specific tasks"

[128] DDC Paper, "Modern LLMs"

[129] DDC Paper, "Human Validates Output"

[130] DDC Paper, "Correction Loop"

[131] DDC Paper, "Automated checks validate schema, relationships, and naming conventions"

[132] DDC Paper, "typed entity meta-model"

[133] DDC Paper, "entidades tipadas almacenadas como archivos markdown versionados con YAML frontmatter"

[134] DDC Paper, "Retrieval-Augmented Generation (RAG)"

[135] DDC Paper, "New entities created per cycle"

[136] DDC Paper, "Correction loops"

[137] DDC Paper, "DDC is a cyclic methodology"

[138] DDC Paper, "analogous to Test-Driven Development (TDD)"

[139] DDC Paper, "Information Provider, Entity Author, Validator"

[140] DDC Paper, "correcting agent fabrications or misunderstandings"

[141] DDC Paper, "semi-automated curation"

[142] DDC Paper, "answers that exist only in human heads"

[143] MCE Paper, "Context Engineering (CE)"

[144] MCE Paper, "Meta Context Engineering (MCE)"

[145] MCE Paper, "manually crafted agentic harnesses"

[146] MCE Paper, "impose structural biases"

[147] MCE Paper, "The only programming language explicitly mentioned is Python"

[148] MCE Paper, "LangChain and LangChain DeepAgents"

[149] MCE Paper, "iterativo con fases de skill evolution, context optimization, evaluation"

[150] MCE Paper, "Claude Agent SDK"

[151] MCE Paper, Claude Agent SDK reference

[152] MCE Paper, "DeepSeek-V3.1, Qwen3-8B, MiniMax M2.1"

[153] GABBE (TechRxiv), Abstract del paper "GABBE: A Neurocognitive Swarm Architecture for Agentic AI Software Engineering"

---

**Fin del Informe**

*Este informe ha sido generado mediante análisis exhaustivo de 8 fuentes documentales sobre arquitecturas de Harness Engineering con IA. Las brechas de información identificadas (Source 7 - paywall, Source 8 - abstract limitado) requieren acceso adicional para completar el análisis comparativo.*