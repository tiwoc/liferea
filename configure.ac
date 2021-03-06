dnl Process this file with autoconf to produce a configure script.

AC_INIT([liferea],[1.11.0],[liferea-devel@lists.sourceforge.net])
AC_CANONICAL_HOST
AC_CONFIG_SRCDIR([src/feedlist.c])

AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE([1.11 foreign std-options -Wall -Werror])
AM_SILENT_RULES([yes])

dnl Needed for automake 1.12
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])

AC_PREREQ(2.59)

LT_INIT
IT_PROG_INTLTOOL([0.40.4])

AC_PROG_CC
AM_PROG_CC_C_O
AC_PROG_MAKE_SET
AC_SYS_LARGEFILE
GLIB_GSETTINGS

AC_ARG_ENABLE(libnotify, AS_HELP_STRING([--disable-libnotify],[compile without libnotify support]),,enable_libnotify=yes)
AC_ARG_ENABLE(libindicate, AS_HELP_STRING([--disable-libindicate],[compile without libindicate support]),,enable_libindicate=yes)

AC_CHECK_FUNCS([strsep])

PKG_PROG_PKG_CONFIG()

dnl *********
dnl libnotify
dnl *********

if test "x$enable_libnotify" = "xyes"; then
  PKG_CHECK_MODULES([LIBNOTIFY], libnotify >= 0.7,enable_libnotify=yes,enable_libnotify=no)
  if test "x$enable_libnotify" = "xyes"; then
    AC_DEFINE(HAVE_LIBNOTIFY, 1, [Define if libnotify support is enabled])
  fi
  AC_SUBST(LIBNOTIFY_CFLAGS)
  AC_SUBST(LIBNOTIFY_LIBS)
else
  enable_libnotify=no
fi

AM_CONDITIONAL(WITH_LIBNOTIFY, test "x$enable_libnotify" = "xyes")

dnl *********
dnl libindicate
dnl *********

if test "x$enable_libindicate" = "xyes"; then
   dnl Try 0.7 first
   PKG_CHECK_MODULES([LIBINDICATE], indicate-0.7 indicate-gtk3-0.7,enable_libindicate=yes,enable_libindicate=no)
   AC_SUBST(LIBINDICATE_CFLAGS)
   AC_SUBST(LIBINDICATE_LIBS)

   dnl But 0.6 will also do...
   if test "x$enable_libindicate" = "xno"; then
      PKG_CHECK_MODULES([LIBINDICATE], indicate-0.6 indicate-gtk3-0.6,enable_libindicate=yes,enable_libindicate=no)
      AC_SUBST(LIBINDICATE_CFLAGS)
      AC_SUBST(LIBINDICATE_LIBS)
   fi
else
   enable_libindicate=no
fi

if test "x$enable_libindicate" = "xyes"; then
  AC_DEFINE(HAVE_LIBINDICATE, 1, [Define if libindicate support is enabled])
fi

AM_CONDITIONAL(WITH_LIBINDICATE, test "x$enable_libindicate" = "xyes")

################################################################################
# Mandatory library dependencies

pkg_modules="	gtk+-3.0 >= 3.4.0
		glib-2.0 >= 2.28.0
		gio-2.0 >= 2.26.0
		pango >= 1.4.0 
		libxml-2.0 >= 2.6.27
		libxslt >= 1.1.19
		sqlite3 >= 3.7.0
		gmodule-2.0 >= 2.0.0
		gthread-2.0
		libsoup-2.4 >= 2.28.2
		webkitgtk-3.0
		json-glib-1.0
		gobject-introspection-1.0
		gsettings-desktop-schemas
		libpeas-1.0 >= 1.0.0
		libpeas-gtk-1.0 >= 1.0.0"

################################################################################

PKG_CHECK_MODULES(PACKAGE, [$pkg_modules])

AC_SUBST(PACKAGE_CFLAGS)
AC_SUBST(PACKAGE_LIBS)

uname=`uname`
AC_DEFINE_UNQUOTED(OS, $uname, [defines a OS version string, used for OS specific code])
AC_DEFINE_UNQUOTED(OSNAME, "$uname", [defines a OS version string, used for the user agent string])
AC_MSG_RESULT(user agent OS = $uname)

################################################################################
# Plugins via Introspection

GOBJECT_INTROSPECTION_CHECK([0.9.3])

################################################################################

AM_GLIB_GNU_GETTEXT

GETTEXT_PACKAGE=liferea
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE], ["${GETTEXT_PACKAGE}"], [gettext domain])


AC_CONFIG_FILES([
Makefile
net.sf.liferea.gschema.xml
src/Makefile
src/webkit/Makefile
src/parsers/Makefile
src/fl_sources/Makefile
src/notification/Makefile
src/ui/Makefile
doc/Makefile
doc/html/Makefile
xslt/Makefile
man/Makefile
man/pl/Makefile
pixmaps/Makefile
pixmaps/16x16/Makefile
pixmaps/22x22/Makefile
pixmaps/24x24/Makefile
pixmaps/32x32/Makefile
pixmaps/48x48/Makefile
pixmaps/scalable/Makefile
opml/Makefile
glade/Makefile
po/Makefile.in
src/liferea-add-feed
])
AC_OUTPUT

echo
echo "$PACKAGE $VERSION"
echo
echo "Use libnotify................... : $enable_libnotify"
echo "Use libindicate................. : $enable_libindicate"
echo
eval eval echo Liferea will be installed in $bindir.
echo
echo configure complete, now type \'make\'
echo
