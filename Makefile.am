# -*- Mode: Makefile; -*-
#
# (C) 2011 by Argonne National Laboratory.
#     See COPYRIGHT in top-level directory.
#

ACLOCAL_AMFLAGS = -I confdb

# automake requires that we initialize variables to something, even just empty,
# before appending to them with "+="
AM_CPPFLAGS =
AM_FFLAGS =
AM_FCFLAGS =
include_HEADERS =
nodist_include_HEADERS =
nodist_noinst_HEADERS =
BUILT_SOURCES =
bin_PROGRAMS =
bin_SCRIPTS =
dist_bin_SCRIPTS =
dist_noinst_SCRIPTS = autogen.sh
noinst_DATA =
dist_noinst_DATA =
noinst_HEADERS =
noinst_LIBRARIES =
noinst_LTLIBRARIES =
sysconf_DATA =
dist_pkgdata_DATA =
nodist_pkgconfig_DATA =
CLEANFILES =
DISTCLEANFILES =
MAINTAINERCLEANFILES =
SUFFIXES = 
EXTRA_DIST =
DOC_SUBDIRS =
MANDOC_SUBDIRS =
HTMLDOC_SUBDIRS =
INSTALL_DATA_LOCAL_TARGETS =
doc1_src_txt =

# add (+=) target names to this variable to add them to the dependencies of the
# 'clean-local' target
CLEAN_LOCAL_TARGETS =

# used by nodist_pkgconfig_DATA
pkgconfigdir = @pkgconfigdir@

# contains all of the "errnames.txt" files that are used by maint/extracterrmsgs
# to build src/mpi/errhan/defmsg.h
errnames_txt_files = 

external_subdirs = @mplsrcdir@ @opasrcdir@
external_ldflags = @mpllibdir@ @opalibdir@
external_libs = @EXTERNAL_LIBS@
mpi_convenience_libs =
pmpi_convenience_libs = @mpllib@ @opalib@

# NOTE on our semi-unconventional usage of DIST_SUBDIRS:
# The automake manual recommends thinking of DIST_SUBDIRS as the list of all
# *configured* subdirectories.  The normal autotools model involves
# unconditional configure steps for all/most directories with conditional make
# steps (i.e., conditional inclusion in SUBDIRS).  But in the MPICH case we
# almost always configure exactly the directories that will be included in
# SUBDIRS.  So most additions to DIST_SUBDIRS should be conditional on the same
# condition as the corresponding SUBDIRS addition.
DIST_SUBDIRS = ${external_subdirs}
SUBDIRS = ${external_subdirs}

## Automake attempts to guess the correct linker among the various compilers
## for each language (see "How the Linker is Chosen" in the AM manual).
## However, this process is static and doesn't assume that you will "disable"
## Fortran support for a library and still actually build that library.
## lib@MPILIBNAME@.la contains both C and F77 source, so AM picks "F77LD" as the
## linker.  Instead we manually override automake's choice based on the value of
## enable_f77.
if BUILD_F77_BINDING
# link with libtool+F77LD
lib_lib@MPILIBNAME@_la_LINK = $(LIBTOOL) $(AM_V_lt) --tag=F77 \
	$(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link $(F77LD) \
	$(AM_FFLAGS) $(FFLAGS) $(lib_lib@MPILIBNAME@_la_LDFLAGS) \
	$(LDFLAGS) -o $@
lib_lib@PMPILIBNAME@_la_LINK = $(LIBTOOL) $(AM_V_lt) --tag=F77 \
	$(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link $(F77LD) \
	$(AM_FFLAGS) $(FFLAGS) $(lib_lib@PMPILIBNAME@_la_LDFLAGS) \
	$(LDFLAGS) -o $@
else !BUILD_F77_BINDING
# link with libtool+CCLD
lib_lib@MPILIBNAME@_la_LINK = $(LIBTOOL) $(AM_V_lt) --tag=CC \
	$(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link $(CCLD) \
	$(lib_lib@MPILIBNAME@_la_CFLAGS) $(CFLAGS) \
	$(lib_lib@MPILIBNAME@_la_LDFLAGS) $(LDFLAGS) -o $@
lib_lib@PMPILIBNAME@_la_LINK = $(LIBTOOL) $(AM_V_lt) --tag=CC \
	$(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link $(CCLD) \
	$(lib_lib@PMPILIBNAME@_la_CFLAGS) $(CFLAGS) \
	$(lib_lib@PMPILIBNAME@_la_LDFLAGS) $(LDFLAGS) -o $@
endif !BUILD_F77_BINDING

if BUILD_F77_BINDING
if BUILD_FC_BINDING
lib_lib@MPIFCLIBNAME@_la_LINK = $(LIBTOOL) $(AM_V_lt) --tag=FC \
	$(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link $(FCLD) \
	$(AM_FCFLAGS) $(FCFLAGS) $(lib_lib@MPIFCLIBNAME@_la_LDFLAGS) \
	$(LDFLAGS) -o $@
else !BUILD_FC_BINDING
lib_lib@MPIFCLIBNAME@_la_LINK = $(LIBTOOL) $(AM_V_lt) --tag=F77 \
	$(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --mode=link $(F77LD) \
	$(AM_FFLAGS) $(FFLAGS) $(lib_lib@MPIFCLIBNAME@_la_LDFLAGS) \
	$(LDFLAGS) -o $@
endif !BUILD_FC_BINDING
endif BUILD_F77_BINDING

## Create lists of files for different libraries
##
## mpi_sources: When compiled with -DMPICH_MPI_FROM_PMPI, these files
## provide "MPI_" public functions for the C bindings and end up in
## lib@MPILIBNAME@.  When compiled without this macro, these files
## provide "PMPI_" public functions for the C bindings as well as a
## few other internal MPICH functions, all of which end up in
## lib@PMPILIBNAME@ (which is the same as lib@MPILIBNAME@ on systems
## that support weak symbols).
##
## mpi_f77_sources: When compiled with -DMPICH_MPI_FROM_PMPI, these
## files provide the "mpi_" public functions for the F77 bindings and
## end up in lib@MPIFCLIBNAME@.
##
## mpi_fc_sources: When compiled with -DMPICH_MPI_FROM_PMPI, these
## files provide the "mpi_" public functions for the F90+ bindings and
## end up in lib@MPIFCLIBNAME@.
##
## mpi_cxx_sources: When compiled with -DMPICH_MPI_FROM_PMPI, these
## files provide the "MPI" public functions for the C++ bindings and
## end up in lib@MPICXXLIBNAME@.
##
## mpi_core_sources: These files are internal non-public functions
## that are used by all bindings.
mpi_sources =
mpi_f77_sources =
mpi_fc_sources =
mpi_fc_modules =
mpi_cxx_sources =
mpi_core_sources =

lib_LTLIBRARIES =
lib_lib@MPILIBNAME@_la_LIBADD =

# include our subdir automake fragments
include maint/Makefile.mk
include src/Makefile.mk

if BUILD_PROFILING_LIB
# dropping mpi_fc_sources and mpi_cxx_sources from libmpmpi since they
# don't contribute any PMPI symbols.
lib_LTLIBRARIES += lib/lib@PMPILIBNAME@.la
lib_lib@PMPILIBNAME@_la_SOURCES = $(mpi_sources) $(mpi_f77_sources) $(mpi_core_sources)
lib_lib@PMPILIBNAME@_la_LDFLAGS = $(external_ldflags) $(ABIVERSIONFLAGS)
lib_lib@PMPILIBNAME@_la_CPPFLAGS = $(AM_CPPFLAGS) -DF77_USE_PMPI
lib_lib@PMPILIBNAME@_la_LIBADD = $(external_libs) $(pmpi_convenience_libs)
EXTRA_lib_lib@PMPILIBNAME@_la_DEPENDENCIES = $(pmpi_convenience_libs)

# lib@MPILIBNAME@.la might depend on lib@PMPILIBNAME@.la.  We add them
# in that order to lib_LTLIBRARIES so libtool doesn't get
# confused. (see https://bugzilla.redhat.com/show_bug.cgi?id=91110)
lib_LTLIBRARIES += lib/lib@MPILIBNAME@.la
lib_lib@MPILIBNAME@_la_SOURCES = $(mpi_sources)
lib_lib@MPILIBNAME@_la_LDFLAGS = $(ABIVERSIONFLAGS)
lib_lib@MPILIBNAME@_la_CPPFLAGS = $(AM_CPPFLAGS) -DMPICH_MPI_FROM_PMPI
lib_lib@MPILIBNAME@_la_LIBADD += lib/lib@PMPILIBNAME@.la $(mpi_convenience_libs)

else !BUILD_PROFILING_LIB

lib_LTLIBRARIES += lib/lib@MPILIBNAME@.la
lib_lib@MPILIBNAME@_la_SOURCES = $(mpi_sources) $(mpi_core_sources)
lib_lib@MPILIBNAME@_la_LDFLAGS = $(external_ldflags) $(ABIVERSIONFLAGS)
lib_lib@MPILIBNAME@_la_CPPFLAGS = $(AM_CPPFLAGS)
lib_lib@MPILIBNAME@_la_LIBADD += $(external_libs) $(pmpi_convenience_libs) $(mpi_convenience_libs)
EXTRA_lib_lib@MPILIBNAME@_la_DEPENDENCIES = $(pmpi_convenience_libs) $(mpi_convenience_libs)
endif !BUILD_PROFILING_LIB

if BUILD_F77_BINDING
lib_LTLIBRARIES += lib/lib@MPIFCLIBNAME@.la
lib_lib@MPIFCLIBNAME@_la_CPPFLAGS = $(AM_CPPFLAGS)
if BUILD_PROFILING_LIB
lib_lib@MPIFCLIBNAME@_la_CPPFLAGS += -DMPICH_MPI_FROM_PMPI -DUSE_ONLY_MPI_NAMES
endif BUILD_PROFILING_LIB
lib_lib@MPIFCLIBNAME@_la_SOURCES = $(mpi_f77_sources)
if BUILD_FC_BINDING
modinc_HEADERS = $(mpi_fc_modules)
lib_lib@MPIFCLIBNAME@_la_SOURCES += $(mpi_fc_sources)
endif BUILD_FC_BINDING
lib_lib@MPIFCLIBNAME@_la_LDFLAGS = $(ABIVERSIONFLAGS)
lib_lib@MPIFCLIBNAME@_la_LIBADD = lib/lib@MPILIBNAME@.la
endif BUILD_F77_BINDING

if BUILD_CXX_BINDING
lib_LTLIBRARIES += lib/lib@MPICXXLIBNAME@.la
lib_lib@MPICXXLIBNAME@_la_SOURCES = $(mpi_cxx_sources)
lib_lib@MPICXXLIBNAME@_la_LDFLAGS = $(ABIVERSIONFLAGS)
lib_lib@MPICXXLIBNAME@_la_LIBADD = lib/lib@MPILIBNAME@.la
endif BUILD_CXX_BINDING

if BUILD_OSU_MVAPICH
SUBDIRS += contrib/hwloc
endif

# build the current directory and then the examples directory after everything else
SUBDIRS += . examples
if BUILD_OSU_MVAPICH
SUBDIRS += src/env/mpiname src/util/mv2_ckpt_rebuild osu_benchmarks
endif
DIST_SUBDIRS += . examples test

if BUILD_OSU_MVAPICH
AM_CPPFLAGS += -Icontrib/hwloc/include/ -I$(top_srcdir)/contrib/hwloc/include
lib_lib@MPILIBNAME@_la_LIBADD += contrib/hwloc/src/libhwloc_embedded.la
endif

EXTRA_DIST += README CHANGES COPYRIGHT README.envvar

testing:
	( cd test && $(MAKE) $(AM_MAKEFLAGS) testing )

test-clean:
	( cd test && $(MAKE) $(AM_MAKEFLAGS) clean )
CLEAN_LOCAL_TARGETS += test-clean

## FIXME: this should live in src/env/Makefile.mk. Moving it there
## will not cause a problem, but it will break the build if another
## Makefile.mk also decides to create a install-exec-hook target. A
## better approach might be to have each Makefile.mk append to a
## common set of rules.
install-exec-hook:
	for e in ${DESTDIR}${bindir}/@MPICC_NAME@ ${DESTDIR}${bindir}/@MPICXX_NAME@ \
		${DESTDIR}${bindir}/@MPIF77_NAME@ ${DESTDIR}${bindir}/@MPIFORT_NAME@ ; do \
		if test -e $${e} ; then \
			sed -e 's|__PREFIX_TO_BE_FILLED_AT_INSTALL_TIME__|${prefix}|g' \
				-e 's|__EXEC_PREFIX_TO_BE_FILLED_AT_INSTALL_TIME__|${exec_prefix}|g' \
				-e 's|__SYSCONFDIR_TO_BE_FILLED_AT_INSTALL_TIME__|${sysconfdir}|g' \
				-e 's|__INCLUDEDIR_TO_BE_FILLED_AT_INSTALL_TIME__|${includedir}|g' \
				-e 's|__LIBDIR_TO_BE_FILLED_AT_INSTALL_TIME__|${libdir}|g' $${e} > $${e}.tmp ; \
			$(INSTALL_SCRIPT) $${e}.tmp $${e} ; \
			rm -f $${e}.tmp ; \
		fi ; \
	done ; \
	if test -e ${DESTDIR}${bindir}/@MPICXX_NAME@ ; then \
		cd ${DESTDIR}${bindir} && ln -f -s @MPICXX_NAME@ @MPICPP_NAME@ ; fi ; \
	if test -e ${DESTDIR}${bindir}/@MPIFORT_NAME@ ; then \
		cd ${DESTDIR}${bindir} && ln -f -s @MPIFORT_NAME@ @MPIF90_NAME@ ; fi ; \
	if test -e ${DESTDIR}${bindir}/@MPIFORT_NAME@ -a ! -e ${DESTDIR}${bindir}/@MPIF77_NAME@ ; then \
		cd ${DESTDIR}${bindir} && ln -f -s @MPIFORT_NAME@ @MPIF77_NAME@ ; fi ; \
	if test -e ${DESTDIR}${libdir}/lib@MPILIBNAME@@SHLIB_EXT@ ; then \
		cd ${DESTDIR}${libdir} && ln -f -s lib@MPILIBNAME@@SHLIB_EXT@ libmpl@SHLIB_EXT@ ; \
		cd ${DESTDIR}${libdir} && ln -f -s lib@MPILIBNAME@@SHLIB_EXT@ libopa@SHLIB_EXT@ ; \
		cd ${DESTDIR}${libdir} && ln -f -s lib@MPILIBNAME@@SHLIB_EXT@ libmpich@SHLIB_EXT@ ; fi ; \
	if test -e ${DESTDIR}${libdir}/lib@MPIFCLIBNAME@@SHLIB_EXT@ ; then \
		cd ${DESTDIR}${libdir} && ln -f -s lib@MPIFCLIBNAME@@SHLIB_EXT@ libfmpich@SHLIB_EXT@ ; \
		cd ${DESTDIR}${libdir} && ln -f -s lib@MPIFCLIBNAME@@SHLIB_EXT@ libmpichf90@SHLIB_EXT@ ; fi ; \
	if test -e ${DESTDIR}${libdir}/lib@MPICXXLIBNAME@@SHLIB_EXT@ ; then \
		cd ${DESTDIR}${libdir} && ln -f -s lib@MPICXXLIBNAME@@SHLIB_EXT@ libmpichcxx@SHLIB_EXT@ ; fi

# the configure step checks for the 'lib' dir to decide whether a source
# directory has been previously used for an in-path build before allowing a
# VPATH build to continue
distclean-local:
	-rm -rf $(top_builddir)/lib
	-rm -rf $(top_builddir)/bin
	-rm -rf $(top_builddir)/include
	-rm -rf $(top_builddir)/share

# created by confdb/aclocal_cache.m4
DISTCLEANFILES += config.system

# we can only have one clean-local, so we hook into it via conditionally
# defined variables in the dependencies section
clean-local: $(CLEAN_LOCAL_TARGETS)

uninstall-local:
	for x in @MPICPP_NAME@ @MPIF90_NAME@ @MPIF77_NAME@ ; do \
		rm -f ${DESTDIR}${bindir}/$$x ; \
	done ; \
	for x in mpl opa mpich fmpich mpichf90 mpichcxx ; do \
		rm -f ${DESTDIR}${libdir}/lib$$x@SHLIB_EXT@ ; \
	done

# --------------------------------------------------------------------------
#  coverage rules
# --------------------------------------------------------------------------

.PHONY: coverage

if BUILD_COVERAGE

# $(lib_lib@MPILIBNAME@_la_SOURCES) already contains $(mpi_sources)
# We don't include the $(lib_lib@PMPILIBNAME@_la_SOURCES) here because of gcov's
# inability to correctly handle multiple object files with the same source file.
gcov_sources = $(lib_lib@MPILIBNAME@_la_SOURCES)

# local var meaning (for profiling foo/bar/baz.c):
#   dir=foo/bar
#   bname=baz.c
#   aux=baz
#
# Note that we delete all extra ".gcov" files that were not the direct goal,
# even though they contain useful information.  A more robust solution is needed
# in the long run, probably one that uses "--long-file-names --preserve-paths"
# and some postprocessing in a perl script.
#
# Gcov has no good way to deal with source files that have been compiled twice
# into two different object files, such as
#   bcast.c --> {bcast.o,lib_libpmpi_la-bcast.o}
# It still generates a bcast.c.gcov output for each object file.  Long file
# names and preserved paths do not help with this problem.
# What we can do in this case is to move the second files to a separate 
# directory, rename name, and run the GCOV on the new files.
# For the web-based coverage reports, the gcov output for the two files
# needs to be merged; see maint/gcovmerge .  
# To make it easier to re-run the coverage generation, we copy, rather than 
# move, when there are two object files for each source file.
coverage:
	@for file in $(gcov_sources) ; do \
	    dir=`dirname $$file` ; \
	    bname=`basename $$file` ; \
	    aux=`echo $$bname | sed -e 's,\.*$$,,'` ; \
	    echo "( $(GCOV) -b -f -o $$file $$file && mv $${bname}.gcov $$dir )" ; \
	    ( $(GCOV) -b -f -o $$file $$file && mv $${bname}.gcov $$dir ) ; \
	    sname=`echo $$bname | sed -e 's/\.[^\.]*$$//'` ;\
	    if [ -s "$$dir/lib_lib@PMPILIBNAME@_la-$$sname.o" ] ; then \
	        if [ ! -d $$dir-pmpi ] ; then mkdir $$dir-pmpi ; fi ; \
		for pfile in $$dir/lib_lib@PMPILIBNAME@_la-$$sname.* ; do \
		     nfile=`echo $$pfile | sed -e 's/lib_lib@PMPILIBNAME@_la-//'` ;\
		     nfile=`basename $$nfile` ; \
		     cp $$pfile $$dir-pmpi/$$nfile ; \
		     echo "( cd $$dir-pmpi && $(GCOV) -b -f $$nfile )" ; \
		     ( cd $$dir-pmpi && $(GCOV) -b -f $$nfile ) ; \
		     $(srcdir)/maint/gcovmerge $$dir-pmpi/$$bname.gcov \
				$$dir/$$bname.gcov > $$dir/$$bname.gcov.merge ;\
		done ; \
	    fi ; \
	    rm -f *.gcov ; \
	done
	for subdir in $(SUBDIRS) - ; do \
	    if test "x$$subdir" = "x-" ; then break ; fi ; \
## SUBDIRS contains "."
	    if test "x$$subdir" = "x." ; then \
	        : ; \
	    else \
		if grep coverage: $$subdir/Makefile >/dev/null 2>&1 ; then  \
	           ( cd $$subdir && $(MAKE) $(AM_MAKEFLAGS) coverage ) ; \
		fi \
	    fi \
	done

CLEAN_LOCAL_TARGETS += coverage-clean
# heavy handed, but it gets the job done
coverage-clean:
	-find $(top_builddir) -name "*.gcda" -o -name "*.gcno" -o -name "*.gcov" | \
	    xargs -n 10 rm -f

else !BUILD_COVERAGE
coverage:
	@echo 'ERROR: MPICH was not configured with "--enable-coverage"'
	exit 1
endif !BUILD_COVERAGE

# --------------------------------------------------------------------------
#  documentation rules
# --------------------------------------------------------------------------

# Tell make (via the automake SUFFIXES var) that our phony suffixes exist and
# should be considered when dissecting suffix-based implicit rules.  These phony
# suffix files should NOT be added to a .PHONY target, otherwise make will not
# use the implicit rules to attempt to build them.
SUFFIXES += .man-phony .html-phony .man1-phony .html1-phony .txt

# "make V=1" support for our documentation recipes
doctextman_verbose = $(doctextman_verbose_@AM_V@)
doctextman_verbose_ = $(doctextman_verbose_@AM_DEFAULT_V@)
doctextman_verbose_0 = @echo "   DOCTEXTMAN  " $@;
doctextman_verbose_1 =
doctexthtml_verbose = $(doctexthtml_verbose_@AM_V@)
doctexthtml_verbose_ = $(doctexthtml_verbose_@AM_DEFAULT_V@)
doctexthtml_verbose_0 = @echo "   DOCTEXTHTML  " $@;
doctexthtml_verbose_1 =

# build dir paths for doc generation, used to override subdir (esp. ROMIO) doc
# generation locations/options
mandoc_path1=$(abs_top_builddir)/man/man1
mandoc_path3=$(abs_top_builddir)/man/man3
htmldoc_path1=$(abs_top_builddir)/www/www1
htmldoc_path3=$(abs_top_builddir)/www/www3
doctext_docnotes=$(abs_top_srcdir)/maint/docnotes
# Provide an easily replaced url root for the generated index file.
# You can override this with URL desired in the index file generated by doctext.
# You can ignore this if you don't use mapnames or tohtml to add links
# to the MPI manual pages to documents.
htmldoc_root3="--your-url-here--"

# Implicit suffix rules to cause the docs to be built.  These are a bit of a
# hack, but using the implicit rules like this instead of a shell for loop
# permits make -jN to work.
.c.man-phony:
	$(doctextman_verbose)$(DOCTEXT) -man -mpath $(mandoc_path3) -ext 3 \
	    -heading MPI -quotefmt -nolocation $(doctext_docnotes) $<
.c.html-phony:
	$(doctexthtml_verbose)$(DOCTEXT) -html -mpath $(htmldoc_path3) \
	    -heading MPI -quotefmt -nolocation \
	    -index $(htmldoc_path3)/mpi.cit -indexdir $(htmldoc_root3) \
		$(doctext_docnotes) $<

.txt.man1-phony:
	$(doctextman_verbose)$(DOCTEXT) -man -mpath $(mandoc_path1) -ext 1 \
	    -heading MPI -quotefmt -nolocation $(doctext_docnotes) $<
.txt.html1-phony:
	$(doctexthtml_verbose)$(DOCTEXT) -html -mpath $(htmldoc_path1) \
	    -heading MPI -quotefmt -nolocation $(doctext_docnotes) $<

# use mandoc-local target to force directory creation before running DOCTEXT
mandoc:
	test -d $(mandoc_path1) || $(MKDIR_P) $(mandoc_path1)
	test -d $(mandoc_path3) || $(MKDIR_P) $(mandoc_path3)
	$(MAKE) $(AM_MAKEFLAGS) mandoc-local
mandoc-local: $(mpi_sources:.c=.man-phony) $(doc1_src_txt:.txt=.man1-phony)
	for subdir in $(MANDOC_SUBDIRS) - ; do \
	    if test "x$$subdir" = "x-" ; then break ; fi ; \
## MANDOC_SUBDIRS might accidentally contain "."
	    if test "x$$subdir" = "x." ; then \
	        : ; \
	    else \
		if grep 'mandoc:' $$subdir/Makefile >/dev/null 2>&1 ; then  \
## make 'mandoc' in subdirectory but tell sub-make to output to the top-level
	           ( cd $$subdir && $(MAKE) $(AM_MAKEFLAGS) \
			mandoc_path1=$(mandoc_path1) \
			mandoc_path3=$(mandoc_path3) \
			doctext_docnotes=$(doctext_docnotes) \
			mandoc ) ; \
		fi \
	    fi \
	done


# use htmldoc-local target to force directory creation before running DOCTEXT
htmldoc:
	test -d $(top_builddir)/www/www1 || $(MKDIR_P) $(top_builddir)/www/www1
	test -d $(top_builddir)/www/www3 || $(MKDIR_P) $(top_builddir)/www/www3
	$(MAKE) $(AM_MAKEFLAGS) htmldoc-local
htmldoc-local: $(mpi_sources:.c=.html-phony) $(doc1_src_txt:.txt=.html1-phony)
	for subdir in $(HTMLDOC_SUBDIRS) - ; do \
	    if test "x$$subdir" = "x-" ; then break ; fi ; \
## HTMLDOC_SUBDIRS might accidentally contain "."
	    if test "x$$subdir" = "x." ; then \
	        : ; \
	    else \
		if grep 'htmldoc:' $$subdir/Makefile >/dev/null 2>&1 ; then  \
## make 'htmldoc' in subdirectory but tell sub-make to output to the top-level
	           ( cd $$subdir && $(MAKE) $(AM_MAKEFLAGS) \
			htmldoc_path1=$(htmldoc_path1) \
			htmldoc_path3=$(htmldoc_path3) \
			doctext_docnotes=$(doctext_docnotes) \
			htmldoc ) ; \
		fi \
	    fi \
	done
	$(srcdir)/maint/createhtmlindex -wwwroot=$(top_builddir)

# install-man does not have a special "-local" target for some reason
INSTALL_DATA_LOCAL_TARGETS += install-man-local
# this is a variation on the recipe that was previously generated by simplemake
install-man-local:
	@dir= ; \
	if test -d $(builddir)/man ; then \
	 dir=$(builddir)/man ; \
	elif test -d $(srcdir)/man ; then \
	 dir=$(srcdir)/man ; \
	fi ; \
	export dir ; \
	if test ! -z $$dir && test -d $$dir && cd $$dir ; then \
	 for name in * ; do \
	  if [ "$$name" = "*" ] ; then continue ; fi ; \
	  if [ -f $$name ] ; then \
	    echo "$(INSTALL_DATA) $$name ${DESTDIR}${mandir}/$$name" ; \
	    $(INSTALL_DATA) $$name ${DESTDIR}${mandir}/$$name ; \
	  elif [ -d $$name ] ; then \
	    if [ ! -d ${DESTDIR}${mandir}/$$name ] ; then $(MKDIR_P) ${DESTDIR}${mandir}/$$name ; fi ;\
	    ( cd $$name && for name2 in * ; do \
	        if [ "$$name2" = "*" ] ; then continue ; fi ; \
	        if [ -f $$name2 ] ; then \
	            echo "$(INSTALL_DATA) $$name2 ${DESTDIR}${mandir}/$$name/$$name2" ; \
	            $(INSTALL_DATA) $$name2 ${DESTDIR}${mandir}/$$name/$$name2 ; \
	        elif [ -d $$name2 ] ; then  \
	            echo "cp -rp $$name2 ${DESTDIR}${mandir}/$$name" ; \
	            cp -rp $$name2 ${DESTDIR}${mandir}/$$name ; \
	        fi \
	    done ) ; \
	  else \
	    echo "Unknown file type for $$name" ; \
	  fi ; \
	 done ; \
	fi

INSTALL_DATA_LOCAL_TARGETS += install-html-local
install-html-local:
	@dir= ; \
	if test -d $(builddir)/www ; then \
	 dir=$(builddir)/www ; \
	elif test -d $(srcdir)/www ; then \
	 dir=$(srcdir)/www ; \
	fi ; \
	export dir ; \
	if test ! -z $$dir && test -d $$dir && cd $$dir ; then \
	 for name in * ; do \
	  if [ "$$name" = "*" ] ; then continue ; fi ; \
	  if [ -f $$name ] ; then \
	    echo "$(INSTALL_DATA) $$name ${DESTDIR}${htmldir}/$$name" ; \
	    $(INSTALL_DATA) $$name ${DESTDIR}${htmldir}/$$name ; \
	  elif [ -d $$name ] ; then \
	    if [ ! -d ${DESTDIR}${htmldir}/$$name ] ; then $(MKDIR_P) ${DESTDIR}${htmldir}/$$name ; fi ;\
	    ( cd $$name && for name2 in * ; do \
	        if [ "$$name2" = "*" ] ; then continue ; fi ; \
	        if [ -f $$name2 ] ; then \
	            echo "$(INSTALL_DATA) $$name2 ${DESTDIR}${htmldir}/$$name/$$name2" ; \
	            $(INSTALL_DATA) $$name2 ${DESTDIR}${htmldir}/$$name/$$name2 ; \
	        elif [ -d $$name2 ] ; then  \
	            echo "cp -rp $$name2 ${DESTDIR}${htmldir}/$$name" ; \
	            cp -rp $$name2 ${DESTDIR}${htmldir}/$$name ; \
	        fi \
	    done ) ; \
	  else \
	    echo "Unknown file type for $$name" ; \
	  fi ; \
	 done ; \
	fi

.PHONY: mandoc mandoc-local htmldoc htmldoc-local install-html-local install-man-local

# build the latex docs in the old style
latexdoc:
	for dir in $(DOC_SUBDIRS) ; do \
		(cd $$dir && ${MAKE} latexdoc ) ; done

# install-pdf will not be run by default in a "make install" unless we do this
INSTALL_DATA_LOCAL_TARGETS += install-pdf-local
# note that these PDFs are currently built in the srcdir, even in a VPATH
# build...
install-pdf-local:
	if [ ! -e ${DESTDIR}${docdir} ] ; then $(MKDIR_P) ${DESTDIR}${docdir} ; fi
	if [ -s $(srcdir)/doc/userguide/user.pdf ] ; then $(INSTALL_DATA) $(srcdir)/doc/userguide/user.pdf ${DESTDIR}${docdir}/user.pdf ; fi
	if [ -s $(srcdir)/doc/installguide/install.pdf ] ; then $(INSTALL_DATA) $(srcdir)/doc/installguide/install.pdf ${DESTDIR}${docdir}/install.pdf ; fi
	if [ -s $(srcdir)/doc/logging/logging.pdf ] ; then $(INSTALL_DATA) $(srcdir)/doc/logging/logging.pdf ${DESTDIR}${docdir}/logging.pdf ; fi

include $(top_srcdir)/doc/Makefile.mk

# use the _TARGETS var as a workaround for being unable to define multiple
# -local targets
install-data-local: $(INSTALL_DATA_LOCAL_TARGETS)

