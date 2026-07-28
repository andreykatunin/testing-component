# Qwen Code + OpenSpec: Business Analyst и System Analyst

Готовый стартовый набор для двух именованных Qwen Code subagents и кастомного OpenSpec workflow.

## Что внутри

```text
.qwen/agents/
  business-analyst.md
  system-analyst.md
openspec/schemas/analysis-driven/
  schema.yaml
  templates/
    proposal.md
    business-requirements.md
    system-requirements.md
    spec.md
    design.md
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

4. Проверьте схему:

```bash
openspec schema validate analysis-driven
openspec schema which analysis-driven
```

5. Перезапустите Qwen Code и проверьте агентов:

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

Затем продолжите OpenSpec workflow:

```text
/opsx:continue add-example-feature
```

или обновите все связанные артефакты:

```text
/opsx:update add-example-feature
```

## Подстановка вашего шаблона

Если в проекте уже есть корпоративный шаблон, замените содержимое:

- `templates/business-requirements.md`
- `templates/system-requirements.md`

Агенты обязаны сохранять заголовки, порядок разделов и обязательные таблицы шаблона. Дополнительно скорректируйте правила идентификаторов и критерии качества в `.qwen/agents/*.md`.

## Важное замечание

Именованные Qwen Code subagents имеют отдельный контекст. Поэтому оба промпта требуют заново читать change, шаблон, существующие документы и релевантный код перед каждым обновлением. Не рассчитывайте, что системный аналитик автоматически знает детали предыдущего диалога бизнес-аналитика.
