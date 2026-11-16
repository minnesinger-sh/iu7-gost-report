GO      ?= go
LATEXMK ?= latexmk

.PHONY: all test report clean

all: test report

test:
	@tmp=$$(mktemp -d) && cp code/* test/* "$$tmp"/ && \
	(cd "$$tmp" && $(GO) vet ./... && $(GO) test ./...); \
	status=$$?; rm -rf "$$tmp"; exit $$status

report:
	cd report/tex && $(LATEXMK) -xelatex -interaction=nonstopmode \
		-halt-on-error -file-line-error -outdir=../build main.tex
	cp report/build/main.pdf report/report.pdf

clean:
	rm -rf report/build
	cd report/tex && rm -f *.aux *.log *.toc *.out *.fls *.fdb_latexmk \
		*.synctex.gz *.xdv *.bbl *.blg *.pdf
	find . -name .DS_Store -not -path './.git/*' -delete
