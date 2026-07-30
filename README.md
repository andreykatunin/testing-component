# Qwen Code + OpenSpec: Business Analyst, System Analyst и QA Engineer

Готовый стартовый набор для трех именованных Qwen Code subagents и кастомного OpenSpec workflow.

## Что внутри

```text
.qwen/agents/
  business-analyst.md
  system-analyst.md
  qa-engineer.md
openspec/schemas/analysis-driven/
  schema.yaml
  templates/
    proposal.md
    business-requirements.md
    system-requirements.md
    spec.md
    design.md
    test-cases.md
    tasks.md
openspec/config.example.yaml
AGENTS.requirements.example.md
```

## Установка

1. Инициализируйте OpenSpec с поддержкой Qwen Code, если это еще не сделано:

```bash
openspec init --tools qwen
```

2. Скопируйте содержимое этого набора в корень проекта, не затирая существующие настройки без review.

3. Объедините `openspec/config.example.yaml` с вашим `openspec/config.yaml`. Ключевое значение:

```yaml
schema: analysis-driven
```

4. Объедините правила из `AGENTS.requirements.example.md` с корневым `AGENTS.md` проекта. Именно `AGENTS.md` задает основному агенту обязательную последовательность делегирования и владение файлами.

5. Проверьте схему:

```bash
openspec schema validate analysis-driven
openspec schema which analysis-driven
```

6. Перезапустите Qwen Code и проверьте агентов:

```text
/agents manage
```

## Рекомендуемый сценарий

Создайте change:

```text
/opsx:new add-example-feature
```

Затем явно делегируйте анализ:

```text
Use the business-analyst subagent for change add-example-feature.
Update business-requirements.md from the proposal and my new input: <описание>.
```

После согласования бизнес-требований:

```text
Use the system-analyst subagent for change add-example-feature.
Derive and update system-requirements.md. Inspect the relevant codebase first.
```

Затем продолжите OpenSpec workflow до готовности delta specs и `design.md`:

```text
/opsx:continue add-example-feature
```

После готовности specs и design делегируйте подготовку тест-кейсов:

```text
Use the qa-engineer subagent for change add-example-feature.
Derive and update test-cases.md from the requirements, specs, and design.
Keep stable test case IDs and vendor-neutral fields for later TMS publication.
```

После этого продолжите workflow для создания `tasks.md`:

```text
/opsx:continue add-example-feature
```

Чтобы обновить все связанные артефакты в порядке зависимостей, используйте:

```text
/opsx:update add-example-feature
```

## Подстановка вашего шаблона

Если в проекте уже есть корпоративный шаблон, замените содержимое:

- `templates/business-requirements.md`
- `templates/system-requirements.md`
- `templates/test-cases.md`

Агенты обязаны сохранять заголовки, порядок разделов и обязательные таблицы шаблона. Дополнительно скорректируйте правила идентификаторов и критерии качества в `.qwen/agents/*.md`.

## Публикация тест-кейсов в TMS

`test-cases.md` хранит каноническую, независимую от вендора тестовую модель. До выбора конкретной TMS поле `External ID` остается пустым, а раздел сопоставления полей не заполняется.

После выбора TMS:

1. Сопоставьте канонические поля с полями целевого проекта TMS.
2. При первой публикации создайте тест-кейсы по стабильным `TC-###`.
3. Сохраните полученные идентификаторы TMS в `External ID`.
4. При повторной публикации обновляйте записи по `External ID`, не создавая дубликаты.

Публикация или синхронизация с конкретной TMS является отдельной операцией и не выполняется `qa-engineer` автоматически.

## Важное замечание

Именованные Qwen Code subagents имеют отдельный контекст. Поэтому все три промпта требуют заново читать change, шаблон, существующие документы и релевантный код или тесты перед каждым обновлением. Не рассчитывайте, что следующий субагент автоматически знает детали диалога предыдущего.
