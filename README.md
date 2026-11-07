# spec-driven-customized

Кастомная [OpenSpec](https://github.com/Fission-AI/OpenSpec)-схема на базе `spec-driven`
(proposal → specs → design → tasks), переиспользуемая между проектами.

Особенности относительно стандартной схемы:

- Сценарии в формате Given/When/Then (Gherkin) с приоритетами P0/P1/P2
- Двуязычные формулировки требований: «Система ДОЛЖНА (MUST) …», «Система МОЖЕТ (MAY) …»
- Инструкции на русском языке

## Структура

```
schema.yaml      # манифест схемы (artifacts, инструкции, порядок apply)
templates/       # шаблоны артефактов
  proposal.md
  spec.md
  design.md
  tasks.md
```

## Подключение к проекту

OpenSpec резолвит кастомные схемы только из `openspec/schemas/<имя-схемы>/`
проекта. Имя директории должно совпадать с полем `name` в `schema.yaml`
(здесь — `spec-driven-customized`).

### Вариант 1: симлинк (для локальной разработки)

```bash
cd <проект>
mkdir -p openspec/schemas
ln -s ../../../spec-driven-customized openspec/schemas/spec-driven-customized
```

Путь в `ln -s` относительный — он поднимается от `openspec/schemas/`
до общей родительской директории (в примере схема лежит рядом с проектом).
Изменения в схеме видны во всех проектах сразу.

### Вариант 2: копирование

```bash
cp -R <путь>/spec-driven-customized openspec/schemas/
rm -rf openspec/schemas/spec-driven-customized/.git
```

Проект получает независимую копию; обновления подтягиваются повторным копированием.

### Вариант 3: git submodule

```bash
git submodule add <url-репозитория> openspec/schemas/spec-driven-customized
```

Пинует конкретный коммит схемы; обновление — через `git submodule update --remote`
и коммит бампа в проекте.

После подключения укажите схему в `openspec/config.yaml` проекта:

```yaml
schema: spec-driven-customized
```

## Проверка

```bash
openspec schema validate spec-driven-customized
openspec templates   # показать резолв шаблонов
```
