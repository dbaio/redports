# Created by: Pakhom Golynga <pakhom706@gmail.com>
# $FreeBSD: head/net-mgmt/zabbix22-agent/Makefile 351870 2014-04-22 18:31:13Z rene $

PORTNAME=	zabbix22
PKGNAMESUFFIX=	-agent
CATEGORIES=	net-mgmt

MASTERDIR=	${.CURDIR}/../zabbix22-server

PLIST=		${PKGDIR}/pkg-plist.agent

OPTIONSFILE=	${PORT_DBDIR}/${PORTNAME}${PKGNAMESUFFIX}/options
OPTIONS_DEFINE=	IPV6
OPTIONS_DEFAULT=	IPV6

.include <bsd.port.options.mk>
.include "${MASTERDIR}/Makefile"
