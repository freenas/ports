# $FreeBSD$

PORTNAME=	sabredav
PORTVERSION=	3.2.2
PORTREVISION=	1
CATEGORIES=	www

MAINTAINER=	meyser@xenet.de
COMMENT=	WebDav, CalDav and Carddav Server/Framework written in PHP

LICENSE=	BSD3CLAUSE

USES=			cpe php
USE_PHP=		dom simplexml mbstring pcre spl ctype iconv xmlwriter
USE_GITHUB=		yes

OPTIONS_MULTI=		DATABASE
OPTIONS_MULTI_DATABASE=	MYSQL PGSQL SQLITE
OPTIONS_DEFAULT=	MYSQL

CPE_VENDOR=		fruux
GH_ACCOUNT=		fruux
GH_PROJECT=		sabre-dav

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MMYSQL}
USE_PHP+=		pdo_mysql
.endif

.if ${PORT_OPTIONS:MPGSQL}
USE_PHP+=		pdo_pgsql
.endif

.if ${PORT_OPTIONS:MSQLITE}
USE_PHP+=		pdo_sqlite
.endif

NO_BUILD=	yes

post-extract:
	@${FIND} ${WRKSRC}/ -type f -name '.*' -delete

do-install:
	  ${MKDIR} ${STAGEDIR}/${WWWDIR}
	  (cd ${WRKSRC} && ${COPYTREE_SHARE} . ${STAGEDIR}/${WWWDIR} )

.include <bsd.port.mk>
