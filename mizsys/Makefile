MFILES	= $(MIZFILES)
MMLLAR  = $(MFILES)/mml.lar
MMLLAR100  = $(MFILES)/mml.lar.100
MIZ100     = $(shell cat $(MMLLAR100))
STANDARDMIZ     = $(shell cat $(MMLLAR))
SHMIZ = $(shell shuffle < $(MMLLAR))

XSLTPROC = xsltproc 
XML2HTML = /home/urban/xsltxt/miz.xsl
ADDABS = /home/urban/xsltxt/addabsrefs.xsl
MIZPL = /home/urban/xsltxt/mizpl.xsl
ACCOM = $(MFILES)/bin/accom 
VERIFIER = $(MFILES)/bin/verifier.7.8.05_hacktopcnst_bex

%.acc: %.miz
	$(ACCOM) $* > $*.acc 

%.bex: %.xml
	$(VERIFIER) -q -c $*
	perl -i -e 'local $$/;$$_=<>; m/((.|[\n])*?)<(PolyEval|.ByExpl)/; print $$1; while(m/(<PolyEval((.|[\n])*?)<\/PolyEval>)/g) { if(!(exists $$h{$$1})) { print $$1; $$h{$$1} = (); }} print "</ByExplanations>";' $*.bex

%.xml: %.acc
	$(VERIFIER) -q   $*  
	perl -i -e 'local $$/;$$_=<>; m/((.|[\n])*?)<(PolyEval|.ByExpl)/; print $$1; while(m/(<PolyEval((.|[\n])*?)<\/PolyEval>)/g) { if(!(exists $$h{$$1})) { print $$1; $$h{$$1} = (); }} print "</ByExplanations>";' $*.bex


%.xml1: %.xml
	$(XSLTPROC) $(ADDABS)  $*.xml > $*.xml1 

%.xml2: %.xml1
	$(XSLTPROC) $(MIZPL)  $*.xml1 > $*.xml2 

%.html: %.xml1
	$(XSLTPROC) --param proof_links \'1\' $(XML2HTML)  $*.xml1 > $*.html 

%.htmla1: %.xml1
	$(XSLTPROC) --param ajax_proofs \'1\' --param colored \'1\' --param proof_links \'1\' $(XML2HTML)  $*.xml1 > $*.htmla1 

%.htmla2: %.xml1
	$(XSLTPROC) --param ajax_proofs \'2\' --param proof_links \'1\' $(XML2HTML)  $*.xml1 > $*.htmla2 

# all: xboole.html
allhtml: $(addsuffix .html, $(SHMIZ))
allhtmla1: $(addsuffix .htmla1, $(SHMIZ))
allhtmla2: $(addsuffix .htmla2, $(SHMIZ))
html100: $(addsuffix .html, $(MIZ100))
allacc: $(addsuffix .acc, $(STANDARDMIZ))
allxml: $(addsuffix .xml, $(STANDARDMIZ))
allxml1: $(addsuffix .xml1, $(SHMIZ))
allxml2: $(addsuffix .xml2, $(SHMIZ))
allbex: $(addsuffix .bex, $(STANDARDMIZ))