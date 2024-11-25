# /bin/make must be run from $REPO_HOME/developer

RESOURCE:=$(REPO_HOME)/developer
include $(RESOURCE)/make/environment_RT_0

# override defaults
LIBDIR=scratchpad

# compiler to use
C=gcc
CFLAGS= -Werror -include "$(RESOURCE)/make/RT_0.h" -I$(INCDIR) 
LINKFLAGS=-L$(LIBDIR) -L/usr/lib64 -L/usr/lib -l$(PROJECT)

# Import the rules. The dash prefix means to ignore include errors. This is
# required because the DEPFILE might not exist yet.
include $(RESOURCE)/make/targets
-include $(DEPFILE)
