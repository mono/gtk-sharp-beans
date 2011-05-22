dnl Process this file with autoconf to produce a configure script.
AC_INIT(gtk-sharp-beans, 2.14.1, stephane@delcroix.org)
AC_CONFIG_AUX_DIR(config)
AC_CONFIG_SRCDIR(AssemblyInfo.cs.in)

AM_INIT_AUTOMAKE([dist-bzip2 tar-ustar])

PACKAGE_VERSION=gtk-sharp-beans-2.0
AC_SUBST(PACKAGE_VERSION)

AC_PATH_PROG(CSC, gmcs)
AC_PATH_PROG(GACUTIL, gacutil, no)
if test "x$GACUTIL" = "xno" ; then
	AC_MSG_ERROR([No gacutil tool found, You need to install either the mono or .Net SDL.])
fi

GACUTIL_FLAGS='/package $(PACKAGE_VERSION) /gacdir $(DESTDIR)$(prefix)/lib'

AC_SUBST(GACUTIL)
AC_SUBST(GACUTIL_FLAGS)

PKG_CHECK_MODULES(GTKSHARP, gtk-sharp-2.0 >= 2.12)
AC_SUBST(GTKSHARP_LIBS)
PKG_CHECK_MODULES(GLIBSHARP, glib-sharp-2.0 >= 2.12)
AC_SUBST(GLIBSHARP_LIBS)
PKG_CHECK_MODULES(GIOSHARP, gio-sharp-2.0 >= 2.18)
AC_SUBST(GIOSHARP_LIBS)
PKG_CHECK_MODULES(GAPI, gapi-2.0 >= 2.12.0)

CSC_DEFINES=""
if pkg-config --atleast-version=2.16 gtk+-2.0; then
	CSC_DEFINES="$CSC_DEFINES -d:GTK_2_16"
fi

AC_SUBST(CSC_DEFINES)

if test `uname -s` = "Darwin"; then
	LIB_PREFIX=
	LIB_SUFFIX=.dylib
else
	LIB_PREFIX=.so
	LIB_SUFFIX=
fi
AC_SUBST(LIB_PREFIX)
AC_SUBST(LIB_SUFFIX)

AC_OUTPUT(
Makefile 
gtk-sharp-beans-2.0.pc
gtk-sharp-beans.dll.config
AssemblyInfo.cs
)
