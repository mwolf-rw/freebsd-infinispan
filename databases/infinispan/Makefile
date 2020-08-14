# $FreeBSD$

PORTNAME=	infinispan
PORTVERSION=	11.0.1
CATEGORIES=	databases java
MASTER_SITES=	https://downloads.jboss.org/${PORTNAME}/${PORTVERSION}.Final/
DISTNAME=	${PORTNAME}-server-${PORTVERSION}.Final

MAINTAINER=	freebsd@rheinwolf.de
COMMENT=	Scalable key/value data store and data grid

LICENSE=	APACHE20

USES=		zip

USE_RC_SUBR=	infinispan

USE_JAVA=	yes
JAVA_VERSION=	1.8+
JAVA_OS=	native
JAVA_VENDOR=	openjdk oracle
JAVA_RUN=	yes

USER=		infinispan
GROUP=		infinispan

VAR_DIR?=	/var
DB_DIR=		${VAR_DIR}/db/${PORTNAME}
LOG_DIR?=	${VAR_DIR}/log/${PORTNAME}
RUN_DIR?=	${VAR_DIR}/run/${PORTNAME}

SUB_LIST+=	DB_DIR=${DB_DIR} \
		GROUP=${GROUP} \
		LOG_DIR=${LOG_DIR} \
		JAVA=${JAVA} \
		PORTNAME=${PORTNAME} \
		RUN_DIR=${RUN_DIR} \
		USER=${USER}

NO_BUILD=	yes
NO_ARCH=	yes

CONFIG_FILES=	bin/cli.log4j2.xml \
		bin/server.conf

post-patch:
.for f in ${CONFIG_FILES}
	${MV} ${WRKSRC}/${f} ${WRKSRC}/${f}.sample
.endfor

do-install:
	${MKDIR} ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/bin
	${MKDIR} ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/boot
	${MKDIR} ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/lib
	${MKDIR} ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/server-default
	${MKDIR} ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/static
	${MKDIR} ${STAGEDIR}${DB_DIR}
	${MKDIR} ${STAGEDIR}${LOG_DIR}
	${MKDIR} ${STAGEDIR}${RUN_DIR}

	# Install files in bin
	${INSTALL_SCRIPT} ${WRKSRC}/bin/*.sh ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/bin
	${INSTALL_DATA} ${WRKSRC}/bin/cli.log4j2.xml.sample ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/bin
	${INSTALL_DATA} ${WRKSRC}/bin/server.conf.sample ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/bin

	# Install files in boot
	${INSTALL_DATA} ${WRKSRC}/boot/*.jar ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/boot

	# Install files in lib
	${INSTALL_DATA} ${WRKSRC}/lib/*.jar ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/lib

	# Install files in docs, static
	(cd ${WRKSRC} && ${COPYTREE_SHARE} "docs static" ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME})

	# Create default database dir
	(cd ${WRKSRC}/server && ${COPYTREE_SHARE} . ${STAGEDIR}${JAVASHAREDIR}/${PORTNAME}/server-default)

.include <bsd.port.mk>
