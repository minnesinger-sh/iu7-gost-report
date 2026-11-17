# Шаблон лабораторной работы по АА

Скопировать каталог в `lab-NN` и
заполнить данные работы в `report/main.tex`: `\labnumber`, `\labtitle`,
`\labteacher` (они же идут на титульный лист `report/01-title.tex`).

| Команда       | Действие                                  |
|---------------|-------------------------------------------|
| `make test`   | `go vet` и `go test` в `src/`             |
| `make run`    | запуск программы                          |
| `make figures` | рисунки `report/img/*.tikz` → `report/img/*.pdf` |
| `make report` | сборка отчёта в `output/pdf/report.pdf`   |
| `make submit` | копия для сдачи (`SUBMIT=<путь>`, по умолчанию `../submit/lab-NN`): `code/`, `test/`, `report/{tex,img,data}`, `report/report.pdf`, `Makefile` |

Работа лежит в `<курс>/lab-NN/`, папка курса — рядом с `iu7-gost-report/`.
Преамбула: `iu7-gost-report/.claude/skills/gost-report/assets/preamble.v4.tex`
(в `report/main.tex` и `report/figure.tex` — путь `../../../iu7-gost-report/…`).
Правила оформления: `iu7-gost-report/.claude/skills/gost-report/references/RULES.md`.

Схемы и графики — по файлу `report/img/<имя>.tikz` (только `tikzpicture`), в отчёт
вставляются готовые PDF: `\includegraphics{<имя>}` внутри `figure`. Стили схем —
`report/figstyles.tex`, сборка одного рисунка — `report/figure.tex`; обозначения,
общие для текста и рисунков, — `report/00-macros.tex` (если нужны). Исходники
рисунков в копию для сдачи не входят. Данные замеров —
`results/` (в сдачу идут в `report/data/`).
