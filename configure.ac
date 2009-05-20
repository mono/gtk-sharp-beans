dnl Process this file with autoconf to produce a configure script.
AC_INIT(gtk-sharp-beans, 2.13.90, stephane@delcroix.org)
AC_CONFIG_AUX_DIR(config)
AC_CONFIG_SRCDIR(Builder.custom)

AM_INIT_AUTOMAKE

AC_PATH_PROG(CSC, gmcs)

PKG_CHECK_MODULES(GTKSHARP, gtk-sharp-2.0 >= 2.12)
PKG_CHECK_MODULES(GLIBSHARP, glib-sharp-2.0 >= 2.12)
PKG_CHECK_MODULES(GAPI, gapi-2.0 >= 2.12.0)

AC_OUTPUT(
Makefile 
sources/Makefile
)
