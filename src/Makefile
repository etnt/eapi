
MODULES = \
	cbuf \
	eapi \
	eapi_erl \
	eapi_c \
	eapi_drv

EBIN = ../ebin
ERLC = erlc

include ../vsn.mk
VSN=$(EAPI_VSN)

APP_NAME= eapi
APP_FILE= $(APP_NAME).app
APP_SRC= $(APP_FILE).src
APP_TARGET= $(EBIN)/$(APP_FILE)

override ERLC_FLAGS = 

OBJS = $(MODULES:%=$(EBIN)/%.beam)

TARGET_FILES = $(OBJS) $(APP_TARGET)

debug: ERLC_FLAGS += -Ddebug 

all: $(TARGET_FILES)

all32: $(TARGET_FILES)

debug: all

release: all

depend:
	edep -MM -o ../ebin $(ERLC_FLAGS) $(MODULES:%=%.erl) > depend.mk

dialyze:
	dialyzer --src -o dia.out $(ERLC_FLAGS) -c $(MODULES:%=%.erl) 

edoc:
	erl -noshell -pa doc -run edoc_run application "eapi" '".."' '[{private,true},no_packages,{sort_functions,false}]'
##erl -noshell -run edoc_run application "'$(APP_NAME)'"

clean:
	rm -f $(OBJS)

$(APP_TARGET): $(APP_SRC) ../vsn.mk
	sed -e 's;%VSN%;$(VSN);'  $< > $@

-include depend.mk

$(EBIN)/%.beam:	%.erl
	$(ERLC) $(ERLC_FLAGS) -o $(EBIN) $<
