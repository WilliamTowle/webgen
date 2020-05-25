#!/usr/bin/make
#	WmT, 19/06/2006

#	http://search.cpan.org/~abw/AppConfig/
#	http://search.cpan.org/CPAN/authors/id/A/AB/ABW/AppConfig-1.56.tar.gz
#APPCFG_VER=1.52
APPCFG_VER=1.56
APPCFG_SRC=src/AppConfig-${APPCFG_VER}.tar.gz
APPCFG_DIR=AppConfig-${APPCFG_VER}

#	http://search.cpan.org/~abw/Template-Toolkit/
#	http://search.cpan.org/CPAN/authors/id/A/AB/ABW/Template-Toolkit-2.15.tar.gz
#TPLTKT_VER=2.08
TPLTKT_VER=2.15
TPLTKT_DIR=Template-Toolkit-${TPLTKT_VER}
TPLTKT_SRC=src/Template-Toolkit-${TPLTKT_VER}.tar.gz


# ,-----
# |	Assistance
# +-----

.PHONY: default
default:
	@echo "Rules are:"
	@grep '^[A-Za-z][A-Za-z0-9\ -]*:' Makefile | sed 's/:[^:#]*/	/' | sed 's/^/	/'


# ,-----
# |	Builds
# +-----

.PHONY: zextract
zextract:
	[ "FILE${FILE}" != 'FILE' ]
	zcat ${FILE} | tar xvf -

.PHONY: appcfg-build
appcfg-build:
	${MAKE} zextract FILE=${APPCFG_SRC}
	( cd ${APPCFG_DIR} && perl Makefile.PL )
	( cd ${APPCFG_DIR} && make )

.PHONY: tpltkt-build
tpltkt-build:
	${MAKE} zextract FILE=${TPLTKT_SRC}
	yes | ( cd ${TPLTKT_DIR} && perl Makefile.PL )
	( cd ${TPLTKT_DIR} && make )

.PHONY: bin
bin:
	mkdir -p bin
	sed 's%.*Id: tpage.*% BEGIN { push @INC, "'`pwd`'/lib" } %' ${TPLTKT_DIR}/bin/tpage > bin/tpage
	chmod a+x bin/tpage
	sed 's%.*Id: ttree.*% BEGIN { push @INC, "'`pwd`'/lib" } %' ${TPLTKT_DIR}/bin/ttree > bin/ttree
	chmod a+x bin/ttree

.PHONY: lib
lib:
	mkdir -p lib
	find ${APPCFG_DIR}/* -name 'AppConfig*' | while read SF ; do cp -r $${SF} lib && chmod a+w -R lib ; done
	find ${APPCFG_DIR}/* -name 'auto' | while read SF ; do cp -r $${SF} lib && chmod a+w -R lib ; done
	find ${TPLTKT_DIR}/* -name 'Template*' | while read SF ; do cp -r $${SF} lib && chmod a+w -R lib ; done
	find ${TPLTKT_DIR}/* -name 'auto' | while read SF ; do cp -r $${SF} lib && chmod a+w -R lib ; done

.PHONY: build
build: appcfg-build tpltkt-build bin lib


# ,-----
# |	Tidiness
# +-----

.PHONY: clean
clean:
	-[ -z "${APPCFG_DIR}" ] || rm -rf ${APPCFG_DIR}
	-[ -z "${TPLTKT_DIR}" ] || rm -rf ${TPLTKT_DIR}

.PHONY: realclean
realclean: clean
	-rm -rf bin lib
