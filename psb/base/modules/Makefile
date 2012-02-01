#include ../../Make.inc

BASIC_MODS=
UTIL_MODS =

MODULES=$(BASIC_MODS) $(UTIL_MODS)

SRC=X_base_vect_mod.p90 X_base_mat_mod.p90 X_csc_mat_mod.p90 \
    X_csr_mat_mod.p90  X_mat_mod.p90  X_linmap_mod.p90 X_vect_mod.p90 \
    X_tools_mod.p90 X_psblas_mod.p90



FSRC1=$(patsubst %.p90, %.f90, $(subst X, psb_$(ARITH), $(filter %.p90, $(SRC))))
FSRC2=$(patsubst %.P90, %.F90, $(subst X, psb_$(ARITH), $(filter %.P90, $(SRC))))
FSRC=$(FSRC1) $(FSRC2) 
FOBJS1=$(FSRC1:.f90=.o)
FOBJS2=$(FSRC2:.F90=.o)
FOBJS=$(FOBJS1) $(FOBJS2)

LIBMOD=
# OBJS= $(SFOBJS) $(DFOBJS) $(CFOBJS) $(ZFOBJS)
# SRCS= $(SFSRC) $(DFSRC) $(CFSRC) $(ZFSRC)
LIBDIR=..
CINCLUDES=-I.
FINCLUDES=$(FMFLAG)$(LIBDIR) $(FMFLAG). $(FIFLAG).


src: ssingle sdouble scomplex sdcompl
ssingle:
	$(MAKE) bsrc ARITH=s
sdouble:
	$(MAKE) bsrc ARITH=d
scomplex:
	$(MAKE) bsrc ARITH=c
sdcompl:
	$(MAKE) bsrc ARITH=z

all:  single double complex dcompl

single:
	$(MAKE) lib ARITH=s
double:
	$(MAKE) lib ARITH=d
complex:
	$(MAKE) lib ARITH=c
dcompl:
	$(MAKE) lib ARITH=z

bsrc: $(FSRC)

lib: $(FOBJS)

X_map.p90:  X_linmap_mod.p90 
	touch $@
X_linmap_mod.p90:  X_mat_mod.p90 X_base_vect_mod.p90
	touch $@
X_base_mat_mod.p90: X_base_vect_mod.p90
	touch $@
X_vect_mod.p90: X_base_vect_mod.p90
	touch $@
X_csc_mat_mod.p90 X_csr_mat_mod.p90: X_base_mat_mod.p90 
	touch $@
X_mat_mod.p90: X_base_mat_mod.p90 X_csc_mat_mod.p90 X_csr_mat_mod.p90 
	touch $@

$(FSRC1): psb_$(ARITH)%.f90 : X%.p90
	sed -f ../../../$(ARITH).sed $< >$@
$(FSRC2): psb_$(ARITH)%.F90 : X%.P90 
	sed -f ../../../$(ARITH).sed $< >$@

clean:
	/bin/rm -f *.f90 *.F90 *.o 

