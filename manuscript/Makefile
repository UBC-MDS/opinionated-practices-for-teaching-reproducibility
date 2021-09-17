arxiv: arxiv-ms.zip

teaching-reproducibility.tex: teaching-reproducibility.Rmd teaching-reproducibility.bib img
	Rscript -e "rmarkdown::render('teaching-reproducibility.Rmd')"

teaching-reproducibility.bbl: teaching-reproducibility.tex teaching-reproducibility.bib img
	pdflatex teaching-reproducibility
	biber teaching-reproducibility
	pdflatex teaching-reproducibility
	pdflatex teaching-reproducibility

arxiv-ms.zip: teaching-reproducibility.tex teaching-reproducibility.bbl img
	zip -r arxiv-ms.zip teaching-reproducibility.tex teaching-reproducibility.bbl img
	rm teaching-reproducibility.{blg,log,aux,run.xml,bcf,out}

clean:
	rm -f arxiv-ms.zip
	rm -f teaching-reproducibility.tex teaching-reproducibility.bbl
