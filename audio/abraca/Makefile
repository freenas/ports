# Created by: Olivier Smedts <olivier@gid0.org>
# $FreeBSD$

PORTNAME=	abraca
PORTVERSION=	0.8.2
PORTREVISION=	3
CATEGORIES=	audio
MASTER_SITES=	SF/${PORTNAME}

MAINTAINER=	madpilot@FreeBSD.org
COMMENT=	GTK2 client for the XMMS2 music player

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING.GPL

DEPRECATED=	Uses deprecated version of python
EXPIRATION_DATE=	2020-09-15

BUILD_DEPENDS=	${LOCALBASE}/bin/valac:lang/vala
LIB_DEPENDS=	libxmmsclient.so:audio/xmms2 \
		libgee-0.8.so:devel/libgee
RUN_DEPENDS=	gnome-themes-extra>=3.0.0:x11-themes/gnome-themes-extra \
		gnome-icon-theme>=0:misc/gnome-icon-theme

USES=		gnome pkgconfig python:2.7,build tar:bzip2 waf
USE_GNOME=	gtk30 intltool

MAKE_ARGS=	MANDIR=${MANPREFIX}/man
CONFIGURE_ARGS+=	--prefix=${PREFIX} \
			--mandir=${PREFIX}/man

OPTIONS_DEFINE=	NLS

OPTIONS_SUB=	yes
NLS_USES=	gettext

post-patch-NLS-off:
	@${REINPLACE_CMD} -e '/\.load(/s/ intltool//' \
		-e '/recurse/s/ po//' ${WRKSRC}/wscript

post-install:
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/bin/${PORTNAME}

.include <bsd.port.mk>
