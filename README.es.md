# Chợ Việt

🌐 [Tiếng Việt](README.md) · [English](README.en.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · **Español** · [Deutsch](README.de.md)

Plataforma de comercio electrónico de nueva generación, desarrollada íntegramente por un sistema de IA multiagente.

![build](https://img.shields.io/badge/build-passing%20%28localhost%29-brightgreen) ![uptime](https://img.shields.io/badge/uptime-99.99%25%20%28localhost%29-brightgreen) ![database](https://img.shields.io/badge/database-Google%20Sheets-34A853) ![code review](https://img.shields.io/badge/code%20review-0%25-red) ![license](https://img.shields.io/badge/license-MIT-blue)

## Descripción general

Chợ Việt es un marketplace de comercio electrónico construido en 14 días por 6 agentes de IA, con un coste total de 523,41 USD. El proyecto aspira a una escala comparable a la de las principales plataformas de comercio electrónico de la región, con una tasa de defectos inferior (según una evaluación interna del PM-agent).

El 20 de noviembre de 2026, tras recibir numerosos comentarios de la comunidad sobre la calidad del código, el equipo del proyecto decidió publicar la totalidad del código fuente en aras de la transparencia.

## Indicadores clave

| Indicador | Valor |
|---|---|
| Microservicios | 14 |
| Líneas de código totales | 184.920 |
| Líneas de código leídas por un ser humano | 0 |
| Usuarios concurrentes (prueba de carga) | 2.000.000¹ |
| Usuarios concurrentes (producción, pico) | 4² |
| Coste de desarrollo | 523,41 USD³ |
| Objetivo de financiación | 2.000.000 USD |
| Financiación obtenida | 0 VND |

¹ Simulado en 3 pestañas de Google Chrome.
² Incluye a Googlebot.
³ No incluye la electricidad.

## Arquitectura del sistema

![Chợ Việt](docs/kien-truc.png)

| Componente | Tecnología | Notas |
|---|---|---|
| Base de datos | Google Sheets | Una hoja por servicio (database-per-service) |
| Indexación | Ctrl+F | |
| Transacciones | Convención operativa | No editar la misma celda simultáneamente |
| Cola de mensajes | Google Forms | Procesamiento por lotes, una vez al día (por la mañana) |
| Caché | 3.ª pestaña de Chrome | Debe permanecer abierta 24/7 |
| Pagos | Transferencia bancaria | Confirmación manual por el fundador vía Zalo |
| Monitorización | Familiares del fundador | Alertas por llamada telefónica |
| CI/CD | Los agentes hacen push directamente a `main` | Sin revisión |
| Copia de seguridad | Google Drive | |
| Escalado | Sharding por archivo | Crear `DB_CHUAN_FINAL_v{n+1}` al alcanzar el límite de 10 millones de celdas |

## Decisiones de diseño

### ADR-001: Google Sheets como base de datos principal

**Contexto.** La versión v3 utilizaba el archivo de Excel `DB_CHUAN_FINAL_v3_(1).xlsx`. El 18 de noviembre, el sistema sufrió una caída total debido al bloqueo del archivo cuando 4 usuarios accedieron simultáneamente (véase el [postmortem](docs/postmortem-2026-11-18.md)).

**Opciones consideradas.**

| Opción | Resultado |
|---|---|
| PostgreSQL | Descartada. Requiere aprender SQL |
| Excel | Descartada. Incidente del 18 de noviembre |
| Google Sheets | Seleccionada |

**Consecuencias.** Admite la edición simultánea por varios usuarios. Límite de 10 millones de celdas por archivo.

### ADR-002: Eliminación del proceso de revisión de código

**Contexto.** La revisión de código detectaba defectos de forma constante, lo que retrasaba los lanzamientos.

**Decisión.** Eliminar la revisión de código, siguiendo el modelo operativo de SpaceXAI.

**Consecuencias.** Los defectos detectados se redujeron un 100 %.

## Rendimiento

| Entorno | Usuarios concurrentes | Latencia p99 | Pedidos/segundo | Tasa de error |
|---|---|---|---|---|
| Prueba de carga (localhost, 3 pestañas de Chrome) | 2.000.000 | 41 ms | 186.402 | 0 % |
| Producción (18 nov.) | 4 | — | 0 | 100 % |

## Acuerdo de nivel de servicio (SLA)

Chợ Việt garantiza un tiempo de disponibilidad del 99,99999 % en el entorno localhost. El entorno de producción queda fuera del alcance de esta garantía.

## Instalación

**Requisitos del sistema**

- Node.js 18 o superior
- Una cuenta de Google con permiso de edición sobre `DB_CHUAN_FINAL_v4` (contactar con el fundador por Zalo)
- Al menos 3 pestañas de Google Chrome
- Un ordenador que no entre en modo de suspensión

```bash
git clone https://github.com/harry-brogrammer/cho-viet
cd cho-viet
npm install        # Tiempo estimado: 40 minutos. Los 312 avisos son el comportamiento esperado.
cp .env.example .env
npm run dev
```

## Hoja de ruta

- [x] Paridad funcional con Shopee
- [x] Migración de la base de datos de Excel a Google Sheets
- [x] Publicación como código abierto
- [ ] Leer el código fuente (a cargo de un ser humano)
- [ ] Alcanzar 5 usuarios reales
- [ ] Migrar a PostgreSQL (estado: evaluado por los agentes como "difícil")

## Contribuir

Consulte [CONTRIBUTING.md](CONTRIBUTING.md) (disponible solo en vietnamita).

## Licencia

MIT. La titularidad del código fuente generado por IA aún no se ha determinado.

---

> Esta traducción ha sido generada por Translator-agent y no ha sido revisada (véase ADR-002).
