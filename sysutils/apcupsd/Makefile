# Created by: Lars Köller <Lars.Koeller@Uni-Bielefeld.DE>
# $FreeBSD: head/sysutils/apcupsd/Makefile 327772 2013-09-20 23:05:58Z bapt $

PORTNAME=	apcupsd
PORTVERSION=	3.14.12
CATEGORIES=	sysutils
MASTER_SITES=	SF/${PORTNAME}/${PORTNAME}%20-%20Stable/${PORTVERSION}:src_sf \
		http://T32.TecNik93.com/FreeBSD/ports/${PORTNAME}/sources/:src_bk \
		http://apcupsd.sourceforge.net/manual/:doc_sf \
		http://T32.TecNik93.com/FreeBSD/ports/${PORTNAME}/sources/:doc_bk
DISTFILES=	${DISTNAME}${EXTRACT_SUFX}:src_sf,src_bk \
		${PORTNAME}.pdf:doc_sf,doc_bk
EXTRACT_ONLY=	${DISTNAME}${EXTRACT_SUFX}

MAINTAINER=	itetcu@FreeBSD.org
COMMENT=	Set of programs for controlling APC UPS

LICENSE=	GPLv2

USE_RC_SUBR=	apcupsd
SUB_FILES=	pkg-message

GNU_CONFIGURE=	yes
USES=	gmake
CONFIGURE_ARGS=	--sbindir=${PREFIX}/sbin \
		--with-nologin=/var/run \
		--disable-install-distdir \
		--sysconfdir=${ETCDIR} \
		--with-serial-dev=/dev/usv

CONFIGURE_ENV+=	LD="${CXX}"
CONFIGURE_ENV+=	SHUTDOWN="/sbin/shutdown"
CONFIGURE_ENV+=	LIBS="-lstdc++"
CPPFLAGS+=	-I${LOCALBASE}/include -L${LOCALBASE}/lib
LDFLAGS+=	-L${LOCALBASE}/lib
CFLAGS+=	-I${LOCALBASE}/include -L${LOCALBASE}/lib

OPTIONS_DEFINE=	APCSMART_DRV APCDUMB_DRV CLIENT_ONLY CGI PCNET_DRV USB SNMP_DRV \
		SNMP_DRV_OLD TCP_WRAPPERS TEST_DRV GAPCMON DOCS
OPTIONS_DEFAULT=	APCSMART_DRV APCDUMB_DRV PCNET_DRV USB TCP_WRAPPERS
APCSMART_DRV_DESC=	Compile APC SmartUPS serial driver
APCDUMB_DRV_DESC=	Compile dumb UPS driver
CLIENT_ONLY_DESC=	Only NIS client (no network server or drivers)
CGI_DESC=		Compile with CGI programms to show status
PCNET_DRV_DESC=		Compile PowerChute Network Shutdown driver
USB_DESC=		Compile with USB Support driver
SNMP_DRV_DESC=		Compile with SNMP driver
SNMP_DRV_OLD_DESC=	Compile with old SNMP driver
TCP_WRAPPERS_DESC=	Compile with TCP_WRAPPERS support
TEST_DRV_DESC=		Compile TEST driver
GAPCMON_DESC=		Build GTK GUI front-end

OPTIONS_SUB=	yes

PORTDOCS=	${PORTNAME}.pdf
IGNOREFILES=	${PORTNAME}.pdf

APCSMART_DRV_CONFIGURE_ENABLE=	apcsmart

APCDUMB_DRV_CONFIGURE_ENABLE=	dumb

CLIENT_ONLY_CONFIGURE_ON=	--enable-net \
				--disable-apcsmart --disable-dumb --disable-usb \
				--disable-snmp --disable-test --disable-pcnet

CGI_CONFIGURE_ON=	--enable-cgi --with-cgi-bin=${ETCDIR}/cgi
CGI_LIB_DEPENDS=	libgd.so:${PORTSDIR}/graphics/gd

GAPCMON_CONFIGURE_ENABLE=	gapcmon
GAPCMON_USE=		GNOME=gconf2
GAPCMON_CONFLICTS=	gapcmon-[0-9]*

PCNET_DRV_CONFIGURE_ENABLE=	pcnet

USB_CONFIGURE_ENABLE=	usb

SNMP_DRV_CONFIGURE_ENABLE=	snmp

SNMP_DRV_OLD_CONFIGURE_ENABLE=	net-snmp
SNMP_DRV_OLD_LIB_DEPENDS=	libnetsnmp.so:${PORTSDIR}/net-mgmt/net-snmp

TCP_WRAPPERS_CONFIGURE_ON=	--with-libwrap=yes

TEST_DRV_CONFIGURE_ENABLE=	test

.include <bsd.port.options.mk>

pre-configure:
.if ${PORT_OPTIONS:MCLIENT_ONLY} && ( ${PORT_OPTIONS:MAPCSMART_DRV} || \
	${PORT_OPTIONS:MAPCDUMB_DRV} || ${PORT_OPTIONS:MPCNET_DRV} || \
	${PORT_OPTIONS:MUSB} || ${PORT_OPTIONS:MSNMP_DRV} || \
	${PORT_OPTIONS:MTEST_DRV} )
	@${ECHO_CMD} "It doesn't make sense to build drivers if all you want is"
	@${ECHO_CMD} "the client; please do make config and select either"
	@${ECHO_CMD} "CLIENT_ONLY or the *_DRV you need"
	@${FALSE}
.endif

post-install:
	for na in apccontrol changeme commfailure commok offbattery onbattery; do \
		${MV} ${STAGEDIR}${ETCDIR}/$$na ${STAGEDIR}${ETCDIR}/$$na.sample; \
	done
	@${MV} ${STAGEDIR}${ETCDIR}/apcupsd.conf ${STAGEDIR}${ETCDIR}/apcupsd.conf.sample
.if ${PORT_OPTIONS:MCGI}
		for na in apcupsd.css hosts.conf multimon.conf; do \
			${MV} ${STAGEDIR}${ETCDIR}/$$na ${STAGEDIR}${ETCDIR}/$$na.sample; \
		done
.endif
	@${ECHO_CMD} "@unexec if [ -d %D/${ETCDIR_REL} ]; then ${ECHO_CMD} \"If you are permanently removing this port, you should do a ``rm -rf ${ETCDIR}`` to remove config files left.\" | ${FMT} ; fi" >> ${TMPPLIST}
.if ${PORT_OPTIONS:MDOCS}
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	${INSTALL_DATA} ${DISTDIR}/${PORTNAME}.pdf ${STAGEDIR}${DOCSDIR}
.endif

.include <bsd.port.mk>
