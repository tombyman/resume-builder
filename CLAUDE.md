# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Назначение проекта

Разработка Claude Code скила `resume-builder`: адаптация существующего IT-резюме под российский рынок труда (hh.ru). Результат — Markdown + PDF, версии на русском и английском; опционально — адаптация под конкретную вакансию.

## Структура

Репозиторий — одновременно Claude Code плагин и marketplace (корень репозитория = корень плагина):

- `.claude-plugin/plugin.json` — манифест плагина
- `.claude-plugin/marketplace.json` — манифест marketplace (обязателен для установки по URL)
- `skills/resume-builder/SKILL.md` — входная точка скила (каноническое расположение)
- `skills/resume-builder/references/*.md` — справочные материалы, которые скил читает по мере необходимости
- `skills/resume-builder/templates/resume.latex` — шаблон вёрстки PDF; в SKILL.md ссылается как `${CLAUDE_SKILL_DIR}/templates/resume.latex`
- `.claude/skills/resume-builder` — symlink на `../../skills/resume-builder`, чтобы скил работал и при локальной работе в этом репозитории

## Установка в других проектах

По URL репозитория (или shorthand `owner/repo` для GitHub):

```bash
claude plugin marketplace add <url-репозитория>
claude plugin install resume-builder@resume-builder
```

Локальная проверка без публикации: `claude plugin validate .` и запуск с `claude --plugin-dir .`.

## Правила

- Контент скила (инструкции, шаблоны, вопросы) пишется на русском; английский используется только в EN-версиях резюме.
- Внешние ресурсы и сервисы не используются: PDF собирается локально через `pandoc --pdf-engine=xelatex` с явным `-V mainfont="DejaVu Serif"` (по умолчанию Latin Modern — без кириллицы).
- Скил никогда не выдумывает опыт, навыки и контакты: недостающие данные запрашиваются у пользователя одним сводным списком вопросов.

## Проверка

Скил проверяется запуском `/resume-builder` на примере резюме; ожидаются `.md`-файлы (RU и EN) и собранные из них PDF.
