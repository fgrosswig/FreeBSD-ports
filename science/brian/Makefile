# Created by: wenheping@gmail.com
# $FreeBSD$

PORTNAME=	brian
PORTVERSION=	1.4.3
CATEGORIES=	science python
MASTER_SITES=	CHEESESHOP

MAINTAINER=	wen@FreeBSD.org
COMMENT=	Clock-driven simulator for spiking neural networks

BUILD_DEPENDS=	${PYTHON_PKGNAMEPREFIX}numpy>=1.1.0,1:math/py-numpy \
		${PYTHON_PKGNAMEPREFIX}scipy>=0.6.0:science/py-scipy \
		${PYTHON_PKGNAMEPREFIX}matplotlib>=0.90.1:math/py-matplotlib
RUN_DEPENDS:=	${BUILD_DEPENDS}

USES=		python zip
USE_PYTHON=	distutils autoplist

.include <bsd.port.mk>
