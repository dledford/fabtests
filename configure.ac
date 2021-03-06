dnl Process this file with autoconf to produce a configure script.

AC_PREREQ(2.57)
AC_INIT([fabtests], [0.0.1], [linux-rdma@vger.kernel.org])
AC_CONFIG_AUX_DIR(config)
AC_CONFIG_MACRO_DIR(config)
AC_CONFIG_HEADERS(config.h)
AM_INIT_AUTOMAKE([1.11 dist-bzip2 foreign -Wall -Werror subdir-objects])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

dnl Fix autoconf's habit of adding -g -O2 by default
AS_IF([test -z "$CFLAGS"],
      [CFLAGS='-O2 -DNDEBUG -Wall'])

# AM PROG_AR did not exist pre AM 1.11.x (where x is somewhere >0 and
# <3), but it is necessary in AM 1.12.x.
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])

AM_PROG_LIBTOOL

AC_ARG_WITH([valgrind],
    AC_HELP_STRING([--with-valgrind],
		   [Enable valgrind annotations - default NO]))

if test "$with_valgrind" != "" && test "$with_valgrind" != "no"; then
	AC_DEFINE([INCLUDE_VALGRIND], 1,
		  [Define to 1 to enable valgrind annotations])
	if test -d $with_valgrind; then
		CPPFLAGS="$CPPLFAGS -I$with_valgrind/include"
	fi
fi

AC_ARG_ENABLE(libcheck, [  --disable-libcheck      do not test for presence of libraries],
[       if test "$enableval" = "no"; then
                disable_libcheck=yes
        fi
])

dnl Checks for programs
AC_PROG_CC
AM_PROG_CC_C_O

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_CHECK_SIZEOF(long)

dnl Checks for libraries
AC_CHECK_LIB([fabric], fi_getinfo, [],
    AC_MSG_ERROR([fi_getinfo() not found.  fabtests requires libfabric.]))

dnl Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADER([rdma/fabric.h], [],
    [AC_MSG_ERROR([<rdma/fabric.h> not found.  fabtests requires libfabric.])])

if test "$disable_libcheck" != "yes"; then
if test "$with_valgrind" != "" && test "$with_valgrind" != "no"; then
AC_CHECK_HEADER(valgrind/memcheck.h, [],
    AC_MSG_ERROR([valgrind requested but <valgrind/memcheck.h> not found.]))
fi
fi

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
