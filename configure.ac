m4_define([libunivalue_major_version], [1])
m4_define([libunivalue_minor_version], [1])
m4_define([libunivalue_micro_version], [1])
m4_define([libunivalue_interface_age], [1])
# If you need a modifier for the version number. 
# Normally empty, but can be used to make "fixup" releases.
m4_define([libunivalue_extraversion], [])

dnl libtool versioning from libunivalue
m4_define([libunivalue_current], [m4_eval(100 * libunivalue_minor_version + libunivalue_micro_version - libunivalue_interface_age)])
m4_define([libunivalue_binary_age], [m4_eval(100 * libunivalue_minor_version + libunivalue_micro_version)])
m4_define([libunivalue_revision], [libunivalue_interface_age])
m4_define([libunivalue_age], [m4_eval(libunivalue_binary_age - libunivalue_interface_age)])
m4_define([libunivalue_version], [libunivalue_major_version().libunivalue_minor_version().libunivalue_micro_version()libunivalue_extraversion()])


AC_INIT([univalue], [1.0.0],
        [http://github.com/jgarzik/univalue/])

AC_PREREQ(2.60)
AC_CONFIG_SRCDIR([lib/univalue.cpp])
AM_INIT_AUTOMAKE([gnu])
AC_CONFIG_HEADERS([univalue-config.h])

LIBUNIVALUE_MAJOR_VERSION=libunivalue_major_version
LIBUNIVALUE_MINOR_VERSION=libunivalue_minor_version
LIBUNIVALUE_MICRO_VERSION=libunivalue_micro_version
LIBUNIVALUE_INTERFACE_AGE=libunivalue_interface_age

# ABI version
# http://www.gnu.org/software/libtool/manual/html_node/Updating-version-info.html
LIBUNIVALUE_CURRENT=libunivalue_current
LIBUNIVALUE_REVISION=libunivalue_revision
LIBUNIVALUE_AGE=libunivalue_age

AC_SUBST(LIBUNIVALUE_CURRENT)
AC_SUBST(LIBUNIVALUE_REVISION)
AC_SUBST(LIBUNIVALUE_AGE)

LT_INIT

AC_PROG_CXX
AC_PROG_CC
AC_PROG_CPP
AC_PROG_CXXCPP
AC_PROG_INSTALL
AC_PROG_OBJC
AC_PROG_LN_S
AC_PROG_MKDIR_P
AC_PROG_SED
AC_PATH_TOOL(AR, ar)
AC_PATH_TOOL(RANLIB, ranlib)
AC_PATH_TOOL(STRIP, strip)
PKG_PROG_PKG_CONFIG

AC_LANG_PUSH([C++])

AC_DEFUN([AX_CHECK_LINK_FLAG],
[AS_VAR_PUSHDEF([CACHEVAR],[ax_cv_check_ldflags_$4_$1])dnl
AC_CACHE_CHECK([whether the linker accepts $1], CACHEVAR, [
  ax_check_save_flags=$LDFLAGS
  LDFLAGS="$LDFLAGS $4 $1"
  AC_LINK_IFELSE([AC_LANG_PROGRAM()],
    [AS_VAR_SET(CACHEVAR,[yes])],
    [AS_VAR_SET(CACHEVAR,[no])])
  LDFLAGS=$ax_check_save_flags])
AS_IF([test x"AS_VAR_GET(CACHEVAR)" = xyes],
  [m4_default([$2], :)],
  [m4_default([$3], :)])
AS_VAR_POPDEF([CACHEVAR])dnl
])dnl AX_CHECK_LINK_FLAGS

case $host in
  *mingw*)
     TARGET_OS=windows
     AC_CHECK_LIB([mingwthrd],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([kernel32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([user32],      [main],, AC_MSG_ERROR(lib missing))
     AC_CHECK_LIB([gdi32],      [main],, AC_MSG_ERROR(lib missing))

     # -static is interpreted by libtool, where it has a different meaning.
     # In libtool-speak, it's -all-static.
     AX_CHECK_LINK_FLAG([[-static]],[LIBTOOL_APP_LDFLAGS="$LIBTOOL_APP_LDFLAGS -all-static"])

     dnl libtool insists upon adding -nostdlib and a list of objects/libs to link against.
     dnl That breaks our ability to build dll's with static libgcc/libstdc++/libssp. Override
     dnl its command here, with the predeps/postdeps removed, and -static inserted. Postdeps are
     dnl also overridden to prevent their insertion later.
     dnl This should only affect dll's.
     archive_cmds_CXX="\$CC -shared \$libobjs \$deplibs \$compiler_flags -static -o \$output_objdir/\$soname \${wl}--enable-auto-image-base -Xlinker --out-implib -Xlinker \$lib"
     postdeps_CXX=

     ;;
esac

AC_CONFIG_FILES([
    lib/Makefile
    test/Makefile
    Makefile
    lib/libunivalue.pc
    lib/libunivalue-uninstalled.pc])
AC_OUTPUT

