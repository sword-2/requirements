#Makefile to build up a document instead of building software

#Some variables
toc_relative=make_stuff/toc_relative #keep
Ofile_rel=o_des_rel.html #when relative links work


#default recipe for make without arguments
all: update_each_folder $(toc_relative) $(Ofile_rel)

doc_dirs=$(sort $(shell find * -maxdepth 1 -type d ! -name make_stuff))
#$(info $$doc_dirs is [${doc_dirs}]) #debugging

#Open of the relative output with terminal browser w3m
open:
	w3m $(Ofile_rel)

#Go thru each sub-directory and update submakes
.PHONY: update_each_folder
update_each_folder: $(doc_dirs)
	@for i in $(doc_dirs); do \
		$(MAKE) -C "$$i"; \
	done

#Make a starting file like index.html. Uses the toc_relative (below).
$(Ofile_rel): update_each_folder Makefile $(toc_relative)
	@if [ -f $@ ]; then rm $@; fi; \
	printf '<html><head>' >> $@; \
	printf '\n<title>Operator Description</title>' >> $@; \
	printf '</head><body>' >> $@; \
	printf '\n<h2>Operator Description</h2>' >> $@; \
	cat $(toc_relative)  >> $@; \
	printf '</body></html>' >> $@;


#make a table of contents by using the directory names
$(toc_relative): $(doc_dirs) Makefile
	@if [ -f $@ ]; then rm $@; fi; \
	printf '\n<table id=toc border=1>' >> $@; \
	printf '\n<caption>Contents</caption>' >> $@; \
	printf '\n<tr><th>No</th>' >> $@; \
	printf '\n<th>Name</th>' >> $@; \
	printf '\n<th>Link</th></tr>' >> $@; \
	for i in $(doc_dirs); do \
		pfix=$${i%%_*}; \
		sfix=$${i#*_}; \
		printf '<tr>' >> $@; \
		printf '\t<td>%s</td>' $$pfix >> $@; \
		printf '\t<td>%s</td>' $$sfix >> $@; \
		printf '\t<td><a href=%s/%s.html>Link</a></td>' $$i $$pfix >> $@; \
		printf '</tr>\n' >> $@; \
	done; \
	printf '</table>' >> $@;


#remove all outputs from make for starting over
.PHONY: clean
clean:
	if [ -f $(Ofile) ]; then \rm $(Ofile); fi; \
	if [ -f $(Ofile_rel) ]; then \rm $(Ofile_rel); fi; \
	if [ -f $(toc_relative) ]; then \rm $(toc_relative); fi; \
	for i in $(doc_dirs); do \
		$(MAKE) -C "$$i" $@; \
	done
