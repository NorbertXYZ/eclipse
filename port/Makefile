PORTNAME=	eclipse
PORTVERSION=	4.32
PORTREVISION=	0
CATEGORIES=	java devel
ECLIPSE_TAG=	R4_32

DIST_SUBDIR=	${PORTNAME}

MAINTAINER=	ports@FreeBSD.org
COMMENT=	Eclipse IDE 2024-06
WWW=	https://www.eclipse.org/

LICENSE=	EPL

ONLY_FOR_ARCHS=	aarch64 amd64 powerpc64 powerpc64le

BUILD_DEPENDS= \
	git:devel/git@lite \
	mvn:devel/maven39 \
	zip:archivers/zip

LIB_DEPENDS= \
	libsecret-1.so:security/libsecret \
	libwebkit2gtk-4.0.so:www/webkit2-gtk3

USE_JAVA=	17+

USES=	compiler:c++17-lang gmake pkgconfig gnome
USE_GNOME=	gtk30

PORTSCOUT=	ignore:1

# The github repositories.  The repository under NorbertXYZ is for a predefined maven
# download, so the build does not need to download while do-build is running
USE_GITHUB=	yes
GH_TUPLE= \
	eclipse-platform:eclipse.platform.releng.aggregator:${ECLIPSE_TAG} \
	eclipse-jdt:eclipse.jdt:${ECLIPSE_TAG}:a/eclipse.jdt \
	eclipse-jdt:eclipse.jdt.core:${ECLIPSE_TAG}:b/eclipse.jdt.core \
	eclipse-jdt:eclipse.jdt.core.binaries:${ECLIPSE_TAG}:c/eclipse.jdt.core.binaries \
	eclipse-jdt:eclipse.jdt.debug:${ECLIPSE_TAG}:d/eclipse.jdt.debug \
	eclipse-jdt:eclipse.jdt.ui:${ECLIPSE_TAG}:e/eclipse.jdt.ui \
	eclipse-pde:eclipse.pde:${ECLIPSE_TAG}:f/eclipse.pde \
	eclipse-platform:eclipse.platform:${ECLIPSE_TAG}:g/eclipse.platform \
	eclipse-platform:eclipse.platform.runtime:fd42b6e331:h/eclipse.platform.runtime \
	eclipse-platform:eclipse.platform.swt:${ECLIPSE_TAG}:i/eclipse.platform.swt \
	eclipse-platform:eclipse.platform.ui:${ECLIPSE_TAG}:j/eclipse.platform.ui \
	eclipse-equinox:equinox:${ECLIPSE_TAG}:k/equinox \
	eclipse-equinox:equinox.binaries:${ECLIPSE_TAG}:l/rt.equinox.binaries \
	eclipse-equinox:p2:${ECLIPSE_TAG}:m/rt.equinox.p2 \
	NorbertXYZ:eclipse-maven:${PORTVERSION}:n

DESKTOP_ENTRIES="Eclipse" \
	"${COMMENT}" \
	"${PORTNAME}" \
	"${PORTNAME}" \
	"Development;IDE;Java;" \
	"false"

SUB_FILES=	${PORTNAME}

.include <bsd.port.pre.mk>

MAVEN_ENV=	MAVEN_OPTS=-Xmx2048m CC=${CC} CFLAGS="${CFLAGS}" JAVA_HOME=${JAVA_HOME}

# To make the build working, set the (maven) architecture to x86_64 instead of amd64
# Finally there are problems with amd64
.if ${ARCH} == amd64
MAVEN_ARCH=	x86_64
.else
MAVEN_ARCH=	${ARCH}
.endif

MAVEN_PARAMS= \
	--offline \
	-Dmaven.repo.local=${WRKDIR}/eclipse-maven-${PORTVERSION}  \
	-Dnative=gtk.freebsd.${MAVEN_ARCH} \
	-DskipTests clean verify

ECLIPSE_RESULT=	eclipse.platform.releng.tychoeclipsebuilder/eclipse.platform.repository/target/products/org.eclipse.sdk.ide-freebsd.gtk.${MAVEN_ARCH}.tar.gz

.if ${COMPILER_TYPE} == clang
CFLAGS+=	-Wno-deprecated-non-prototype
.endif

pre-patch:
	cd ${WRKSRC} && ${CP} -R ${FILESDIR}/addons/ .

do-build:
	cd ${WRKSRC} && ${SETENV} ${MAVEN_ENV} mvn ${MAVEN_PARAMS}

do-install:
	${MKDIR} ${STAGEDIR}${DATADIR}
	${TAR} -x --directory ${STAGEDIR}${DATADIR}/.. --file ${WRKSRC}/${ECLIPSE_RESULT}
	${INSTALL_SCRIPT} ${WRKDIR}/${PORTNAME} ${STAGEDIR}${PREFIX}/bin

# Generate dynamic plist, to cater for different ARCHS
post-install:
	cd ${STAGEDIR}${PREFIX} && ${FIND} -s bin/${PORTNAME} share/${PORTNAME} -not -type d >> ${TMPPLIST}
	cd ${STAGEDIR}${PREFIX} && ${FIND} -ds share/${PORTNAME} -type d | ${SED} -e 's,^,@dir ,' >> ${TMPPLIST}

.include <bsd.port.post.mk>
