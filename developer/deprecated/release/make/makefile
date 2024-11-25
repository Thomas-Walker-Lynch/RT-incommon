

#--------------------------------------------------------------------------------
# This makefile must be called through a local makefile rather than directly.
# the local make file should include:
#
#  1. $(RESOURCE)/library/makefile-environment  to give an environment to work with
#  2. it should set the variables to custom values
#  3. it should include this file, $(RESOURCE)/library/makefile
#  4. if it needs tailored targets it should then define them
#

#--------------------------------------------------------------------------------
# todo ...

# should add test that incdir is not pwd before deleting the include
# file. Currently it is working for us despite this problem because dist-clean is
# looking for $(module).h while the include file is always $(module).lib.h.

#--------------------------------------------------------------------------------
# files named by convention
#   this implements the .lib.c  and the .cli.c convention
#

# these are the sources edited by the programmer
C_SOURCE_LIB=  $(wildcard $(SRCDIR)/*.lib.c)
C_SOURCE_EXEC=  $(wildcard $(SRCDIR)/*.cli.c)
CC_SOURCE_LIB= $(wildcard $(SRCDIR)/*.lib.cc)
CC_SOURCE_EXEC= $(wildcard $(SRCDIR)/*.cli.cc)

#remove the suffix to get base name
C_BASE_LIB=  $(sort $(patsubst %.lib.c,  %, $(notdir $(C_SOURCE_LIB))))
C_BASE_EXEC=  $(sort $(patsubst %.cli.c,  %, $(notdir $(C_SOURCE_EXEC))))
CC_BASE_LIB= $(sort $(patsubst %.lib.cc, %, $(notdir $(CC_SOURCE_LIB))))
CC_BASE_EXEC= $(sort $(patsubst %.cli.cc, %, $(notdir $(CC_SOURCE_EXEC))))

# two sets of object files, one for the lib, and one for the command line interface progs
OBJECT_LIB= $(patsubst %, $(TMPDIR)/%.lib.o, $(C_BASE_LIB) $(CC_BASE_LIB))
OBJECT_EXEC= $(patsubst %, $(TMPDIR)/%.cli.o, $(C_BASE_EXEC) $(CC_BASE_EXEC))

# executables are made from EXEC sources
EXEC= $(patsubst %, $(EXECDIR)/%, $(C_BASE_EXEC) $(CC_BASE_EXEC))


#--------------------------------------------------------------------------------
# targets

.PHONY: all
all: version

.PHONY: version
version:
	@echo makefile version 7.0
	if [ ! -z "$(C)" ]; then $(C) -v; fi
	if [ ! -z "$(CC)" ]; then $(CC) -v; fi
	make -v

.PHONY: info
info:
	@echo "SHELL: " $(SHELL)
	@echo "ECHO: " $(ECHO)
	@echo "C: " $(C)
	@echo "CFLAGS: " $(CFLAGS)
	@echo "CC: " $(CC)
	@echo "CCFLAGS: " $(CCFLAGS)
	@echo "LINKFLAGS: " $(LINKFLAGS)
	@echo "DOCDIR: " $(DOCDIR)
	@echo "ENVDIR: " $(ENVDIR)
	@echo "EXECDIR: " $(EXECDIR)
	@echo "LIBDIR: " $(LIBDIR)
	@echo "TESTDIR: " $(TESTDIR)
	@echo "TMPDIR: " $(TMPDIR)
	@echo "TRYDIR: " $(TRYDIR)
	@echo "DEPFILE: " $(DEPFILE)
	@echo "LIBFILE: " $(LIBFILE)
	@echo "INCFILE: " $(INCFILE)
	@echo "C_SOURCE_LIB: " $(C_SOURCE_LIB)
	@echo "C_SOURCE_EXEC: " $(C_SOURCE_EXEC)
	@echo "CC_SOURCE_LIB: " $(CC_SOURCE_LIB)
	@echo "CC_SOURCE_EXEC: " $(CC_SOURCE_EXEC)
	@echo "C_BASE_LIB: " $(C_BASE_LIB)
	@echo "C_BASE_EXEC: " $(C_BASE_EXEC)
	@echo "CC_BASE_LIB: " $(CC_BASE_LIB)
	@echo "CC_BASE_EXEC: " $(CC_BASE_EXEC)
	@echo "OBJECT_LIB: " $(OBJECT_LIB)
	@echo "OBJECT_EXEC: " $(OBJECT_EXEC)
	@echo "EXEC: " $(EXEC)

# should be safe to run this in an already setup or partially setup directory
# gee looks like a candidate for a makefile function ..
.PHONY: setup
setup:
	[ ! -e $(DOCDIR)  ]  && mkdir $(DOCDIR)  || true
	[ ! -e $(EXECDIR) ]  && mkdir $(EXECDIR) || true
	[ ! -e $(LIBDIR)  ]  && mkdir $(LIBDIR)  || true
	[ ! -e $(SRCDIR)  ]  && mkdir $(SRCDIR)  || true
	[ ! -e $(TESTDIR) ]  && mkdir $(TESTDIR) || true
	[ ! -e $(TMPDIR)  ]  && mkdir $(TMPDIR)  || true
	[ ! -e $(TRYDIR)  ]  && mkdir $(TRYDIR)  || true

.PHONY: dependency
dependency:
	@rm -f $(DEPFILE)
	@if [ -z "$(CC)" ]; then\
	  if [ -z "$(C)" ]; then\
	    $(ECHO) "No compiler specified";\
	    exit 1;\
	  else\
	    echo "C compiler only deps" ;\
	    $(C) $(CFLAGS) -MM  $(C_SOURCE_LIB) $(C_SOURCE_EXEC) | sed 's|^.*\.o|$(TMPDIR)/&|' >> $(DEPFILE);\
	    echo "deps for C linking";\
	    for i in $(C_BASE_EXEC); do\
	      $(ECHO) >> $(DEPFILE);\
	      $(ECHO) "$(EXECDIR)/$$i : $(TMPDIR)/$$i.cli.o $(LIBFILE)" >> $(DEPFILE);\
	      $(ECHO) "	$(C) -o $(EXECDIR)/$$i $(TMPDIR)/$$i.cli.o $(LINKFLAGS)" >> $(DEPFILE);\
	    done;\
	  fi;\
	else\
	  $(CC) $(CCFLAGS) -MM  $(CC_SOURCE_LIB) $(CC_SOURCE_EXEC) | sed 's|^.*\.o|$(TMPDIR)/&|' >> $(DEPFILE);\
	  if [ -z "$C()" ]; then\
	    echo "CC compiler only deps" ;\
	  else\
	    echo "CC and C mixed compile deps" ;\
	    $(C) $(CFLAGS) -MM   $(C_SOURCE_LIB) $(C_SOURCE_EXEC) | sed 's|^.*\.o|$(TMPDIR)/&|' >> $(DEPFILE);\
	  fi;\
	  echo "deps for CC linking";\
	  for i in $(CC_BASE_EXEC) $(C_BASE_EXEC) ; do\
	    $(ECHO) >> $(DEPFILE);\
	    $(ECHO) "$(EXECDIR)/$$i : $(TMPDIR)/$$i.cli.o $(LIBFILE)" >> $(DEPFILE);\
	    $(ECHO) "	$(CC) -o $(EXECDIR)/$$i $(TMPDIR)/$$i.cli.o $(LINKFLAGS)" >> $(DEPFILE);\
	  done;\
	fi

.PHONY: library
library: $(LIBFILE)

$(LIBFILE): $(OBJECT_LIB)
	ar rcs $(LIBFILE) $(OBJECT_LIB)

.PHONY: cli
cli: $(LIBFILE)
	make sub_cli

.PHONY: sub_cli
sub_cli: $(EXEC)

.PHONY: share
share:
	if [ ! -z "$(wildcard $(LIBDIR)/*)" ]; then cp $(LIBDIR)/* $(DIST_TARGET)/$(LIBDIR); fi
	if [ ! -z "$(wildcard $(EXECDIR)/*)" ]; then cp $(EXECDIR)/* $(DIST_TARGET)/$(EXECDIR); fi
#	if [ ! -z "$(wildcard $(SRCDIR)/*.h)" ]; then cp $(SRCDIR)/*.h $(DIST_TARGET)/include; fi


.PHONY: clean
clean:
	for i in $(wildcard $(TMPDIR)/*); do rm $$i || true; done 

.PHONY: dist-clean
dist-clean:
	for i in $(wildcard $(TMPDIR)/*); do rm $$i || true; done 
	for i in $(EXEC); do [ -e $$i ] && rm $$i || true; done 
	rm -f $(LIBFILE)

# recipes
 $(TMPDIR)/%.o : $(SRCDIR)/%.c
	$(C) $(CFLAGS) -o $@ -c $<

 $(TMPDIR)/%.o : $(SRCDIR)/%.cc
	$(CC) $(CCFLAGS) -o $@ -c $<

# recipes
#$(TMPDIR)/%.o : $(SRCDIR)/%.c
#	$(C) $(CFLAGS) -o $@ -c <($(CPPP) $<)

$(TMPDIR)/%.o : $(SRCDIR)/%.c
	$(CPPP) $< $(TMPDIR)/tempfile.c
	$(C) $(CFLAGS) -o $@ -c $(TMPDIR)/tempfile.c
#	rm $(TMPDIR)/tempfile.c
