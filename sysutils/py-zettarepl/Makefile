# $FreeBSD$

PORTNAME=	zettarepl
PORTVERSION=	0.1
PORTREVISION=	31
CATEGORIES=	sysutils python
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	vladimirv@ixsystems.com
COMMENT=	Cross-platform ZFS replication solution

USES=		python:3.6+
USE_PYTHON=	autoplist distutils
USE_GITHUB=	yes
GH_ACCOUNT=	truenas
GH_TAGNAME=	099779a71697cb1aa0bc7315528f1dab4440576b

RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}coloredlogs>0:devel/py-coloredlogs@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}croniter>0:sysutils/py-croniter@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}isodate>0:devel/py-isodate@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}jsonschema>0:devel/py-jsonschema@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}paramiko>0:security/py-paramiko@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}dateutil>0:devel/py-dateutil@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pytz>0:devel/py-pytz@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}yaml>0:devel/py-yaml@${PY_FLAVOR}

.include <bsd.port.mk>
