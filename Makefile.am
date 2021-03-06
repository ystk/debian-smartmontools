## Process this file with automake to produce Makefile.in
#
# $Id: Makefile.am 3115 2010-06-02 17:23:05Z chrfranke $
#

@SET_MAKE@

# Make sure .cpp takes precedence to avoid compiling old .c file
SUFFIXES = .cpp .c .s .o

# BUILD_INFO can be provided by package maintainers (see INSTALL file)
BUILD_INFO= "(local build)"

AM_CPPFLAGS = -DBUILD_INFO='$(BUILD_INFO)' -DSMARTMONTOOLS_SYSCONFDIR='"$(sysconfdir)"'
if ENABLE_DRIVEDB
AM_CPPFLAGS += -DSMARTMONTOOLS_DRIVEDBDIR='"$(drivedbdir)"'
endif
if ENABLE_SAVESTATES
AM_CPPFLAGS += -DSMARTMONTOOLS_SAVESTATES='"$(savestates)"'
endif
if ENABLE_ATTRIBUTELOG
AM_CPPFLAGS += -DSMARTMONTOOLS_ATTRIBUTELOG='"$(attributelog)"'
endif

if NEED_GETOPT_LONG
AM_CPPFLAGS += -I$(srcdir)/getopt -DHAVE_GETOPT_LONG -D__GNU_LIBRARY__
endif
if NEED_REGEX
AM_CPPFLAGS += -I$(srcdir)/regex
endif

sbin_PROGRAMS = smartd 	\
		smartctl

if ENABLE_DRIVEDB
sbin_SCRIPTS = update-smart-drivedb
endif


smartd_SOURCES =  smartd.cpp      \
                  atacmdnames.cpp \
                  atacmdnames.h   \
                  atacmds.cpp     \
                  atacmds.h       \
                  dev_ata_cmd_set.cpp \
                  dev_ata_cmd_set.h   \
                  dev_interface.cpp   \
                  dev_interface.h     \
                  dev_tunnelled.h     \
                  drivedb.h           \
                  extern.h        \
                  int64.h         \
                  knowndrives.cpp \
                  knowndrives.h   \
                  scsicmds.cpp    \
                  scsicmds.h      \
                  scsiata.cpp     \
                  utility.cpp     \
                  utility.h

smartd_LDADD = @os_deps@ @os_libs@ @CAPNG_LDADD@
smartd_DEPENDENCIES = @os_deps@

EXTRA_smartd_SOURCES = os_darwin.cpp    \
                       os_darwin.h      \
                       os_linux.cpp     \
                       os_linux.h       \
                       os_freebsd.cpp   \
                       os_freebsd.h     \
                       os_netbsd.cpp    \
                       os_netbsd.h      \
                       os_openbsd.cpp   \
                       os_openbsd.h     \
                       os_qnxnto.cpp    \
                       os_qnxnto.h      \
                       os_solaris.cpp   \
                       os_solaris.h     \
                       os_solaris_ata.s \
                       os_win32.cpp     \
                       os_generic.cpp   \
                       os_generic.h     \
                       cciss.cpp        \
                       cciss.h          \
                       dev_legacy.cpp   \
                       megaraid.h


if OS_WIN32_MINGW

smartd_SOURCES += \
        os_win32/daemon_win32.cpp \
        os_win32/daemon_win32.h \
        os_win32/hostname_win32.cpp \
        os_win32/hostname_win32.h \
        os_win32/syslog_win32.cpp \
        os_win32/syslog.h

endif

smartctl_SOURCES= smartctl.cpp    \
                  smartctl.h      \
                  atacmdnames.cpp \
                  atacmdnames.h   \
                  atacmds.cpp     \
                  atacmds.h       \
                  ataprint.cpp    \
                  ataprint.h      \
                  dev_ata_cmd_set.cpp \
                  dev_ata_cmd_set.h   \
                  dev_interface.cpp   \
                  dev_interface.h     \
                  dev_tunnelled.h     \
                  drivedb.h           \
                  extern.h        \
                  int64.h         \
                  knowndrives.cpp \
                  knowndrives.h   \
                  scsicmds.cpp    \
                  scsicmds.h      \
                  scsiata.cpp     \
                  scsiprint.cpp   \
                  scsiprint.h     \
                  utility.cpp     \
                  utility.h


smartctl_LDADD = @os_deps@ @os_libs@
smartctl_DEPENDENCIES = @os_deps@

EXTRA_smartctl_SOURCES = os_linux.cpp \
                       os_linux.h     \
                       os_freebsd.cpp \
                       os_freebsd.h   \
                       os_netbsd.cpp  \
                       os_netbsd.h    \
                       os_openbsd.cpp \
                       os_openbsd.h   \
                       os_qnxnto.cpp  \
                       os_qnxnto.h    \
                       os_solaris.cpp \
                       os_solaris.h   \
                       os_win32.cpp   \
                       os_generic.cpp \
                       os_generic.h   \
                       cciss.cpp      \
                       cciss.h        \
                       dev_legacy.cpp \
                       megaraid.h

if NEED_GETOPT_LONG

smartctl_SOURCES += \
        getopt/getopt.c \
        getopt/getopt.h \
        getopt/getopt1.c

smartd_SOURCES += \
        getopt/getopt.c \
        getopt/getopt.h \
        getopt/getopt1.c

endif

if NEED_REGEX

smartctl_SOURCES += \
        regex/regex.c \
        regex/regex.h \
        regex/regex_internal.h

smartd_SOURCES += \
        regex/regex.c \
        regex/regex.h \
        regex/regex_internal.h

# Included by regex.c:
EXTRA_smartctl_SOURCES += \
        regex/regcomp.c \
        regex/regexec.c \
        regex/regex_internal.c

EXTRA_smartd_SOURCES += \
        regex/regcomp.c \
        regex/regexec.c \
        regex/regex_internal.c

endif

if OS_SOLARIS
# This block is required because Solaris uses manual page section 1m
# for administrative command (linux/freebsd use section 8) and Solaris
# uses manual page section 4 for file formats (linux/freebsd use
# section 5).  Automake can deal cleanly with man page sections 1-8
# and n, but NOT with sections of the form 1m.
extra_MANS =      smartd.conf.4 \
                  smartctl.1m   \
                  smartd.1m
install-man: $(extra_MANS)
	@$(NORMAL_INSTALL)
	$(mkinstalldirs) $(DESTDIR)$(mandir)/man4
	$(mkinstalldirs) $(DESTDIR)$(mandir)/man1m
	for i in $(extra_MANS); do \
	  if test -f $(srcdir)/$$i; then file=$(srcdir)/$$i; \
	  else file=$$i; fi; \
	  ext=`echo $$i | sed -e 's/^.*\\.//'`; \
	  inst=`echo $$i | sed -e 's/\\.[0-9a-z]*$$//'`; \
	  inst=`echo $$inst | sed -e 's/^.*\///'`; \
	  inst=`echo $$inst | sed '$(transform)'`.$$ext; \
	  echo " $(INSTALL_DATA) $$file $(DESTDIR)$(mandir)/man$$ext/$$inst"; \
	  $(INSTALL_DATA) $$file $(DESTDIR)$(mandir)/man$$ext/$$inst; \
	done
uninstall-man:
	@$(NORMAL_UNINSTALL)
	for i in $(extra_MANS); do \
	  if test -f $(srcdir)/$$i; then file=$(srcdir)/$$i; \
	  else file=$$i; fi; \
	  ext=`echo $$i | sed -e 's/^.*\\.//'`; \
	  inst=`echo $$i | sed -e 's/\\.[0-9a-z]*$$//'`; \
	  inst=`echo $$inst | sed -e 's/^.*\///'`; \
	  inst=`echo $$inst | sed '$(transform)'`.$$ext; \
	  echo " rm -f $(DESTDIR)$(mandir)/man$$ext/$$inst"; \
	  rm -f $(DESTDIR)$(mandir)/man$$ext/$$inst; \
	done
%.1m: %.8
	awk '/^.TH/ {$$3="1m"} {print}' < $< | \
	sed -e 's/smartd\.conf\(.*\)(5)/smartd.conf\1(4)/g' \
            -e 's/syslog\.conf\(.*\)(5)/syslog.conf\1(4)/g' \
	    -e 's/smartctl\(.*\)(8)/smartctl\1(1m)/g' \
	    -e 's/syslogd\(.*\)(8)/syslogd\1(1m)/g' \
            -e 's|/var/log/messages|/var/adm/messages|g' \
	    -e 's/smartd\(.*\)(8)/smartd\1(1m)/g' > $@
%.4: %.5
	awk '/^.TH/ {$$3="4"}  {print}' < $< | \
	sed -e 's/smartd\.conf\(.*\)(5)/smartd.conf\1(4)/g' \
            -e 's/syslog\.conf\(.*\)(5)/syslog.conf\1(4)/g' \
	    -e 's/smartctl\(.*\)(8)/smartdctl\1(1m)/g' \
	    -e 's/syslogd\(.*\)(8)/syslogd\1(1m)/g' \
            -e 's|/var/log/messages|/var/adm/messages|g' \
	    -e 's/smartd\(.*\)(8)/smartd\1(1m)/g' > $@
else
# For systems that adopts traditional manner
man_MANS =        smartd.conf.5 \
                  smartctl.8    \
                  smartd.8
endif

docsdir=$(docdir)
docs_DATA = AUTHORS     \
            CHANGELOG   \
            COPYING     \
            INSTALL     \
            NEWS        \
            README      \
            TODO        \
            WARNINGS    \
            smartd.conf

sysconf_DATA = smartd.conf$(smartd_suffix)

if SMARTD_SUFFIX
smartd.conf$(smartd_suffix): smartd.conf
	cp ${srcdir}/smartd.conf smartd.conf$(smartd_suffix)
endif

EXTRA_DIST = smartd.initd.in                       \
             smartd.8.in                           \
             smartctl.8.in                         \
             smartd.conf.5.in                      \
             smartd.conf                           \
             autogen.sh                            \
             update-smart-drivedb.in               \
             os_darwin/SMART.in                    \
             os_darwin/StartupParameters.plist     \
             os_darwin/English_Localizable.strings \
             os_win32/installer.nsi                \
             $(docs_DATA)

CLEANFILES = smartd.conf.5      \
             smartd.conf.4      \
             smartd.8           \
             smartd.1m          \
             smartd.8.html      \
             smartd.8.txt       \
             smartctl.8         \
             smartctl.1m        \
             smartctl.8.html    \
             smartctl.8.txt     \
             smartd.conf.5.html \
             smartd.conf.5.txt  \
             smartd.initd       \
             svnversion.h       \
             update-smart-drivedb \
             SMART

if SMARTD_SUFFIX
CLEANFILES += smartd.conf$(smartd_suffix)
endif

# 'make maintainer-clean' also removes files generated by './autogen.sh'
MAINTAINERCLEANFILES = \
        $(srcdir)/Makefile.in \
        $(srcdir)/aclocal.m4 \
        $(srcdir)/configure \
        $(srcdir)/config.guess \
        $(srcdir)/config.h.in \
        $(srcdir)/config.h.in~ \
        $(srcdir)/config.sub \
        $(srcdir)/depcomp \
        $(srcdir)/install-sh \
        $(srcdir)/missing \
        $(srcdir)/mkinstalldirs

utility.o: svnversion.h

if IS_SVN_BUILD
# Get version info from SVN
svnversion.h: CHANGELOG Makefile $(srcdir)/.svn/entries
	echo '/* svnversion.h.  Generated by Makefile from svn info.  */' > $@
	(cd $(srcdir) \
	 && svnversion 2>/dev/null | sed -n 's,^\([0-9].*\),REV  "\1",p' \
	 && TZ= LC_ALL=C svn info 2>/dev/null \
	 | sed -n 'h;s,^.* Date: *\([^ ]*\) .*$$,DATE "\1",p;g;s,^.* Date: *[^ ]* *\([^ ]*\) .*$$,TIME "\1",p') \
	| sed 's,^,#define SMARTMONTOOLS_SVN_,' >> $@
else

# SVN not available, guess version info from Id strings
svnversion.h: CHANGELOG Makefile
	echo '/* svnversion.h.  Generated by Makefile from Id strings.  */' > $@
	(cd $(srcdir) && cat CHANGELOG Makefile.am configure.in smart*.in *.cpp *.h *.s) \
	| sed -n 's,^.*\$$[I][d]: [^ ]* \([0-9][0-9]* [0-9][-0-9]* [0-9][:0-9]*\)[^:0-9][^$$]*\$$.*$$,\1,p' \
	| sort -n -r \
	| sed -n 'h;s,^\([^ ]*\) .*$$,REV  "\1",p;g;s,^[^ ]* \([^ ]*\) .*$$,DATE "\1",p;g;s,^[^ ]* [^ ]* \([^ ]*\)$$,TIME "\1",p;q' \
	| sed 's,^,#define SMARTMONTOOLS_SVN_,' >> $@
endif


if ENABLE_DRIVEDB
drivedb_DATA = drivedb.h
endif

if ENABLE_SAVESTATES
# Create $(savestatesdir) only
savestates_DATA =
endif

if ENABLE_ATTRIBUTELOG
# Create $(attributelogdir) only
attributelog_DATA =
endif

smartd.conf.5.in: smartd.8.in
	sed '1,/STARTINCLUDE/ D;/ENDINCLUDE/,$$D' < $(srcdir)/smartd.8.in > $(top_builddir)/tmp.directives
	sed '/STARTINCLUDE/,$$D'  < $(srcdir)/smartd.conf.5.in > $(top_builddir)/tmp.head
	sed '1,/ENDINCLUDE/D'   < $(srcdir)/smartd.conf.5.in > $(top_builddir)/tmp.tail
	cat $(top_builddir)/tmp.head > $(srcdir)/smartd.conf.5.in
	echo '.\" STARTINCLUDE' >> $(srcdir)/smartd.conf.5.in
	cat $(top_builddir)/tmp.directives >> $(srcdir)/smartd.conf.5.in
	echo '.\" ENDINCLUDE'   >> $(srcdir)/smartd.conf.5.in
	cat $(top_builddir)/tmp.tail >> $(srcdir)/smartd.conf.5.in
	rm -f $(top_builddir)/tmp.head $(top_builddir)/tmp.tail $(top_builddir)/tmp.directives

update-smart-drivedb: update-smart-drivedb.in config.status
	$(SHELL) ./config.status --file=$@
	chmod +x $@


if INSTALL_INITSCRIPT
if OS_DARWIN
initd_DATA = SMART                            \
	os_darwin/StartupParameters.plist     \
	os_darwin/English_Localizable.strings

initd_install_name = SMART

initd_DATA_install = install-initdDATA-darwin
initd_DATA_uninstall = uninstall-initdDATA-darwin

SMART : os_darwin/SMART.in
	sed "s|/usr/sbin/|$(sbindir)/|" $< > $@

install-initdDATA-darwin: $(initd_DATA)
	$(mkinstalldirs) $(DESTDIR)$(initddir)
	$(mkinstalldirs) $(DESTDIR)$(initddir)/SMART
	$(mkinstalldirs) $(DESTDIR)$(initddir)/SMART/Resources
	$(INSTALL_SCRIPT) $(top_builddir)/SMART $(DESTDIR)$(initddir)/SMART
	$(INSTALL_DATA) $(srcdir)/os_darwin/StartupParameters.plist \
	    $(DESTDIR)$(initddir)/SMART/StartupParameters.plist
	for i in English ; do \
	  RDIR=$(DESTDIR)$(initddir)/SMART/Resources/$${i}.lproj ; \
	  $(mkinstalldirs) $$RDIR ;\
	  $(INSTALL_DATA) $(srcdir)/os_darwin/$${i}_Localizable.strings \
	    $$RDIR/Localizable.strings ; \
	done
	@echo -e "\n\n####################################################################\n#"
	@echo -e "#                       PLEASE READ THIS BOX!\n#"
	@echo -e "#   To manually start the smartd daemon, run:\n#   ${initddir}/SMART/SMART start\n#"
	@echo -e "#   To automatically start smartd on bootup, add the line:\n#   SMARTd=-YES-\n#   to /etc/hostconfig\n#"
	@echo -e "#   smartd can now use a configuration file ${sysconfdir}/smartd.conf. Do:\n#   man smartd"
	@echo -e "#   to learn about it. A sample configuration file can be found in:\n#   ${docdir}\n#"
	@echo -e "####################################################################\n\n"

uninstall-initdDATA-darwin:
	rm -rf $(DESTDIR)$(initddir)/$(initd_install_name)

else

initd_DATA = smartd.initd

smartd.initd: $(srcdir)/smartd.initd.in Makefile
	sed "s|/usr/local/sbin/|$(sbindir)/|g" $(srcdir)/smartd.initd.in > $@

initd_install_name = smartd$(smartd_suffix)

initd_DATA_install = install-initdDATA-generic
initd_DATA_uninstall = uninstall-initdDATA-generic

install-initdDATA-generic: $(initd_DATA)
	$(mkinstalldirs) $(DESTDIR)$(initddir)
	$(INSTALL_SCRIPT) $(top_builddir)/smartd.initd $(DESTDIR)$(initddir)/smartd$(smartd_suffix)
	@echo -e "\n\n####################################################################\n#"
	@echo -e "#                       PLEASE READ THIS BOX!\n#"
	@echo -e "#   To manually start the smartd daemon, run:\n#   ${initddir}/smartd start\n#"
	@echo -e "#   To automatically start smartd on bootup, run:\n#   /sbin/chkconfig --add smartd\n#"
	@echo -e "#   smartd can now use a configuration file ${sysconfdir}/smartd.conf. Do:\n#   man smartd"
	@echo -e "#   to learn about it. A sample configuration file can be found in:\n#   ${docdir}\n#"
	@echo -e "####################################################################\n\n"


uninstall-initdDATA-generic:
	rm -rf $(DESTDIR)$(initddir)/$(initd_install_name)
endif
else

initd_DATA_install = install-initdDATA-null
initd_DATA_uninstall = uninstall-initdDATA-null

install-initdDATA-null:
uninstall-initdDATA-null:
endif

install-initdDATA : $(initd_DATA_install)

uninstall-initdDATA: $(initd_DATA_uninstall)

if ENABLE_CAPABILITIES
MAN_CAPABILITIES = cat
else
MAN_CAPABILITIES = sed '/BEGIN ENABLE_CAPABILITIES/,/END ENABLE_CAPABILITIES/d'
endif

if ENABLE_DRIVEDB
MAN_DRIVEDB = sed "s|/usr/local/share/smartmontools/drivedb\\.h|$(drivedbdir)/drivedb.h|g"
else
MAN_DRIVEDB = sed '/BEGIN ENABLE_DRIVEDB/,/END ENABLE_DRIVEDB/d'
endif

if ENABLE_SAVESTATES
MAN_SAVESTATES = sed "s|/usr/local/var/lib/smartmontools/smartd\\.|$(savestates)|g"
else
MAN_SAVESTATES = sed '/BEGIN ENABLE_SAVESTATES/,/END ENABLE_SAVESTATES/d'
endif

if ENABLE_ATTRIBUTELOG
MAN_ATTRIBUTELOG = sed "s|/usr/local/var/lib/smartmontools/attrlog\\.|$(attributelog)|g"
else
MAN_ATTRIBUTELOG = sed '/BEGIN ENABLE_ATTRIBUTELOG/,/END ENABLE_ATTRIBUTELOG/d'
endif

MAN_FILTER = \
    sed "s|CURRENT_SVN_VERSION|$(releaseversion)|g; \
         s|CURRENT_SVN_DATE|`sed -n 's,^.*DATE[^"]*"\([^"]*\)".*$$,\1,p' svnversion.h`|g; \
         s|CURRENT_SVN_REV|`sed -n 's,^.*REV[^"]*"\([^"]*\)".*$$,r\1,p' svnversion.h`|g; \
         s|/usr/local/share/man/|$(mandir)/|g; \
         s|/usr/local/sbin/|$(sbindir)/|g; \
         s|/usr/local/etc/rc\\.d/init.d/|$(initddir)/|g; \
         s|/usr/local/share/doc/smartmontools/|$(docsdir)/|g;  \
         s|/usr/local/etc/smartd\\.conf|$(sysconfdir)/smartd.conf|g; \
         s|/usr/local/etc/smart_drivedb\\.h|$(sysconfdir)/smart_drivedb\\.h|g" | \
    $(MAN_CAPABILITIES) | \
    $(MAN_DRIVEDB) | \
    $(MAN_SAVESTATES) | \
    $(MAN_ATTRIBUTELOG)

# Implicit rule 'smart%: smart%.in ...' does not work with BSD make
smartctl.8: smartctl.8.in Makefile svnversion.h
	cat $(srcdir)/smartctl.8.in | $(MAN_FILTER) > $@

smartd.8: smartd.8.in Makefile svnversion.h
	cat $(srcdir)/smartd.8.in | $(MAN_FILTER) > $@

smartd.conf.5: smartd.conf.5.in Makefile svnversion.h
	cat $(srcdir)/smartd.conf.5.in | $(MAN_FILTER) > $@

# Commands to convert man pages into .html and .txt
# TODO: configure
MAN2HTML = man2html
#MAN2HTML = groff -man -Thtml
MAN2TXT = groff -man -Tascii -P'-bcou'

# Remove HTTP header and fix links in man2html output
FIXHTML = sed '1s,^Content-type.*,,' \
        | sed 's,<A HREF="http://[-a-z/]*/man2html?\([1-8]\)+\(smart[cd][.a-z]*\)">,<A HREF="\2.\1.html">,g' \
        | sed 's,<A HREF="http://[-a-z/]*/man2html">,<A HREF=".">,g' \
        | sed 's,<A HREF="http://[-a-z/]*/man2html?[^"]*">\([^<]*\)</A>,\1,g' \
        | sed 's,<A HREF="mailto:[^s][^m][^a][^"]*">\([^<]*\)</A>,\1,g'

# Convert man pages into .html and .txt

htmlman: smartctl.8.html smartd.8.html smartd.conf.5.html

txtman:  smartctl.8.txt smartd.8.txt smartd.conf.5.txt

if OS_WIN32_MINGW

%.5.html: %.5
	$(DOS2UNIX) < $< | $(MAN2HTML) | $(FIXHTML) > $@

%.8.html: %.8
	$(DOS2UNIX) < $< | $(MAN2HTML) | $(FIXHTML) > $@
else

%.5.html: %.5
	$(MAN2HTML) $< | $(FIXHTML) > $@

%.8.html: %.8
	$(MAN2HTML) $< | $(FIXHTML) > $@
endif

%.5.txt: %.5
	$(MAN2TXT) $< > $@

%.8.txt: %.8
	$(MAN2TXT) $< > $@


# Check drive database syntax
check:
	@if ./smartctl -B $(srcdir)/drivedb.h -P showall >/dev/null; then \
	  echo "$(srcdir)/drivedb.h: OK"; \
	else \
	  echo "$(srcdir)/drivedb.h: Syntax check failed"; exit 1; \
	fi


if OS_WIN32_MINGW
# Definitions for Windows distribution

if OS_WIN64
win_bits = 64
else
win_bits = 32
endif

distdir_win32  = $(PACKAGE)-$(VERSION).win$(win_bits)
distzip_win32  = $(PACKAGE)-$(VERSION).win$(win_bits).zip
distinst_win32 = $(PACKAGE)-$(VERSION).win$(win_bits)-setup.exe

exedir_win32 = $(distdir_win32)/bin
docdir_win32 = $(distdir_win32)/doc

FILES_WIN32 = $(exedir_win32)/smartctl.exe \
              $(exedir_win32)/smartctl-nc.exe \
              $(exedir_win32)/smartd.exe \
              $(docdir_win32)/AUTHORS.txt \
              $(docdir_win32)/CHANGELOG.txt \
              $(docdir_win32)/COPYING.txt \
              $(docdir_win32)/INSTALL.txt \
              $(docdir_win32)/NEWS.txt \
              $(docdir_win32)/README.txt \
              $(docdir_win32)/TODO.txt \
              $(docdir_win32)/WARNINGS.txt \
              $(docdir_win32)/smartd.conf \
              $(docdir_win32)/smartctl.8.html \
              $(docdir_win32)/smartctl.8.txt \
              $(docdir_win32)/smartd.8.html \
              $(docdir_win32)/smartd.8.txt \
              $(docdir_win32)/smartd.conf.5.html \
              $(docdir_win32)/smartd.conf.5.txt

CLEANFILES += $(FILES_WIN32) $(exedir_win32)/syslogevt.exe \
              smartctl-nc.exe smartctl-nc.exe.tmp \
              distdir.mkdir syslogevt.check

# Textfile converter from package cygutils or tofrodos
# Note: Only use without options to be compatible with both packages
UNIX2DOS = unix2dos
DOS2UNIX = dos2unix

# Build Windows distribution

dist-win32: $(distzip_win32)

install-win32: $(distinst_win32)
	./$(distinst_win32)

installer-win32: $(distinst_win32)

distdir-win32: distdir.mkdir $(FILES_WIN32) syslogevt.check

$(distzip_win32): distdir.mkdir $(FILES_WIN32) syslogevt.check
	@rm -fv $(distzip_win32)
	cd $(distdir_win32) && zip -9Dr ../$(distzip_win32) .

# Build NSIS installer, try to locate makensis in default location first
# Note: Only option character '-' is also compatible with Linux version of makensis
$(distinst_win32): $(srcdir)/os_win32/installer.nsi distdir.mkdir $(FILES_WIN32) syslogevt.check
	@makensis="$(MAKENSIS)"; if [ -z "$$makensis" ]; then \
	  if [ ! -z "$$PROGRAMFILES" ] && "$$PROGRAMFILES/NSIS/makensis" -VERSION >/dev/null 2>&1; then \
	    makensis="$$PROGRAMFILES/NSIS/makensis"; \
	  elif makensis -VERSION >/dev/null 2>&1; then \
	    makensis=makensis; \
	  else \
	    echo 'makensis: command not found. Please download and install NSIS' 1>&2; \
	    echo 'from http://nsis.sourceforge.net/Download' 1>&2; exit 1; \
	  fi; \
	fi; \
	date=`sed -n 's,^.*DATE[^"]*"\([^"]*\)".*$$,\1,p' svnversion.h`; \
	rev=`sed -n 's,^.*REV[^"]*"\([^"]*\)".*$$,r\1,p' svnversion.h`; \
	verstr="$(PACKAGE_VERSION) $$date $$rev "$(BUILD_INFO); \
	echo "$$makensis -V2 -NOCD -DINPDIR=$(distdir_win32) -DOUTFILE=$(distinst_win32) -DVERSTR='$$verstr' $(srcdir)/os_win32/installer.nsi"; \
	"$$makensis" -V2 -NOCD -DINPDIR="$(distdir_win32)" -DOUTFILE="$(distinst_win32)" -DVERSTR="$$verstr" "$(srcdir)/os_win32/installer.nsi"

cleandist-win32:
	rm -rf $(distdir_win32) distdir.mkdir syslogevt.check

distdir.mkdir:
	@test -d $(exedir_win32) || mkdir -pv $(exedir_win32)
	@test -d $(docdir_win32) || mkdir -pv $(docdir_win32)
	touch $@

syslogevt.check:
	@if [ -f $(srcdir)/os_win32/syslogevt.exe ]; then \
	  cp -pv $(srcdir)/os_win32/syslogevt.exe $(exedir_win32)/syslogevt.exe; \
	 else echo "Warning: $(srcdir)/os_win32/syslogevt.exe missing."; fi
	touch $@

$(exedir_win32)/%.exe: %.exe
	cp -p $< $@
	if test -n '$(STRIP)'; then $(STRIP) -s $@; else strip -s $@; fi
	touch -r $< $@

$(docdir_win32)/%.html: %.html
	$(UNIX2DOS) < $< > $@
	touch -r $< $@

$(docdir_win32)/%.txt: %.txt
	$(UNIX2DOS) < $< > $@
	touch -r $< $@

$(docdir_win32)/%.txt: $(srcdir)/%
	$(UNIX2DOS) < $< > $@
	touch -r $< $@

$(docdir_win32)/%.conf: $(srcdir)/%.conf
	$(UNIX2DOS) < $< > $@
	touch -r $< $@

# Build non-console version of smartctl for GSmartControl.
# The script below changes the word at offset 220 (Subsystem) from 3
# (Console) to 2 (GUI) in a copy of smartctl.exe.
# This will be changed when a tool (like 'editbin') is available in
# the Cygwin distribution
smartctl-nc.exe: smartctl.exe
	@rm -f $@
	cp -p smartctl.exe $@.tmp
	@if test `od -A n -j 220 -N 2 -d $@.tmp` -eq 3; then :; \
	  else echo "invalid EXE header"; exit 1; fi
	@echo "editbin /subsystem:windows $@.tmp"
	@echo -ne '\002' | dd bs=1 seek=220 count=1 conv=notrunc of=$@.tmp 2>/dev/null
	@if test `od -A n -j 220 -N 2 -d $@.tmp` -eq 2; then :; \
	  else echo "EXE patch failed"; exit 1; fi
	mv -f $@.tmp $@


# Build {config,svnversion}_vc8.h for MSVC8 from MinGW {config,svnversion}.h

config-vc8: $(srcdir)/os_win32/config_vc8.h  $(srcdir)/os_win32/svnversion_vc8.h

$(srcdir)/os_win32/config_vc8.h: config.h
	sed '1i/* config_vc8.h.  Generated from config.h by Makefile.  */' $< | \
	sed 's,^#define HAVE_\(ATTR_PACKED\|INTTYPES_H\|STDINT_H\|STRINGS_H\|STRTOULL\|U*INT64_T\|UNISTD_H\|WORKING_SNPRINTF\) 1$$,/* #undef HAVE_\1 */,' | \
	sed 's,i.86-pc-mingw32,i686-pc-win32vc8,' > $@

$(srcdir)/os_win32/svnversion_vc8.h: svnversion.h
	cp svnversion.h $@

endif

SUBDIRS= . examplescripts
