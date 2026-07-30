# Оркестрация OpenSpec

## Диалог с оператором

- При `/opsx:explore` отделяй подтверждённые факты от открытых вопросов и предложений.
- Не считай предложение агента, best practice или молчание оператора подтверждённым фактом.
- Перед созданием proposal, business requirements, system requirements и test cases
  сначала выполняй DISCOVERY, затем показывай preview и запрашивай явное подтверждение.
- Задавай до пяти наиболее важных вопросов за один раунд.

Если артефакт или субагент вернул `RESULT: NEEDS_INPUT`:

1. Не создавай целевой артефакт и не переходи к следующему.
2. Передай вопросы оператору без самостоятельного выбора ответа.
3. После ответов повтори DISCOVERY с тем же субагентом.
4. При `READY_FOR_REVIEW` покажи оператору preview и попроси подтверждение.
5. Только после подтверждения запусти `MODE: WRITE` или запиши proposal.

Важный ответ оператора должен попасть в требование и в поле `Ответ / решение`
соответствующего BQ-*, SQ-* или QA-Q-*. Не оставляй решение только в чате.

## Последовательность

1. Proposal создаёт основной агент только после подтверждения preview.
2. Business requirements формирует `business-analyst`: DISCOVERY → review → WRITE.
3. Основной агент устанавливает `Approved` и `Approved by` только после явного
   согласования владельцем бизнеса или продукта.
4. Только после этого `system-analyst` выполняет DISCOVERY → review → WRITE.
5. Перед specs выполни
   `node scripts/check-workflow.mjs <change-id> --stage requirements`.
6. После specs и design вызови `qa-engineer`: DISCOVERY → review → WRITE.
7. После test cases создай tasks и получи `Readiness: Ready`, `Reviewed by`.
8. Перед apply выполни
   `node scripts/check-workflow.mjs <change-id> --stage apply`.
9. После apply выполни `/opsx:verify <change-id>`, затем sync/archive.

## Владение файлами

- `business-analyst` изменяет только `business-requirements.md`.
- `system-analyst` изменяет только `system-requirements.md`.
- `qa-engineer` изменяет только `test-cases.md`.
- Основной агент владеет proposal, specs, design и tasks.
- Не допускай параллельного редактирования одного файла двумя агентами.

## Изменения и конфликты

- Изменение бизнес-смысла требует повторного запуска `business-analyst`, а затем
  всех затронутых downstream-ролей.
- Техническое ограничение, меняющее бизнес-смысл, возвращается Business Analyst.
- QA Engineer возвращает противоречие владельцу исходного требования и не выбирает
  ожидаемое поведение самостоятельно.
- Результаты тестовых запусков не записываются в `test-cases.md`.

## Стабильные идентификаторы

- Business: `BR-*`, `AC-*`, `BQ-*`.
- System: `SR-*`, `NFR-*`, `SQ-*`.
- Specs/QA/tasks: `SC-*`, `TC-*`, `QA-Q-*`, `TASK-*`.
- ID не перенумеровываются и не переиспользуются.
