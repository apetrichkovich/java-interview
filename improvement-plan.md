# Java Interview Page — Plan & Review

> Статус: темы из раздела «Приоритеты» **уже добавлены** на страницу новыми блоками
> (отмечены ✅). Удаление/перестановку решаешь сам. Раздел про неточности и тултип —
> к ревью.

## Общая оценка

**Формат** — хороший: краткое описание + tooltip при наведении. Компактно, работает как шпаргалка.
**Структура** — темы вперемешку (Java core → DB → Spring → Kafka → K8s), без явного разделения по категориям. Навигации нет.
**Текст** — в основном корректный, но есть несколько фактических ошибок (см. раздел 4).

⚠️ **Конфликт формата (главное).** Страница — плотная шпаргалка для *быстрого* повторения. Полный список ниже — это ~20+ новых тем, что почти удваивает объём. Уже добавлены приоритетные; остальное держи как «знаю, но в шпаргалку не выношу», иначе теряется смысл «пробежать глазами за 10 минут».

---

## 1. Критически важные пропуски

### Java Core
- ✅ **CompletableFuture / Future** — `thenApply`, `thenCompose`, `exceptionally`, `allOf`, `anyOf`. *(добавлено)*
- ✅ **Optional** — `of`, `ofNullable`, `orElse`, `orElseThrow`, `map`, `filter`, `ifPresent`. *(добавлено)*
- ✅ **default / static / private методы в интерфейсах** (Java 8/9). *(добавлено)*
- ✅ **Java 21 / Virtual Threads (Project Loom)** + Records, Sealed, Pattern Matching, Structured Concurrency. *(добавлено)*
- ✅ **WeakReference / SoftReference / PhantomReference** + **ThreadLocal**. *(добавлено)*

### Concurrency
- ✅ **Проблемы многопоточности** — Deadlock, Livelock, Starvation, Race condition, Visibility. *(добавлено)*
- ✅ **CountDownLatch, CyclicBarrier, Semaphore, Phaser, Exchanger**. *(добавлено)*
- ✅ **ReentrantLock, ReadWriteLock, StampedLock, Fork/Join**. *(добавлено)*

### Collections / Data Structures
- ✅ **Внутреннее устройство HashMap** — bucket, hash & index, load factor 0.75, treeify, resize. *(добавлено)*
- ⬜ **Структуры данных** (Tree, Graph, Heap, Trie) — не добавлено, опционально.

---

## 2. Spring / Hibernate / Microservices
- ✅ **Dependency Injection** — Constructor / Setter / Field. *(добавлено)*
- ✅ **Spring Security** — JWT, OAuth2/OIDC, authn vs authz. *(добавлено)*
- ✅ **Spring Cloud** — Service Discovery (Eureka), API Gateway, Config Server. *(добавлено)*
- ✅ **@Transactional** — propagation (REQUIRED, REQUIRES_NEW, NESTED…). *(добавлено)*
- ✅ **Hibernate entity states** — Transient / Persistent / Detached / Removed. *(добавлено)*
- ✅ **Lazy vs Eager, N+1 problem, Cascade, JPQL/Criteria**. *(добавлено)*
- ✅ **Microservices patterns** — Saga, CQRS, Event Sourcing, Outbox, DB-per-service. *(добавлено)*
- ⬜ **Spring Boot auto-configuration** (`@ConditionalOnClass`) — не добавлено, опционально.

---

## 3. Прочие темы
- ✅ **HTTP status codes** (2xx–5xx), методы, идемпотентность, HTTP/2-3, gRPC/WS/SSE. *(добавлено)*
- ✅ **Тестирование** — JUnit 5, Mockito, Spring Test, Testcontainers. *(добавлено)*
- ✅ **Docker** — Dockerfile, слои, multi-stage. *(добавлено)*
- ✅ **Observability** — Logging (SLF4J/ELK), Metrics (Micrometer/Prometheus), Tracing. *(добавлено)*
- ✅ **Maven lifecycle / Gradle**. *(добавлено)*
- ✅ **Redis** (типы данных, persistence) + **RabbitMQ**. *(добавлено)*
- ✅ **Web security (OWASP)** — SQLi, XSS, CSRF. *(добавлено)*

---

## 4. Фактические ошибки в существующем тексте (✅ исправлены)

| Блок | Проблема | Рекомендация |
|------|----------|--------------|
| **CAP theorem** | **Категория «CA: MySQL, PostgreSQL…» некорректна.** Классическое заблуждение: в распределённой системе нельзя отказаться от P (сеть всё равно рвётся), а одиночная RDBMS — не распределённая система, к ней CAP неприменима в таком виде. Это более грубая ошибка, чем спорный «Redis = CP». | Убрать категорию CA или пояснить, что это про single-node СУБД; для PostgreSQL-кластера это CP. |
| **Garbage Collectors** | **CMS удалён в Java 14.** Также отсутствуют современные **ZGC** и **Shenandoah** (low-pause). | Пометить CMS как deprecated/removed, добавить ZGC/Shenandoah. |
| **Thread Lifecycle** | `ACTIVE (RUNNABLE, RUNNING)` — это обучающая модель, а не значения enum. Реальный `Thread.State`: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED. | Пометить, что ACTIVE/RUNNING — не из enum (RUNNABLE делится на ready/running на уровне ОС). |
| **AOP** | «split program to modules» — неточно. AOP — это вынос *cross-cutting concerns* (logging, security, transactions) из бизнес-логики. | Переформулировать. |
| **Design Patterns** | Нет: Observer, Template Method, Chain of Responsibility, Abstract Factory, Facade, Flyweight. | Добавить по желанию. |
| **Java 8 streams** | `collect()` без примеров Collectors (`toList`, `groupingBy`, `joining`). | Дополнить tooltip. |
| **Functional programming** | Слишком кратко: можно добавить pure functions, immutability, no side effects. | Опционально. |

> Примечание: два `hashCode()` в tooltip — это намеренно **два разных примера** (ручной `31*result` и `Objects.hash`), не баг. Снято из претензий.

---

## 5. Баг тултипа: выпрыгивает не у элемента (✅ исправлен)

**Диагноз — конфликт версий Bootstrap.** В `<head>`:
- CSS: **Bootstrap 5.3.0-alpha3**
- JS: jQuery 3.2.1 slim + **Popper.js 1.12.9** + **Bootstrap 4.0.0 JS**

Bootstrap 4 JS строит тултип с разметкой `<div class="arrow">` (это в шаблоне в коде) и позиционирует через Popper **1.x**. Но CSS от Bootstrap **5** ожидает класс `.tooltip-arrow` и другую модель отступов/позиционирования. Из-за несовпадения CSS не применяется к тултипу корректно → он рендерится со смещением/«прыгает».

**Фикс (вариант A, минимальный — рекомендую):** опустить CSS до Bootstrap 4, чтобы совпало с JS 4.0.0 + Popper 1.x. Заменить строку `<link ... bootstrap@5.3.0-alpha3 ...>` на Bootstrap 4.6.2 CSS. jQuery-инициализация `$('abbr').tooltip()` и шаблон с `.arrow` останутся рабочими — менять JS не нужно.

**Фикс (вариант B, современный):** полностью перейти на Bootstrap 5 — убрать jQuery и Popper 1.x, подключить bootstrap.bundle.min.js (Popper 2 внутри), заменить инициализацию на `new bootstrap.Tooltip(el)` для каждого `abbr` и `placement: 'auto'` оставить. Больше работы, но без jQuery.

---

## 6. Структурные предложения
1. ✅ **Разделители категорий** — добавлены 12 разделителей: OOP & Paradigms, Java Core, Collections, Java Versions, Testing & Design Patterns, Database & JPA, Spring Framework, Concurrency, DB Theory & CAP, Algorithms & Operators, DevOps & Infrastructure, Messaging & Microservices.
2. **Дубль темы hashCode/equals** — отдельный блок + внутри «Object methods». Можно оставить (повтор полезен для зубрёжки) или схлопнуть.
3. **Последний блок «Questions»** — личные заметки, не трогаю.

---

## Приоритеты (всё ✅ добавлено)
1. CompletableFuture · 2. Optional · 3. Virtual Threads (Java 21) · 4. Deadlock/Race · 5. HashMap internals · 6. Entity states · 7. N+1 · 8. @Transactional propagation · 9. Spring Security · 10. Saga/CQRS/Event Sourcing
