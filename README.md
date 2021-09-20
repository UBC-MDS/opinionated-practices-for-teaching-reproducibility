# Opinionated practices for teaching reproducibility: motivation, guided instruction and practice
Manuscript submitted to the Journal of Statistisc and Data Science Education

Authors: Joel Ostblom, Tiffany A. Timbers

## Abstract

In the data science courses at UBC, we define data science as the study 
and development of reproducible and auditable processes to obtain value 
(*i.e.,* insight) from data. While reproducibility is core to our definition, 
most data science learners enter the field with other aspects of data science in mind, 
for example, predictive modelling being the most interesting topic to novices. 
This fact, along with the highly technical nature 
of the industry standard reproducibility tools currently employed in data science, 
present out-of-the gate challenges in teaching reproducibility in the data science classroom. 
Put simply, students are not as intrinsically motivated to learn this topic, 
and it is not an easy one for them to learn. What can a data science educator do? 
Over several iterations of teaching courses focused on reproducible data science tools and workflows, 
we have found that motivation, guided instruction 
and practice are key to effectively teach this challenging, yet important subject. 
Here we present examples of how we deeply motivate, effectively guide
and provide ample practice opportunities data science students 
to effectively engage them in learning about this topic. 

## Preprint submission to the arXiv

The arXiv requires single spaced submissions so I commented the double spacing LaTeX command
(the journal we submitted to requires double spacing for the review process).

The arXiv requires us to submit a `.tex` file rather than a rendered PDF
and they reject PDF submissions when they detect that it is from a `.tex` file.
To create the `.tex` file from our `.Rmd`
we need to include the following in the preamble:

```yaml
output:
  bookdown::pdf_document2:
    keep_tex: true
```

The arXiv requires all the files necessary to build the PDF from the `.tex` file,
which includes the references file and any figures.
Unfortunately,
their servers to not run `bibtex` so they don't accept submission of `.bib` files.
Instead they require us to submit the built references in `.bbl` format.
Since we ran into error trying to generate the `.bbl` file via the `bibtex` command,
I switched our references to use `biblatex`,
so that we can use the `biber` command instead.
`biber` has issues with underscores in filename,
so I renamed all files.
`biber` requires specifying `biblio-style: authoryear` in the preamble,
otherwise it default to just showing numbers for the inline citations.
It also inserts the "References" heading automatically,
so we don't need a markdown heading for this section.
The preamble now looks like this:

```yaml
bibliography: teaching-reproducibility.bib
biblio-style: authoryear
output:
  bookdown::pdf_document2:
    keep_tex: true
    citation_package: biblatex
    biblatexoptions: backend=biber
```

### Build process and brief description of build commands

To facilitate the building of both the arXiv preprint
and any journal submission,
we collected both in a `Makefile`,
which we can run from the root of this repository:

```
make -C manuscript
```

To generate the `.bbl` file and correctly include the references in the `.tex` file,
the `Makefile` will run the following commands:

```bash
pdflatex teaching-reproducibility
biber teaching-reproducibility
pdflatex teaching-reproducibility
pdflatex teaching-reproducibility
```

We can then create an archive
containing the LaTeX, bibliography, and image folder to submit to the arXiv:

```bash
zip -r arxiv/teaching-reproducibility.pdf teaching-reproducibility.tex teaching-reproducibility.bbl img
```

The `Makefile` also strip any identifying information using `sed`
and save the blinded and preprint PDF to the journal submission directory.

```bash
sed -i '/\\author{/d;/pdfauthor={/d;s/University of British Columbia/University of ABC/;s/ UBC/ ABC/;s/[(]UBC[)]/(ABC)/;s/ DSCI/ XYZ/' teaching-reproducibility.tex
```

Then it cleans up the intermediate LaTeX artifacts in the repo:

```bash
rm teaching-reproducibility.{blg,log,aux,run.xml,bcf,bbl,tex}
```
