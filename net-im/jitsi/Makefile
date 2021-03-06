# Created by: Tzanetos Balitsaris <tzabal@it.teithe.gr>
# $FreeBSD$

PORTNAME=	jitsi
PORTVERSION=	2.10.5550
PORTREVISION=	0
CATEGORIES=	net-im java
MASTER_SITES=	https://download.jitsi.org/jitsi/src/
DISTNAME=	${PORTNAME}-src-${PORTVERSION}

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Open source IM and VoIP application

LICENSE=	LGPL21

BROKEN=		fails to start, missing requirement osgi.native
ONLY_FOR_ARCHS=	i386 amd64

USES=		zip
USE_JAVA=	yes
USE_ANT=	yes
USE_XORG=	x11
MAKE_ARGS=	rebuild
USE_LDCONFIG=	${PREFIX}/lib/jitsi/lib/native

WRKSRC=		${WRKDIR}/${PORTNAME}

DESKTOP_ENTRIES="Jitsi" \
		"VoIP and Instant Messenger" \
		"${PREFIX}/share/pixmaps/jitsi.png" \
		"${PORTNAME}" \
		"Network;InstantMessaging;VideoConference;Java;" \
		true

do-install:
# Installation directory structure
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/jitsi/sc-bundles
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/jitsi/lib/bundle
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/jitsi/lib/native
# Bundles (Jitsi modules)
	cd ${WRKSRC}/sc-bundles && ${COPYTREE_SHARE} . \
		${STAGEDIR}${PREFIX}/lib/jitsi/sc-bundles
	${RM} ${STAGEDIR}${PREFIX}/lib/jitsi/sc-bundles/*slick*.jar
	${RM} -r ${STAGEDIR}${PREFIX}/lib/jitsi/sc-bundles/os-specific
# Library JARs
	cd ${WRKSRC}/lib/bundle && ${COPYTREE_SHARE} . \
		${STAGEDIR}${PREFIX}/lib/jitsi/lib/bundle "! -name junit.jar"
	cd ${WRKSRC}/lib && ${COPYTREE_SHARE} "felix.jar logging.properties \
		felix.client.run.properties" ${STAGEDIR}${PREFIX}/lib/jitsi/lib
# Native libraries
	cd ${WRKSRC}/lib/native/freebsd${ARCH:S/i386//:S/amd/-/:S/x86_/-/} && \
		${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/lib/jitsi/lib/native
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/lib/jitsi/lib/native/*.so
# Wrapper script to run Jitsi
	${INSTALL_SCRIPT} ${FILESDIR}/jitsi.sh ${STAGEDIR}${PREFIX}/bin/jitsi
# Manual page
	${INSTALL_MAN} ${FILESDIR}/jitsi.1 ${STAGEDIR}${MANPREFIX}/man/man1
# Desktop related
	${INSTALL_DATA} ${WRKSRC}/resources/images/logo/sc_logo_128x128.png \
		${STAGEDIR}${PREFIX}/share/pixmaps/jitsi.png

.include <bsd.port.mk>
