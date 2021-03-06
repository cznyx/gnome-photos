AC_INIT([Photos],
        [3.27.90],
        [https://bugzilla.gnome.org/enter_bug.cgi?product=gnome-photos],
        [gnome-photos],
        [https://wiki.gnome.org/Apps/Photos])
AC_PREREQ(2.63)

AC_CONFIG_AUX_DIR([config])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([src])

m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

AM_INIT_AUTOMAKE([dist-xz no-dist-gzip serial-tests tar-ustar])
AM_MAINTAINER_MODE([enable])

AX_IS_RELEASE([git-directory])

AX_CHECK_ENABLE_DEBUG([yes],,, [$ax_is_release])
AX_COMPILER_FLAGS([WARN_CFLAGS], [WARN_LDFLAGS], [$ax_is_release])

AS_IF([test "x$ax_is_release" != "xyes"], [PACKAGE_COMMIT_ID=$(git describe)])
AS_IF([test -n "$PACKAGE_COMMIT_ID"],
      [AC_DEFINE_UNQUOTED([PACKAGE_COMMIT_ID],
                          ["$PACKAGE_COMMIT_ID"],
                          [Define to the description of this package's latest commit.])])

AC_CANONICAL_HOST
AC_PROG_CC
AC_PROG_CC_C_O
AC_HEADER_STDC

AC_CHECK_LIBM
AC_SUBST(LIBM)

GLIB_GSETTINGS

CAIRO_MIN_VERSION=1.14.0
DAZZLE_MIN_VERSION=3.26.0
GDATA_MIN_VERSION=0.15.2
GDK_PIXBUF_MIN_VERSION=2.32
GEGL_MIN_VERSION=0.3.28
GFBGRAPH_MIN_VERSION=0.2.1
GLIB_MIN_VERSION=2.44.0
GOA_MIN_VERSION=3.8.0
GRILO_MIN_VERSION=0.3.0
GTK_MIN_VERSION=3.22.16

G_LOG_DOMAIN=AC_PACKAGE_TARNAME
AC_DEFINE_UNQUOTED([G_LOG_DOMAIN], ["$G_LOG_DOMAIN"], [Log domain])

GETTEXT_PACKAGE=AC_PACKAGE_TARNAME
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE], ["$GETTEXT_PACKAGE"],
                   [GETTEXT package name])

AX_REQUIRE_DEFINED([AM_GNU_GETTEXT_VERSION])
AM_GNU_GETTEXT_VERSION([0.19.8])

AX_REQUIRE_DEFINED([AM_GNU_GETTEXT])
AM_GNU_GETTEXT([external])

LT_INIT

AC_CHECK_LIB([jpeg], [jpeg_destroy_decompress], , AC_MSG_ERROR([JPEG library not found]))
AC_MSG_CHECKING([for jpeglib.h])
AC_PREPROC_IFELSE([AC_LANG_SOURCE([[
#include <stdio.h>
#undef HAVE_STDDEF_H
#undef HAVE_STDLIB_H
#undef PACKAGE
#undef VERSION
#include <jpeglib.h>]])],
                  AC_MSG_RESULT([yes]),
                  AC_MSG_ERROR([JPEG header file not found]))
AC_CHECK_LIB([jpeg], [jpeg_save_markers], JPEG_LIBS='-ljpeg', AC_MSG_ERROR([JPEG library is too old]))
AC_SUBST(JPEG_LIBS)

# ****************************************************************
# Support for nl_langinfo (_NL_MEASUREMENT_MEASUREMENT) (optional)
# ****************************************************************
AC_MSG_CHECKING([for _NL_MEASUREMENT_MEASUREMENT])
AC_TRY_LINK([#include <langinfo.h>], [
char c;
c = *((unsigned char *)  nl_langinfo(_NL_MEASUREMENT_MEASUREMENT));
], have_langinfo_measurement=yes, have_langinfo_measurement=no)
AC_MSG_RESULT($have_langinfo_measurement)
if test "$have_langinfo_measurement" = "yes"; then
  AC_DEFINE([HAVE__NL_MEASUREMENT_MEASUREMENT], [1],
            [Define if _NL_MEASUREMENT_MEASUREMENT is available])
fi

GB_ENABLE_RDTSCP
AS_IF([test "x$enable_rdtscp" = "xyes"], [CFLAGS="$CFLAGS -DEGG_HAVE_RDTSCP"])
AC_CHECK_FUNCS([sched_getcpu])

AC_CHECK_FUNCS([shm_open], [LIBRT=], [AC_CHECK_LIB([rt], [shm_open], [LIBRT=-lrt], [LIBRT=])])
AC_SUBST(LIBRT)

PKG_CHECK_MODULES(BABL, [babl])
PKG_CHECK_MODULES(CAIRO, [cairo >= $CAIRO_MIN_VERSION cairo-gobject])
PKG_CHECK_MODULES(DAZZLE, [libdazzle-1.0 >= $DAZZLE_MIN_VERSION])
PKG_CHECK_MODULES(GFBGRAPH, [libgfbgraph-0.2 >= $GFBGRAPH_MIN_VERSION])
PKG_CHECK_MODULES(GDATA, [libgdata >= $GDATA_MIN_VERSION])
PKG_CHECK_MODULES(GEGL, [gegl-0.3 >= $GEGL_MIN_VERSION])
PKG_CHECK_MODULES(GEOCODE, [geocode-glib-1.0])
PKG_CHECK_MODULES(GEXIV2, [gexiv2])
PKG_CHECK_MODULES(GDK_PIXBUF, [gdk-pixbuf-2.0 >= $GDK_PIXBUF_MIN_VERSION])
PKG_CHECK_MODULES(GLIB, [glib-2.0 >= $GLIB_MIN_VERSION])
PKG_CHECK_MODULES(GIO, [gio-2.0 gio-unix-2.0])

PKG_CHECK_MODULES(GOA, [goa-1.0 >= $GOA_MIN_VERSION])
AC_DEFINE([GOA_API_IS_SUBJECT_TO_CHANGE], [], [We are aware that GOA's API can change])

PKG_CHECK_MODULES(GRILO, [grilo-0.3 >= $GRILO_MIN_VERSION])
PKG_CHECK_MODULES(GSETTINGS_DESKTOP_SCHEMAS, [gsettings-desktop-schemas])
PKG_CHECK_MODULES(GTK, [gtk+-3.0 >= $GTK_MIN_VERSION])
PKG_CHECK_MODULES(GTK_UNIX_PRINT, [gtk+-unix-print-3.0])
PKG_CHECK_MODULES(PNG, [libpng16])
PKG_CHECK_MODULES(TRACKER, [tracker-control-2.0 tracker-sparql-2.0])

LIBGD_INIT([
  gtk-hacks
  main-box
  main-icon-view
  static
  tagged-entry
])

AC_ARG_ENABLE(dogtail,
              [AS_HELP_STRING([--enable-dogtail],
                              [test using dogtail [default=yes]])],,
              enable_dogtail=yes)
AM_CONDITIONAL(ENABLE_DOGTAIL, test "$enable_dogtail" != no)

GLIB_TESTS

GLIB_COMPILE_RESOURCES=`$PKG_CONFIG --variable glib_compile_resources gio-2.0`
AC_SUBST(GLIB_COMPILE_RESOURCES)

AC_ARG_VAR([DESKTOP_FILE_VALIDATE], [the desktop-file-validate program])
AC_PATH_PROG([DESKTOP_FILE_VALIDATE], [desktop-file-validate], [])
if test -z "$DESKTOP_FILE_VALIDATE"; then
  AC_MSG_ERROR([desktop-file-validate not found])
fi

#############################################
#Documentation
#############################################

AX_REQUIRE_DEFINED([YELP_HELP_INIT])
YELP_HELP_INIT

AC_CONFIG_FILES([
Makefile
data/Makefile
data/org.gnome.Photos.desktop.in
data/icons/Makefile
help/Makefile
libgd/Makefile
src/Makefile
tests/Makefile
po/Makefile.in
])
AC_OUTPUT
