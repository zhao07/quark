# configure script for pywebkitgtk

m4_define([pywebkitgtk_major_version], [1])
m4_define([pywebkitgtk_minor_version], [1])
m4_define([pywebkitgtk_micro_version], [8])

m4_define([pywebkitgtk_version],
          [pywebkitgtk_major_version.pywebkitgtk_minor_version.pywebkitgtk_micro_version])

AC_INIT([pywebkitgtk],
        [pywebkitgtk_version],
        [http://code.google.com/p/pywebkitgtk/issues])

AC_PREREQ([2.59])
AC_CONFIG_MACRO_DIR([m4])
AC_SUBST(ACLOCAL_AMFLAGS, "-I m4 -I .")

AC_CONFIG_SRCDIR([configure.ac])
AC_CONFIG_HEADERS([config.h])

AC_DEFINE_UNQUOTED(PYWEBKITGTK_VERSION, pywebkitgtk_version,
                   [The release version of Python WebKit Gtk+])
AC_DEFINE_UNQUOTED(PYWEBKITGTK_MAJOR_VERSION, pywebkitgtk_major_version,
                   [The major version of Python WebKit Gtk+.])
AC_DEFINE_UNQUOTED(PYWEBKITGTK_MINOR_VERSION, pywebkitgtk_minor_version,
                   [The minor version of Python WebKit Gtk+.])
AC_DEFINE_UNQUOTED(PYWEBKITGTK_MICRO_VERSION, pywebkitgtk_micro_version,
                   [The micro version of Python WebKit Gtk+.])

AM_INIT_AUTOMAKE([foreign subdir-objects])

AM_PROG_CC_C_O
AC_PROG_LIBTOOL
dnl when using libtool 2.x create libtool early, because it's used in configure
m4_ifdef([LT_OUTPUT], [LT_OUTPUT])

dnl check for python
AM_PATH_PYTHON(2.4)
AM_CHECK_PYTHON_HEADERS(,[AC_MSG_ERROR(could not find Python headers)])

dnl from pygtk: get rid of -export-dynamic stuff from the configure flags...
export_dynamic=`(./libtool --config; echo eval echo \\$export_dynamic_flag_spec) | sh`

dnl check for dependencies
dnl Check for libxslt until upstream WebKit checks for it as this is a webkit
dnl dep and not pywebkitgtk's.
PKG_CHECK_MODULES(DEPS, 
                  [libxslt
                  gthread-2.0
                  pygtk-2.0])
AC_SUBST(DEPS_CFLAGS)
AC_SUBST(DEPS_LIBS)
AC_SUBST(DEPS_CPPFLAGS)

dnl python code gen
AC_PATH_PROGS(PYGOBJECT_CODEGEN, pygobject-codegen-2.0 pygtk-codegen-2.0, no)
if test "x$PYGOBJECT_CODEGEN" = xno; then
   AC_MSG_ERROR(could not find pygobject-codegen-2.0 script)
fi

dnl check for pygtk defsdir
AC_MSG_CHECKING(for pygtk defs)
PYGTK_DEFSDIR=`$PKG_CONFIG --variable=defsdir pygtk-2.0`
AC_SUBST(PYGTK_DEFSDIR)
AC_MSG_RESULT($PYGTK_DEFSDIR)

dnl check for pygtk codegen dir
AC_MSG_CHECKING(for pygtk codegendir)
PYGTK_CODEGENDIR=`$PKG_CONFIG --variable=codegendir pygobject-2.0`
AC_SUBST(PYGTK_CODEGENDIR)
AC_MSG_RESULT($PYGTK_CODEGENDIR)

# the minimum version of WebKitGtk required for this version of PyWebKitGtk to work
WEBKITGTK_REQUIRED_VERSION=1.1.10

dnl check for webkit versions installed
PKG_CHECK_MODULES(WEBKIT, [webkit-1.0 >= $WEBKITGTK_REQUIRED_VERSION])
AC_SUBST(WEBKIT_CFLAGS)
AC_SUBST(WEBKIT_LIBS)
AC_SUBST(WEBKIT_CPPFLAGS)

dnl output
AC_CONFIG_FILES([
Makefile
pywebkitgtk-1.0.pc:pywebkitgtk.pc.in
])

AC_OUTPUT
