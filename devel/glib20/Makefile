# Created by: Vanilla I. Shu <vanilla@FreeBSD.org>
# $FreeBSD$

PORTNAME=	glib
PORTVERSION=	2.50.2
PORTREVISION=	3
PORTEPOCH=	1
CATEGORIES=	devel
MASTER_SITES=	GNOME
DIST_SUBDIR=	gnome2

MAINTAINER=	gnome@FreeBSD.org
COMMENT=	Some useful routines of C programming (current stable version)

LICENSE=	LGPL20

USES=		tar:xz
PORTSCOUT=	limitw:1,even

.if !defined(REFERENCE_PORT)

LIB_DEPENDS+=	libpcre.so:devel/pcre \
		libffi.so:devel/libffi

GNU_CONFIGURE=	yes
USE_LDCONFIG=	yes
# iconv:wchar_t - our iconv in base doesn't support utf-8 -> wchar_t (boooo)
# (wchar_t is used by glibmm, rawtherapee triggered this)
USES+=		compiler:c11 gettext gmake gnome iconv:wchar_t libtool \
		localbase pathfix perl5 pkgconfig python shebangfix
USE_PYTHON=	py3kplist
CONFIGURE_ARGS=	--disable-gtk-doc --with-html-dir=${PREFIX}/share/doc \
		--disable-man --without-xml-catalog \
		--enable-static=yes \
		--with-pcre=system \
		--disable-fam
CONFIGURE_ENV=	ac_cv_header_sys_inotify_h=
INSTALL_TARGET=	install-strip

SHEBANG_FILES=	*/*.pl

LIBVERSION=	0.5000.2
PLIST_SUB+=	LIBVERSION=${LIBVERSION}

glib_MAN=	gtester.1 gtester-report.1 glib-gettextize.1
gio_MAN=	gdbus.1 gsettings.1 gio-querymodules.1 \
		glib-compile-schemas.1 gapplication.1 \
		glib-compile-resources.1 gresource.1 gdbus-codegen.1
gobject_MAN=	glib-genmarshal.1 glib-mkenums.1 gobject-query.1

OPTIONS_DEFINE=	COLLATION_FIX DEBUG
# libc collation was fixed by https://svnweb.freebsd.org/changeset/base/290494
COLLATION_FIX_DESC=	Use ICU for UTF-8 string collation (if libc is broken)

OPTIONS_EXCLUDE_FreeBSD_11=	COLLATION_FIX
OPTIONS_EXCLUDE_FreeBSD_12=	COLLATION_FIX

DEBUG_CONFIGURE_ON=	--enable-debug=yes

COLLATION_FIX_LIB_DEPENDS+=	libicui18n.so:devel/icu
COLLATION_FIX_EXTRA_PATCHES+=	${FILESDIR}/extra-patch-glib_Makefile.in \
		${FILESDIR}/extra-patch-glib_gunicollate.c
# Disable it until someone can figure why it causes net-im/pidgin crashed.
# http://www.freebsd.org/cgi/query-pr.cgi?pr=175930 see comment by ed@
#COLLATION_FIX_CONFIGURE_ENV+=	CFLAGS="-D__STDC_ISO_10646__"

.include <bsd.port.pre.mk>

# doesn't build yet
CONFIGURE_ARGS+=--disable-dtrace

.if empty(ICONV_LIB)
CONFIGURE_ARGS+=	--with-libiconv=no
.else
CONFIGURE_ARGS+=	--with-libiconv=gnu
.endif

.if ${ARCH} == powerpc64
EXTRA_PATCHES=	${FILESDIR}/extra-arch-powerpc64
.endif

# Let glib use asm code for implementing atomic ops on i386 and amd64.
.if ${ARCH} == "i386"
GLIB_ARCH=	i486
.elif ${ARCH} == "amd64"
GLIB_ARCH=	x86_64
.else
GLIB_ARCH=	${ARCH}
.endif
CONFIGURE_TARGET=${GLIB_ARCH}-portbld-freebsd${OSREL}

post-patch:
	@${REINPLACE_CMD} -e 's|/usr/local|${LOCALBASE}|g ; \
		s|/usr/share/locale/locale|${LOCALBASE}/share/locale/locale|g' \
			${WRKSRC}/glib/gutils.c
	@${REINPLACE_CMD} -e 's|inotify_support=yes|inotify_support=no| ; \
		s|-Werror|| ; \
		s|#define HAVE_SYS_INOTIFY_H 1||' ${WRKSRC}/configure

post-install:
	@${MKDIR} ${STAGEDIR}${PREFIX}/share/GConf/gsettings
	@${MKDIR} ${STAGEDIR}${PREFIX}/lib/gio/modules
.for m in glib gio gobject
	${INSTALL_MAN} ${${m}_MAN:S|^|${WRKSRC}/docs/reference/${m}/|} \
		${STAGEDIR}${PREFIX}/man/man1
.endfor

.include <bsd.port.post.mk>

.endif
