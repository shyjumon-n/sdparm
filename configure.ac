AC_INIT(sdparm, 1.08, dgilbert@interlog.com)

AM_INIT_AUTOMAKE
AM_MAINTAINER_MODE
AM_CONFIG_HEADER(config.h)

AC_PROG_CC
# AC_PROG_CXX
AC_PROG_INSTALL

# check for headers
AC_HEADER_STDC
AC_CHECK_HEADERS([linux/types.h linux/bsg.h linux/kdev_t.h], [], [],
     [[#ifdef HAVE_LINUX_TYPES_H
     # include <linux/types.h>
     #endif
     ]])

AC_CHECK_FUNCS(getopt_long,
	       GETOPT_O_FILES='',
	       GETOPT_O_FILES='getopt_long.o')
AC_SUBST(GETOPT_O_FILES)

AC_CANONICAL_HOST

AC_DEFINE_UNQUOTED(SG_LIB_BUILD_HOST, "${host}", [sdparm Build Host])

case "${host}" in
        *-*-linux-gnu*)
                AC_SUBST([os_deps], ['sg_pt_linux.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_LINUX, 1, [sdparm on linux])
                AC_SUBST([os_libs], ['']) ;;
        *-*-linux*)
                AC_SUBST([os_deps], ['sg_pt_linux.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_LINUX, 1, [sdparm on linux])
                AC_SUBST([os_libs], ['']) ;;
        *-*-freebsd*|*-*-kfreebsd*-gnu*)
                AC_SUBST([os_deps], ['sg_pt_freebsd.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_FREEBSD, 1, [sdparm on FreeBSD])
                AC_SUBST([os_libs], ['-lcam']);;
        *-*-solaris*)
                AC_SUBST([os_deps], ['sg_pt_solaris.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_SOLARIS, 1, [sdparm on Solaris])
                AC_SUBST([os_libs], ['']);;
        *-*-osf*)
                AC_SUBST([os_deps], ['sg_pt_osf1.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_OSF1, 1, [sdparm on Tru64 UNIX])
                AC_SUBST([os_libs], ['']) ;;
        *-*-cygwin*)
                AC_SUBST([os_deps], ['sg_pt_win32.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_WIN32, 1, [sdparm on Win32])
		AC_DEFINE_UNQUOTED(WIN32_SPT_DIRECT, 1, [allow large buffers, aligned?])
                AC_SUBST([os_libs], ['']) ;;
        *-*-mingw*)
                AC_SUBST([os_deps], ['sg_pt_win32.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_WIN32, 1, [sdparm on Win32])
		AC_DEFINE_UNQUOTED(SG_LIB_MINGW, 1, [also MinGW environment])
		AC_DEFINE_UNQUOTED(WIN32_SPT_DIRECT, 1, [allow large buffers, aligned?])
                AC_SUBST([os_libs], ['']) ;;
        *)
                AC_SUBST([os_deps], ['sg_pt_linux.o'])
		AC_DEFINE_UNQUOTED(SG_LIB_LINUX, 1, [assume sdparm on linux])
                AC_SUBST([os_libs], ['']) ;;
esac

# Define platform-specific symbol.
AM_CONDITIONAL(OS_FREEBSD, [echo $host_os | grep 'freebsd' > /dev/null])
AM_CONDITIONAL(OS_LINUX, [echo $host_os | grep '^linux' > /dev/null])
AM_CONDITIONAL(OS_OSF, [echo $host_os | grep '^osf' > /dev/null])
AM_CONDITIONAL(OS_SOLARIS, [echo $host_os | grep '^solaris' > /dev/null])
AM_CONDITIONAL(OS_WIN32_MINGW, [echo $host_os | grep '^mingw' > /dev/null])
AM_CONDITIONAL(OS_WIN32_CYGWIN, [echo $host_os | grep '^cygwin' > /dev/null])

AC_ARG_ENABLE([linuxbsg],
  AC_HELP_STRING([--disable-linuxbsg], [ignore linux bsg (sgv4) if present]),
  [AC_DEFINE_UNQUOTED(IGNORE_LINUX_BSG, 1, [ignore linux bsg], )], [])

AC_CHECK_LIB(sgutils2, sg_ll_inquiry,
  [SGUTILS_LIBS="-lsgutils2"; have_sgutils=yes], have_sgutils=no)
AC_SUBST(SGUTILS_LIBS)
AM_CONDITIONAL(HAVE_SGUTILS, test x"$have_sgutils" = xyes)

AC_ARG_ENABLE([scsistrings],
  [AS_HELP_STRING([--disable-scsistrings],
                  [Disable full SCSI sense strings])],
  [], [AC_DEFINE_UNQUOTED(SG_SCSI_STRINGS, 1, [full SCSI sense strings], )])

# AC_PROG_LIBTOOL
AC_OUTPUT(Makefile src/Makefile doc/Makefile scripts/Makefile)
