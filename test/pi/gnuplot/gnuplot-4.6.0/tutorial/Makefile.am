## Process this file with automake to produce Makefile.in -*-Makefile-*-
AUTOMAKE_OPTIONS = foreign 1.2h

##plt-files-begin
PLT_FILES = eg1.plt eg2.plt eg3.plt eg4.plt eg5.plt eg6.plt eg7.plt \
linepoin.plt test.plt test_tikz.plt 
##plt-files-end

EXTRA_DIST = Makefile.am.in $(PLT_FILES) \
header.tex makefile.dst tutorial.tex eg3.dat

CLEANFILES = tutorial.aux tutorial.dvi tutorial.log tutorial.pdf tutorial.ps \
tutorial.toc eg1.tex eg2.tex eg3.tex eg4.tex eg5.tex eg6.tex eg7.tex \
eg7.eps eg7.pdf test.tex

DVIPS = dvips
LATEX = @LATEX@

SUFFIXES = .dvi .plt .pdf .tex .ps

# default target
all: @TUTORIAL@

tutorial: tutorial.dvi

notutorial:

# To touch it up after changes:
remake: tutorial.dvi

ps: tutorial.ps

tutorial.ps: tutorial.dvi

pdf: tutorial.pdf

tutorial.dvi: eg1.tex eg2.tex eg3.tex eg4.tex eg5.tex eg6.tex eg7.tex eg7.eps test.tex \
	tutorial.tex header.tex
	@echo Building LaTeX tutorial
	TEXINPUTS=.:$(srcdir):${TEXINPUTS}: $(LATEX) tutorial
	TEXINPUTS=.:$(srcdir):${TEXINPUTS}: $(LATEX) tutorial

tutorial.pdf: eg1.tex eg2.tex eg3.tex eg4.tex eg5.tex eg6.tex eg7.tex eg7.pdf test.tex \
	tutorial.tex header.tex
	@echo "Building LaTeX tutorial (PDF version)"
	TEXINPUTS=.:$(srcdir):${TEXINPUTS}: $(PDFLATEX) tutorial
	TEXINPUTS=.:$(srcdir):${TEXINPUTS}: $(PDFLATEX) tutorial

RUN_GNUPLOT = if test -x $(top_builddir)/src/gnuplot ; then \
	  GNUPLOT_PS_DIR=$(top_srcdir)/term/PostScript \
	  GNUPLOT_LIB=$(srcdir) \
	  GNUTERM=latex \
	  $(top_builddir)/src/gnuplot $< ; \
	else \
	  gnuplot $< ; \
	fi

eg7.eps: eg7.plt
	$(RUN_GNUPLOT)

.dvi.ps:
	$(DVIPS) -o $@ $<

.plt.tex:
	$(RUN_GNUPLOT)

.eps.pdf:
	epstopdf $<

distclean-local:
	@if test "$(top_srcdir)" != "$(top_builddir)" ; then \
	  rm -f eg3.dat; \
	fi

Makefile.am: Makefile.am.in
	rm -f $@ $@t
	sed -n '1,/^##plt-files-begin/p' Makefile.am.in > $@t
	echo PLT_FILES = *.plt | fmt | (tr '\012' @; echo ) \
	  |sed 's/@$$/%/;s/@/ \\@/g;' | tr @% '\012 ' >> $@t
	sed -n '/^##plt-files-end/,$$p' $< >> $@t
	chmod og-w $@t
	mv $@t $@

