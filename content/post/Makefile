POSTDIR := content/post
OBJS := $(shell find -name '*.Rmd')
OUTFILES := $(patsubst %.Rmd,%.md,$(OBJS))
KNIT = Rscript -e "library(knitr); knit('$<')"

all:$(OUTFILES)

$(OUTFILES):$(OBJS)
	$(KNIT); \
	cp -r img/* ../../static/img/; \
	sed -i 's/img\//\/img\//g' $(OUTFILES); \
	cd ../../ && hugo undraft $(POSTDIR)/$(OUTFILES)

clean:
	rm $(OUTFILES)

