# Process this file with autoconf to produce a configure script.
AC_INIT(libmpeg3, 1.8)
AM_INIT_AUTOMAKE([dist-bzip2])
AC_PREREQ(2.53)
AM_CONFIG_HEADER([config.h])
AM_PROG_AS
AC_ENABLE_SHARED
AC_ENABLE_STATIC
AC_PROG_LIBTOOL

LIBMPEG3_VERSION_CURRENT=1
LIBMPEG3_VERSION_REVISION=0
LIBMPEG3_VERSION_AGE=0
LIBMPEG3_VERSION_INFO=$LIBMPEG3_VERSION_CURRENT:$LIBMPEG3_VERSION_REVISION:$LIBMPEG3_VERSION_AGE
AC_SUBST(LIBMPEG3_VERSION_INFO)

# Checks for programs.
AC_PROG_CC
AM_PROG_CC_C_O
AC_PROG_CPP
AC_PROG_CXX

############## MMX

AC_ARG_ENABLE(mmx, 
	AC_HELP_STRING([--enable-mmx], [enables support for mmx (default is autodetect)]),
		[ enable_mmx=$enableval ],[ enable_mmx=auto ])
case "$target_cpu" in
i586 | i686)
	test "x$enable_mmx" = "xauto" && enable_mmx=yes

	CPU_CFLAGS="-DX86_CPU $CPU_CFLAGS"

	if test "x$enable_mmx" = "xyes"; then
		CPU_CFLAGS="-DHAVE_MMX -DUSE_MMX $CPU_CFLAGS" # -D_MMX_ doesn't work
		enable_mmx32=yes
	fi
;;
x86_64)
	test "x$enable_mmx" = "xauto" && enable_mmx=yes
	CPU_CFLAGS="-DX86_CPU $CPU_CFLAGS"
	if test "x$enable_mmx" = "xyes"; then
		CPU_CFLAGS="-DHAVE_MMX -DUSE_MMX $CPU_CFLAGS"

       fi
;;
esac

test "x$enable_mmx" = "xauto" && enable_mmx=no

AM_CONDITIONAL(USEMMX, test "x$enable_mmx" = "xyes")
AC_SUBST(LIBDECORE_LIBADD)
AC_SUBST(CPU_CFLAGS)

# Check for assembler
#
CCASFLAGS=""
have_assembler=false
if test x$enable_mmx = xtrue; then
  AC_PATH_PROG([NASM], [nasm], [no])
  if test "x$NASM" != "xno"; then
    if test x$cpu_x86_64 = xtrue; then
      CCASFLAGS="$NASM -O2 -felf64"
    else
      CCASFLAGS="$NASM -O2 -felf"
    fi
    have_assembler=true
  fi

  AC_PATH_PROG([YASM], [yasm], [no])
  if test "x$YASM" != "xno"; then
    if test x$cpu_x86_64 = xtrue; then
      CCASFLAGS="$YASM -Xgnu -felf64"
    else
      CCASFLAGS="$YASM -Xgnu -felf"
    fi
    have_assembler=true
  fi
fi
AC_SUBST(CCAS)
AC_SUBST(CCASFLAGS)

############## END OF MMX

LARGEFILE_CFLAGS="-D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64"
AC_SUBST(LARGEFILE_CFLAGS)

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([stdlib.h string.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_C_INLINE
AC_STRUCT_TM

## Checks for libraries.

dnl A52 Support
AC_CHECK_LIB(a52,a52_init,[liba52=yes])
AC_CHECK_HEADER(a52dec/a52.h,[liba52h=yes])

if test "x$liba52$liba52h" = "xyesyes"; then
    A52DEC_CFLAGS=
    A52DEC_LIBS=-la52
fi
AC_SUBST(A52DEC_CFLAGS)
AC_SUBST(A52DEC_LIBS)

dnl Quicktime Support
PKG_CHECK_MODULES([QUICKTIME], [libquicktime],[quicktime=true],
  [AC_MSG_WARN([libquicktime is not installed - mpeg2qt will not be built])
  ])
AM_CONDITIONAL(HAVE_QUICKTIME, [test x$quicktime = xtrue])

AC_CONFIG_FILES([
Makefile
audio/Makefile
video/Makefile
libmpeg3.spec
libmpeg3.pc
])
AC_OUTPUT

