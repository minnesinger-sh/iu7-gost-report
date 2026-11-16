# Отчёты LaTeX по ГОСТ: навык для Claude и шаблон лабораторной

Навык `gost-report` для Claude Code и шаблон лабораторной работы по курсу
«Анализ алгоритмов» (МГТУ им. Н. Э. Баумана, кафедра ИУ7). Навык помогает
писать и исправлять отчёты LaTeX по ГОСТ — лабораторные, курсовые, НИР —
и накапливает правила оформления по замечаниям.

## Состав

```
.claude/skills/gost-report/      навык
  SKILL.md                       порядок работы над отчётом
  references/RULES.md            уровни правил, что читать, куда записывать
  references/common.md           общие правила оформления
  references/research.md         тесты, измерения, графики
  references/build.md            сборка и проверка PDF
  references/documents/          правила типов документов
  references/courses/            договорённости курсов
  references/experience.md       опыт прошлых работ (подсказки)
  references/preamble.md         версии преамбулы
  references/gost/INDEX.md       коллекция ГОСТов
  assets/preamble.vN.tex         версии преамбулы LaTeX
default-aa-latex-template/       шаблон лабораторной по «Анализу алгоритмов»
```

Правила отдельных работ хранятся в `references/local/` и в репозиторий
не входят.

## Требования

- XeLaTeX и `latexmk` (на macOS — MacTeX).
- Шрифты Times New Roman и Arial. На macOS есть в системе; на Линуксе —
  пакет `ttf-mscorefonts-installer` или файлы шрифтов в `~/.fonts`.
- Го (Go) — для кода лабораторных.

## Навык

Скопировать `.claude/skills/gost-report/` в `.claude/skills/` проекта или
в `~/.claude/skills/`, чтобы навык был доступен во всех проектах. Claude
подключает его сам, когда задача касается отчёта по ГОСТ; начинает
с `references/RULES.md`.

Правила разложены по уровням: общие нормы → тип документа → курс →
конкретная работа. Новые требования и замечания записываются на свой
уровень; более частный уровень уточняет более общий.

## Шаблон лабораторной

1. Скопировать `default-aa-latex-template/` в `lab-NN/` рядом с `.claude/`.
2. В `report/main.tex` заполнить `\labnumber`, `\labtitle`, `\labauthor`,
   `\labgroup`, `\labteacher`.
3. Команды из `lab-NN/`:

| Команда | Действие |
|---|---|
| `make test` | `go vet` и `go test` |
| `make run` | запуск программы |
| `make figures` | рисунки `report/img/*.tikz` → `report/img/*.pdf` |
| `make report` | отчёт `output/pdf/report.pdf` |
| `make submit SUBMIT=<путь>` | копия для сдачи: `code/`, `test/`, `report/`, `Makefile` |

## Авторство и лицензия

Основано на [aa-report-gost-skill](https://github.com/coma8765) (coma8765).

Без условий: копируйте, изменяйте и распространяйте как угодно —
см. [LICENSE](LICENSE) (The Unlicense). Тексты ГОСТов в
`references/gost/` принадлежат их правообладателям.
