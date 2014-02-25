#
# Caution: Don't remove any tab characters.
# A tab character is used to distinguish dependencies from commands.
##########################################################################
# Define the main file that includes all others and the bib file
# In local make script, define, e.g., 'export MAIN=other_thesis' to overwrite default

MAIN := thesis
BIB := thesis

##########################################################################
# Get all .fig files in current directory
# and replace .fig suffix by .pdf and .pdf_t, respectively.

FIGURES   := $(wildcard *.fig)

FIGPDF    := $(patsubst %.fig,%.pdf,$(FIGURES))
FIGPDFT   := $(patsubst %.fig,%.pdf_t,$(FIGURES))

FIGPSTEX  := $(patsubst %.fig,%.pstex,$(FIGURES))
FIGPSTEXT := $(patsubst %.fig,%.pstex_t,$(FIGURES))

FIGEEPIC  := $(patsubst %.fig,%.eepic,$(FIGURES))

##########################################################################
# Get all .eps files in current directory
# and replace .eps suffix by .pdf suffix.

EPS       := $(wildcard *.eps)

EPSPDF    := $(patsubst %.eps,%.pdf,$(EPS))

##########################################################################
# Define the pattern rules
#
# Example:
# The following rule tells 'make' how to generate
# .dvi files from .tex files:
#
# %.dvi : %.tex $(FIGURES)
#	latex $<
#
# The generation depends on the .tex file as well as on the $(FIGURES).
# % matches any non-empty substring, e.g., 'foo'.
# Then $< is 'foo.tex', $@ is 'foo.dvi', and $* is 'foo'.

%.bbl :	$(BIB).bib %.tex
	bibtex $*

%.pdf : %.tex $(FIGURES)
	pdflatex $<

%.dvi: %.tex $(FIGURES)
	latex $<

%.ps : %.dvi
	dvips -P cmz -t letter -o $@ $<

%.pdf : %.fig
	fig2dev -L pdftex -n $@ $< $@

%.pdf_t : %.fig
	fig2dev -L pdftex_t -p $*.pdf $< $@

%.pstex : %.fig
	fig2dev -L pstex -n $@ $< $@

%.pstex_t : %.fig
	fig2dev -L pstex_t -p $*.pstex $< $@

%.eepic : %.fig
	fig2dev -L eepic $< $@

%.pdf : %.eps
	epstopdf $<

%.ps.gz : %.ps
	gzip -c $< > $@

##########################################################################
# Define the rules
#
# The first rule is the default rule and will be invoked by 'make'.

.PHONY : thesis again dgain bib fig pdf ps dvi all clean final

thesis:	fig pdf bib

again:	pdf
	pdflatex $(MAIN)

dgain:	dvi
	latex $(MAIN)

bib:	$(MAIN).bbl

fig:    $(FIGPDF) $(FIGPDFT) $(FIGEEPIC) $(EPSPDF)

pdf:    $(MAIN).pdf

ps:	fig dvi bib $(MAIN).ps

dvi:	$(MAIN).dvi

all:    thesis
	pdflatex $(MAIN)
	pdflatex $(MAIN)

final: final.pdf

final.pdf: $(MAIN).pdf
	ps2pdf14 -dPDFSETTINGS=/prepress $(MAIN).pdf final.pdf

clean:
	/bin/rm -f *~ *.aux *.bbl *.blg *.dvi *.idx *.ilg *.ind *.lof \
                   *.log *.lot *.ps *.ps.gz *.pdf *.toc *.pstex *.pstex_t \
                   *.pdf_t *.eepic *.fig.bak *.pdfsync *.out
