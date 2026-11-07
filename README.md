# spec-driven-ru

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

OpenSpec ищет кастомные схемы в трёх местах (по убыванию приоритета):
`openspec/schemas/<имя-схемы>/` проекта, `~/.local/share/openspec/schemas/<имя-схемы>/`
(все проекты пользователя) и среди встроенных схем CLI. Подробнее —
[документация OpenSpec](https://openspec.dev/docs/customize-schemas).
Имя директории должно совпадать с полем `name` в `schema.yaml`
(здесь — `spec-driven-ru`).

### Рекомендуемый способ: git submodule

Схема версионируется вместе с проектом, и каждый инженер получает её
при клонировании репозитория:

```bash
cd <проект>
git submodule add git@github.com:ab-budaev/spec-driven-ru.git openspec/schemas/spec-driven-ru
git commit -m "Add spec-driven-ru OpenSpec schema"
```

Клонирование проекта со схемой:

```bash
git clone --recurse-submodules <url-проекта>
# если проект уже склонирован:
git submodule update --init
```

Обновление схемы — осознанный коммит в проекте:

```bash
git submodule update --remote openspec/schemas/spec-driven-ru
git commit -am "Bump spec-driven-ru schema"
```

### Альтернативы

- **Симлинк** — для локальной разработки самой схемы: изменения видны
  во всех проектах сразу.

  ```bash
  ln -s ../../../spec-driven-ru openspec/schemas/spec-driven-ru
  ```

- **Копирование** — `cp -R` папки схемы в `openspec/schemas/`: проект получает
  независимую копию, обновления подтягиваются вручную.

После подключения укажите схему в `openspec/config.yaml` проекта:

```yaml
schema: spec-driven-ru
```

## Проверка

```bash
openspec schema validate spec-driven-ru
openspec templates   # показать резолв шаблонов
```
