# Created by: Li-Wen Hsu <lwhsu@FreeBSD.org>
# $FreeBSD$

PORTNAME=	python-jenkins
PORTVERSION=	0.4.14
CATEGORIES=	devel python
MASTER_SITES=	CHEESESHOP
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	lwhsu@FreeBSD.org
COMMENT=	Python bindings for the remote Jenkins API

LICENSE=	BSD3CLAUSE

BUILD_DEPENDS=	${PYTHON_PKGNAMEPREFIX}pbr>=0.8.2:devel/py-pbr
RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}pbr>=0.8.2:devel/py-pbr \
		${PYTHON_PKGNAMEPREFIX}six>0:devel/py-six \
		${PYTHON_PKGNAMEPREFIX}multi_key_dict>0:devel/py-multi_key_dict

USES=		python
USE_PYTHON=	distutils autoplist

.include <bsd.port.mk>
