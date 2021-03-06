# Created by: Guerkan Karaman <gk.freebsd@googlemail.com>
# $FreeBSD$

PORTNAME=	ipxe
DISTVERSION=	20200314
CATEGORIES=	net

MAINTAINER=	tobik@FreeBSD.org
COMMENT=	Open source network boot firmware

LICENSE=	GPLv2

ONLY_FOR_ARCHS=	amd64 i386
ONLY_FOR_ARCHS_REASON=	not yet ported to anything other than amd64/i386

USES=		gmake perl5 shebangfix
USE_BINUTILS=	yes
USE_GCC=	yes
USE_GITHUB=	yes
GH_TAGNAME=	bdf0e029ae8c9a0e173d5988ee64ff910538af53
USE_PERL5=	build
SHEBANG_FILES=	util/geniso util/gensdsk
MAKE_ARGS=	AR=${AR} \
		ARCH=${ARCH:S/amd64/i386/} \
		AS=${AS} \
		CC=${CC} \
		HOST_CC=${CC} \
		LD=${LD} \
		NM=${NM} \
		NO_WERROR=1 \
		OBJCOPY=${OBJCOPY} \
		OBJDUMP=${OBJDUMP} \
		PERL=${PERL} \
		RANLIB=${RANLIB} \
		SIZE=${SIZE} \
		V=1
# Append user-settable build args from make.conf
MAKE_ARGS+=	${IPXE_MAKE_ARGS}
ALL_TARGET=	bin/ipxe.dsk bin/ipxe.lkrn bin/ipxe.pxe bin/ipxe.usb \
		bin/undionly.kpxe
# VMware ROMs
ALL_TARGET+=	bin/8086100f.mrom bin/808610d3.mrom bin/10222000.rom \
		bin/15ad07b0.rom
SUB_FILES=	pkg-message
WRKSRC_SUBDIR=	src
PLIST_FILES=	${ALL_TARGET:S@^bin/@${DATADIR}/@}

# Possible options are listed in ${WRKSRC}/config/*.h and
# http://ipxe.org/buildcfg.  Add a ! prefix to an option to undef it
# i.e. turn it off.
_IPXE_BUILDCFG=	branding:PRODUCT_NAME="${PKGNAME} (${OPSYS} ${OSREL}/${ARCH})" \
		console:CONSOLE_SERIAL \
		console:KEYBOARD_MAP=us \
		general:!DOWNLOAD_PROTO_FTP \
		general:DOWNLOAD_PROTO_HTTPS \
		general:DOWNLOAD_PROTO_NFS \
		general:IMAGE_TRUST_CMD \
		general:NET_PROTO_IPV6 \
		general:PING_CMD \
		general:POWEROFF_CMD

OPTIONS_DEFINE=		EFI ISO
OPTIONS_DEFAULT=	EFI ISO

EFI_DESC=	Create EFI image
ISO_DESC=	Create bootable CD image

_EFI_ARCHS=		${ARCH:Mamd64:S/amd64/x86_64/} i386
.for _arch in ${_EFI_ARCHS}
EFI_PLIST_FILES+=	${DATADIR}/ipxe.efi-${_arch}
.endfor

ISO_BUILD_DEPENDS=	${LOCALBASE}/share/syslinux/bios/core/isolinux.bin:sysutils/syslinux \
			bash:shells/bash \
			mkisofs:sysutils/cdrtools
ISO_MAKE_ARGS=		ISOLINUX_BIN="${WRKSRC}/isolinux.bin" \
			LDLINUX_C32="${LOCALBASE}/share/syslinux/bios/com32/elflink/ldlinux/ldlinux.c32"
ISO_ALL_TARGET=		bin/ipxe.iso

.include <bsd.port.pre.mk>

# Add user supplied build configuration after port options etc have
# been processed as they might want to overwrite some settings.
_IPXE_BUILDCFG+=	${IPXE_BUILDCFG}

# XXX: iPXE uses csplit when embedding certificates and assumes
# it's GNU csplit, so give it what it wants as a workaround if
# CERT=... is in {IPXE_,}MAKE_ARGS.
.if ${MAKE_ARGS:MCERT=*}
BUILD_DEPENDS+=	gcsplit:sysutils/coreutils
MAKE_ARGS+=	CSPLIT=gcsplit
.endif

do-configure:
.for _cat _opt in ${_IPXE_BUILDCFG:C/\:/ /}
	@${ECHO_CMD} '#undef ${_opt:S/^!//:C/(.*)=(.*)/\1/}' >> ${WRKSRC}/config/local/${_cat}.h
.if ${_opt:N!*}
	@${ECHO_CMD} '#define ${_opt:C/=/ /:S/^'//:S/'$//}' >> ${WRKSRC}/config/local/${_cat}.h
.endif
.endfor

pre-build-ISO-on:
# ISO creation fails if isolinux.bin is read only
	${INSTALL} -m 644 ${LOCALBASE}/share/syslinux/bios/core/isolinux.bin ${WRKSRC}/isolinux.bin

do-build-EFI-on:
.for _arch in ${_EFI_ARCHS}
	${DO_MAKE_BUILD} ARCH=${_arch} -C ${WRKSRC} bin-${_arch}-efi/ipxe.efi
.endfor

do-install:
	@${MKDIR} ${STAGEDIR}${DATADIR}
	cd ${WRKSRC} && ${INSTALL_DATA} ${ALL_TARGET} ${STAGEDIR}${DATADIR}

do-install-EFI-on:
.for _arch in ${_EFI_ARCHS}
	${INSTALL_DATA} ${WRKSRC}/bin-${_arch}-efi/ipxe.efi ${STAGEDIR}${DATADIR}/ipxe.efi-${_arch}
.endfor

.include <bsd.port.post.mk>

RUN_DEPENDS:=	${RUN_DEPENDS:Ngcc*}
