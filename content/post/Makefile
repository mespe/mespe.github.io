POSTDIR := content/post
# OBJS := $(shell find -name '*.Rmd')
# OUTFILES := $(patsubst %.Rmd,%.md,$(OBJS))
OUTFILES:= power.md R_best_practice.md
KNIT = Rscript -e "library(knitr); knit('$<')"

all: $(OUTFILES)

%.md: %.Rmd
	$(KNIT) ;\
	cp -r figure/* ../../static/img/; \
	sed -i 's/figure\//\/img\//g' $(OUTFILES); \

# $(OUTFILES):$(OBJS)
# 	$(KNIT); \
# 	cp -r figure/* ../../static/img/; \
# 	sed -i 's/figure\//\/img\//g' $(OUTFILES); \
# 	#cd ../../ && hugo undraft $(POSTDIR)/$(OUTFILES)

clean:
	rm $(OUTFILES)

