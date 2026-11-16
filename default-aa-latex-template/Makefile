# Лабораторная работа по анализу алгоритмов.
# Команды выполняются из корня лабораторной работы.

GO      ?= go
LATEXMK ?= latexmk
REPORT  := output/pdf/report.pdf

.PHONY: all build test run figures report submit clean

all: test report

## Программа

build:
	cd src && $(GO) build -o ../bin/app .

test:
	cd src && $(GO) vet ./... && $(GO) test ./...

run:
	cd src && $(GO) run .

## Рисунки: каждый img/<имя>.tikz собирается в отдельный img/<имя>.pdf
## (та же преамбула и стили, что у отчёта); отчёт вставляет готовые PDF.

FIGS := $(patsubst %.tikz,%.pdf,$(wildcard report/img/*.tikz))

figures: $(FIGS)

report/img/%.pdf: report/img/%.tikz report/figure.tex report/figstyles.tex
	mkdir -p build/fig
	cd report && xelatex -interaction=nonstopmode -halt-on-error \
		-output-directory=../build/fig -jobname=$* \
		"\def\figname{img/$*.tikz}\input{figure}" > /dev/null
	cp build/fig/$*.pdf $@

## Отчёт (XeLaTeX; на macOS --- MacTeX)

report: $(REPORT)

$(REPORT): $(wildcard report/*.tex) $(FIGS) $(wildcard src/*.go)
	mkdir -p build output/pdf
	cd report && $(LATEXMK) -xelatex -interaction=nonstopmode -halt-on-error \
		-file-line-error -outdir=../build main.tex
	cp build/main.pdf $(REPORT)

## Копия для сдачи: code/, test/, report/{tex,img,data}, report/report.pdf.
## В каталоге SUBMIT заменяются только code/, test/ и report/.

SUBMIT   ?= ../submit/$(notdir $(CURDIR))
PREAMBLE := $(shell sed -n 's|.*\\input{\(.*preamble\.v[0-9]*\.tex\)}.*|\1|p' report/main.tex)

submit: test report
	@test -n "$(SUBMIT)" && test "$(SUBMIT)" != "/" && test -n "$(PREAMBLE)"
	@test $$(wc -c < $(REPORT)) -le 15728640 || { echo "report.pdf больше 15 МБ"; exit 1; }
	rm -rf "$(SUBMIT)/code" "$(SUBMIT)/test" "$(SUBMIT)/report"
	mkdir -p "$(SUBMIT)/code" "$(SUBMIT)/test" "$(SUBMIT)/report/tex" \
		"$(SUBMIT)/report/img" "$(SUBMIT)/report/data"
	cp src/go.mod $(filter-out %_test.go,$(wildcard src/*.go)) $(wildcard src/*.s) "$(SUBMIT)/code/"
	$(if $(wildcard src/*_test.go),cp $(wildcard src/*_test.go) "$(SUBMIT)/test/")
	cp $(filter-out report/figure.tex report/figstyles.tex,$(wildcard report/*.tex)) "$(SUBMIT)/report/tex/"
	cp report/$(PREAMBLE) "$(SUBMIT)/report/tex/00-preamble.tex"
	sed -e 's|\\input{.*preamble\.v[0-9]*\.tex}|\\input{00-preamble}|' \
		-e 's|\\newcommand{\\srcdir}{../src/}|\\newcommand{\\srcdir}{../../code/}|' \
		-e 's|\\newcommand{\\testdir}{../src/}|\\newcommand{\\testdir}{../../test/}|' \
		report/main.tex > "$(SUBMIT)/report/tex/main.tex"
	find report/img -maxdepth 1 \( -name "*.pdf" -o -name "*.jpg" -o -name "*.png" \) -exec cp {} "$(SUBMIT)/report/img/" \;
	$(if $(wildcard results/*),cp results/* "$(SUBMIT)/report/data/")
	cp $(REPORT) "$(SUBMIT)/report/report.pdf"
	cp tools/submit/Makefile "$(SUBMIT)/Makefile"
	cp tools/submit/latexmkrc "$(SUBMIT)/report/tex/.latexmkrc"
	touch "$(SUBMIT)/.gitignore"
	test -z "$$(tail -c1 "$(SUBMIT)/.gitignore")" || echo >> "$(SUBMIT)/.gitignore"
	while IFS= read -r l; do grep -qxF "$$l" "$(SUBMIT)/.gitignore" || echo "$$l" >> "$(SUBMIT)/.gitignore"; done < tools/submit/gitignore
	@echo "Копия для сдачи: $(SUBMIT)"

clean:
	rm -rf build bin
