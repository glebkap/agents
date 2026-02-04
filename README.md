# AI Tools

Кастомные агенты и скиллы для Claude Code.

## Структура проекта

```
ai-tools/
├── .claude/
│   ├── agents/                      # Кастомные агенты
│   │   ├── one-shot/               # One-shot субагенты
│   │   │   ├── explorer.md
│   │   │   ├── analyst.md
│   │   │   ├── architect.md
│   │   │   ├── implementer.md
│   │   │   ├── e2e-test-writer.md
│   │   │   ├── qc-auditor.md
│   │   │   ├── METHODOLOGY.md
│   │   │   └── README.md
│   │   ├── e2e-test-writer.md      # Автономный E2E тестировщик
│   │   └── qc-auditor.md           # Автономный QC аудитор
│   ├── skills/                      # Кастомные скиллы
│   │   ├── one-shot/               # One-shot workflow
│   │   │   └── SKILL.md
│   │   ├── create-jira-issue/
│   │   └── investigate-jira-bug/
│   └── settings.local.json
├── dev_process/                     # Процессы разработки
└── README.md
```

## Артефакты задач

Все задачи, выполненные через one-shot методологию, создают артефакты в:

```
docs/tasks/<task_id>/
├── plan.md              # Утверждённый план
└── reports/
    ├── 0_orchestrator.md # Финальный отчёт оркестратора
    ├── 1_explorer.md    # Структура, файлы
    ├── 2_analyst.md     # AC чеклист
    ├── 3_architect.md   # Архитектура
    ├── 4_implementer.md # Изменённые файлы
    ├── 5_tester.md      # Тесты, маппинг AC
    └── 6_qc.md          # Вердикт QC
```

## Установка

Для использования агентов и скиллов во всех проектах, создайте символические ссылки:

```bash
# Перейти в директорию проекта
cd /path/to/ai-tools

# Создать симлинки (если еще не созданы)
ln -s "$(pwd)/.claude/agents" ~/.claude/agents
ln -s "$(pwd)/.claude/skills" ~/.claude/skills
```

После этого все агенты и скиллы будут доступны глобально в любом проекте.

## Проверка установки

```bash
# Проверить, что симлинки созданы
ls -la ~/.claude/ | grep -E "agents|skills"

# Вывод должен быть примерно таким:
# lrwxrwxrwx ... agents -> /path/to/ai-tools/.claude/agents
# lrwxrwxrwx ... skills -> /path/to/ai-tools/.claude/skills

# Проверить доступные агенты
ls ~/.claude/agents/
ls ~/.claude/agents/one-shot/
```

## Использование

### One-Shot Methodology (Skill)

Методология решения задач за один проход без потери контекста.

**Быстрый старт:**

Просто вызовите skill с описанием задачи:

```
/one-shot

Привести архитектуру и tech-stack к консистентному виду после изменений в prd.md
```

Skill автоматически:
1. Запросит task_id (если не указан)
2. Создаст структуру отчётов
3. Последовательно запустит фазы: анализ → архитектура → реализация → тесты → QC
4. Создаст финальный отчёт в `docs/tasks/<task_id>/`

**Полная документация:** [.claude/agents/one-shot/README.md](.claude/agents/one-shot/README.md)

### Skills

- **one-shot** (`/one-shot`) - One-shot решение задач за один проход: анализ → архитектура → реализация → тесты → QC
- **create-jira-issue** - Создание задач в Jira
- **investigate-jira-bug** - Исследование багов из Jira

### Агенты

**One-shot субагенты** (используются автоматически через `/one-shot` skill):
- **explorer** - Исследование codebase
- **analyst** - Анализ ТЗ, создание AC чеклиста
- **architect** - Проектирование архитектуры, планирование
- **implementer** - Диспетчер разработчиков
- **e2e-test-writer** - E2E тестирование
- **qc-auditor** - QC аудит и верификация

**Автономные агенты:**
- **e2e-test-writer** - Автономный E2E тестировщик
- **qc-auditor** - Автономный QC аудитор

## Добавление новых агентов/скиллов

Просто создайте новый файл в соответствующей директории:

```bash
# Новый агент
touch .claude/agents/my-agent.md

# Новый скилл
mkdir .claude/skills/my-skill
touch .claude/skills/my-skill/SKILL.md
```

Он автоматически станет доступен глобально через симлинк!

## Dev Process

Документация процессов разработки находится в [dev_process/](dev_process/):

- [dev_process.md](dev_process/dev_process.md) - Общий процесс разработки
- [e2e-testing-guide.md](dev_process/e2e-testing-guide.md) - Руководство по E2E тестированию
- [example_claude_md.md](dev_process/example_claude_md.md) - Примеры Claude промптов
