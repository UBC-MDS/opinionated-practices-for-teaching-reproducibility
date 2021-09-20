# Since the preprint and journal submission share most of the creation process,
# I chose to simplify the Makefile
# by making the journal submission files depend on the preprint being created first.

.PHONY: all clean
# Use bash instead of sh. Required for brace filename expansion
SHELL=/usr/bin/bash

all: teaching-reproducibility.tex\
     arxiv/teaching-reproducibility.zip\
     journal-of-statistics-and-data-science-education/teaching_reproducibility.pdf\
     journal-of-statistics-and-data-science-education/teaching_reproducibility_blinded.pdf

teaching-reproducibility.tex: teaching-reproducibility.Rmd teaching-reproducibility.bib img
	Rscript -e "rmarkdown::render('teaching-reproducibility.Rmd')"
	pdflatex teaching-reproducibility
	biber teaching-reproducibility
	pdflatex teaching-reproducibility
	pdflatex teaching-reproducibility

arxiv/teaching-reproducibility.zip: teaching-reproducibility.tex
	zip -r teaching-reproducibility.zip teaching-reproducibility.tex teaching-reproducibility.bbl img
	mv teaching-reproducibility.zip arxiv/

journal-of-statistics-and-data-science-education/teaching_reproducibility.pdf: arxiv/teaching-reproducibility.zip
	cp teaching-reproducibility.pdf journal-of-statistics-and-data-science-education/teaching_reproducibility.pdf

journal-of-statistics-and-data-science-education/teaching_reproducibility_blinded.pdf: journal-of-statistics-and-data-science-education/teaching_reproducibility.pdf
	sed -i '/\\author{/d;/pdfauthor={/d;s/University of British Columbia/University of ABC/;s/ UBC/ ABC/;s/[(]UBC[)]/(ABC)/;s/ DSCI/ XYZ/' teaching-reproducibility.tex
	pdflatex teaching-reproducibility
	mv teaching-reproducibility.pdf journal-of-statistics-and-data-science-education/teaching_reproducibility_blinded.pdf
	rm teaching-reproducibility.{blg,log,aux,run.xml,bcf,bbl,tex}

clean:
	rm -f arxiv/teaching-reproducibility.zip journal-of-statistics-and-data-science-education/teaching_reproducibility{.pdf,_blinded.pdf}
