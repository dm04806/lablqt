GCC=g++ -c -pipe -g -Wall -W -D_REENTRANT `pkg-config --cflags QtGui`

CMOS=stub_helpers.cmo qtstubs.cmo
CMXS=stub_helpers.cmx qtstubs.cmx
C_STUBS=cstubs.o

BIND_CPP_OBJS=`find ./out/cpp/ -name "*.o"`
BIND_ML_OBJS=    out/stubs.cmo out/classes.cmo out/creators.cmo
BIND_ML_OBJS_OPT=out/stubs.cmx out/classes.cmx out/creators.cmx

.SUFFIXES: .cpp .ml .cmx .cmo

all: byte opt

byte: $(CMOS) kamlo $(C_STUBS) mklib

opt: $(CMXS) kamlo $(C_STUBS) mklib_opt

test:
	$(MAKE) -C test
	#$(MAKE) -C test4
	#$(MAKE) -C test5 depend all
	#$(MAKE) -C test6 depend all

kamlo:
	$(MAKE) -C out 

mklib:
	ocamlmklib -o lablqt `pkg-config --libs QtGui` -lstdc++ -lpthread  $(C_STUBS) $(CMOS) \
		$(BIND_CPP_OBJS) $(BIND_ML_OBJS) 

mklib_opt:
	ocamlmklib -o lablqt `pkg-config --libs QtGui` -lstdc++ -lpthread  $(C_STUBS) $(CMXS) \
		$(BIND_CPP_OBJS) $(BIND_ML_OBJS_OPT) 

.cpp.o:
	$(GCC) -c -I`ocamlc -where` $(COPTS) -fpic $<

.ml.cmx:
	ocamlopt -c $<

.ml.cmo:
	ocamlc -c $<

clean: clean_tests
	rm -f *.o *.a *.cm[iaox] *.cmxa dlllablqt.so 

clean_tests:
	$(MAKE) -C test  clean
	$(MAKE) -C test4 clean
	echo "" > test5/.depend
	$(MAKE) -C test5 clean
	echo "" > test6/.depend
	$(MAKE) -C test6 clean

