# Most of this is stolen from caja-extensions / caja-dropbox
# Initialization

AC_INIT([caja-xattrs], 1.19.0, [https://github.com/mate-desktop/caja-xattrs])

AM_INIT_AUTOMAKE([1.9 tar-ustar dist-xz foreign no-dist-gzip check-news])

AC_CONFIG_MACRO_DIR([m4])
AC_PREFIX_DEFAULT(/usr)

AC_CONFIG_HEADERS(config.h)

# not ready for i18n yet
#AC_PROG_INTLTOOL([0.29])

# Dependency checks
CAJA_REQUIRED=1.17.1
LIBMATE_DESKTOP_REQUIRED=1.17.0
GLIB_REQUIRED=2.36.0
GTK_REQUIRED=3.14.0

GLIB_GSETTINGS

# Used programs
AM_DISABLE_STATIC
AC_PROG_CC
AM_PROG_LIBTOOL

# Check for pkg-config
AC_CHECK_PROG(HAVE_PKGCONFIG, pkg-config, yes, no)
AC_PATH_PROG(PKG_CONFIG, pkg-config, no)
if test "x$HAVE_PKGCONFIG" = "xno"; then
	AC_MSG_ERROR(you need to have pkgconfig installed !)
fi

PKG_CHECK_MODULES(CAJA, libcaja-extension >= $CAJA_REQUIRED)
PKG_CHECK_MODULES(GLIB, glib-2.0 >= $GLIB_REQUIRED)

# Make dependency CFLAGS and LIBS available
AC_SUBST(CAJA_CFLAGS)
AC_SUBST(CAJA_LIBS)
AC_SUBST(GLIB_CFLAGS)
AC_SUBST(GLIB_LIBS)

# lol stolen from the automake manual
AC_ARG_ENABLE([debug],
[  --enable-debug    Turn on debugging],
[case "${enableval}" in
yes) debug=true ;;
no)  debug=false ;;
*) AC_MSG_ERROR([bad value ${enableval} for --enable-debug]) ;;
esac],[debug=false])
AM_CONDITIONAL([DEBUG], [test x$debug = xtrue])

AC_ARG_WITH(caja-extension-dir,
				[AS_HELP_STRING([--with-caja-extension-dir],
					[specify the caja extension directory])])

# Get caja extensions directory
AC_MSG_CHECKING([for caja extension directory])
if test -n "$with_caja_extension_dir"; then
	CAJA_EXTENSION_DIR=$with_caja_extension_dir
else
	CAJA_EXTENSION_DIR=`$PKG_CONFIG --variable=extensiondir libcaja-extension`
fi
if test -z "$CAJA_EXTENSION_DIR"; then
	CAJA_EXTENSION_DIR='${exec_prefix}/lib/caja/extensions-2.0'
fi

AC_MSG_RESULT([${CAJA_EXTENSION_DIR}])
AC_SUBST(CAJA_EXTENSION_DIR)

CAJA_VERSION=`$PKG_CONFIG --modversion libcaja-extension`
if test -n "$CAJA_VERSION"; then
	CAJA_VERSION_MAJOR=`echo -n $CAJA_VERSION | cut -d . -f 1`
	CAJA_VERSION_MINOR=`echo -n $CAJA_VERSION | cut -d . -f 2`
	CAJA_VERSION_MICRO=`echo -n $CAJA_VERSION | cut -d . -f 3`
fi

AC_DEFINE_UNQUOTED(CAJA_VERSION_MAJOR, [$CAJA_VERSION_MAJOR], [ caja version major])
AC_DEFINE_UNQUOTED(CAJA_VERSION_MINOR, [$CAJA_VERSION_MINOR], [ caja version minor])
AC_DEFINE_UNQUOTED(CAJA_VERSION_MICRO, [$CAJA_VERSION_MICRO], [ caja version micro])

AC_CONFIG_FILES([
	Makefile
	src/libcaja-xattr-tags-property-page.caja-extension.in
	src/Makefile
	po/Makefile.in 
])

# intltool stuff
IT_PROG_INTLTOOL(0.18)
GETTEXT_PACKAGE=caja-xattrs
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE, "$GETTEXT_PACKAGE", [gettext package])
AM_GLIB_GNU_GETTEXT

AC_OUTPUT

