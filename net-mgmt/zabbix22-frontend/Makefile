# Created by: Jim Riggs <ports@christianserving.org>
# $FreeBSD: head/net-mgmt/zabbix22-frontend/Makefile 351870 2014-04-22 18:31:13Z rene $

PORTNAME=	zabbix22
PKGNAMESUFFIX=	-frontend
CATEGORIES=	net-mgmt

MASTERDIR=	${.CURDIR}/../zabbix22-server

NO_BUILD=	yes
PATCHDIR=
PLIST=		${PKGDIR}/pkg-plist.frontend

USE_PHP=	bcmath ctype gd pcre snmp sockets mbstring session dom xml \
		xmlreader xmlwriter simplexml gettext ldap
WANT_PHP_WEB=	yes

OPTIONSFILE=	${PORT_DBDIR}/${PORTNAME}${PKGNAMESUFFIX}/options
OPTIONS_DEFINE=	MYSQL MYSQLI PGSQL SQLITE ORACLE
OPTIONS_DEFAULT=	MYSQL MYSQLI
MYSQLI_DESC=	MySQLI backend

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MMYSQL}
USE_PHP+=	mysql
.endif

.if ${PORT_OPTIONS:MMYSQLI}
USE_PHP+=	mysqli
.endif

.if ${PORT_OPTIONS:MPGSQL}
USE_PHP+=	pgsql
.endif

.if ${PORT_OPTIONS:MSQLITE}
USE_PHP+=	sqlite3
.endif

.if ${PORT_OPTIONS:MORACLE}
ZABBIX_REQUIRE=
CONFIGURE_ARGS+=        --with-oracle
.endif

do-install:
	@${INSTALL} -d ${STAGEDIR}${WWWDIR}
	@cd ${WRKSRC}/frontends/php/ && ${COPYTREE_SHARE} . ${STAGEDIR}${WWWDIR}

.include "${MASTERDIR}/Makefile"
