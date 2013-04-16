# -*- coding: utf-8 -*-
# Author:   Ondrej Platek,2013, code is without any warranty!
# Created:  11:13:40 15/04/2013
# Modified: 11:13:40 15/04/2013

# Created after: http://www.yolinux.com/TUTORIALS/LibraryArchives-StaticAndDynamic.html

CC = g++
SRCS = ctest1.c ctest2.c 
OBJS = $(SRCS:.c=.o)
DEPS = $(SRCS:.c=.o)
LIB_NAME = ctest
VERSION = 1.0
LDD_FILE = lib$(LIB_NAME).so.$(VERSION)
AR_FILE = lib$(LIB_NAME).a
PROGRAM = prog

LIB_LOCATION = $(PWD) 

.PHONY: dynamic clean all run_dynamic

all: run_dynamic run_static

clean:
	rm -f *.so *.so.[0-9] *.so.[0-9].[0-9] *.o $(PROGRAM) static_$(PROGRAM)

### Default patterns
%.o: %.c $(DEPEND)
	    $(CC) -Wall -fPIC -c -o $@ $<


### dynamic part 
dynamic: $(LDD_FILE)
	# Deploying - necessary for linking
	ln -sf $< `echo $< | sed -e 's:.[0-9]\.[0-9]::'`
	ln -sf $< `echo $< | sed -e 's:.[0-9]\.[0-9]:.2:'`

$(LDD_FILE): $(OBJS)
	$(CC) -shared -Wl,-soname,$@ -o $@ $^


# Compiling the main program
# exapmle usage gcc -Wall -I/path/to/include-files -L/path/to/libraries prog.c -lctest -o prog

$(PROGRAM): dynamic  $(PROGRAM).c
	$(CC) -Wall -L$(LIB_LOCATION) $(PROGRAM).c -l$(LIB_NAME) -o $@

run_dynamic: $(PROGRAM)
	@echo '======= Running dynamicaly linked program ====='
	LD_LIBRARY_PATH=$(PWD) $(PWD)/$<


### static part
$(AR_FILE): $(OBJS) 
	ar -cvq $@ $^

static_$(PROGRAM): $(PROGRAM).c  $(AR_FILE)
	$(CC) -o $@ $(PROGRAM).c -L$(PWD) -l$(LIB_NAME)

run_static: static_$(PROGRAM)
	@echo '======= Running staticly linked program ====='
	$(PWD)/$<