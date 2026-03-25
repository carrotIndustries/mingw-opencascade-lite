# Maintainer: Rafal Brzegowy <rafal.brzegowy@yahoo.com>

_realname=opencascade
pkgbase=mingw-w64-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}-lite")
provides=("${MINGW_PACKAGE_PREFIX}-${_realname}")
conflicts=("${MINGW_PACKAGE_PREFIX}-${_realname}")
pkgver=7.9.2
_pkgver=${pkgver//./_}
pkgrel=1
pkgdesc='Open CASCADE Technology, 3D modeling & numerical simulation (mingw-w64)'
arch=('any')
mingw_arch=('mingw64' 'ucrt64' 'clang64' 'clangarm64')
url='https://dev.opencascade.org'
msys2_repository_url="https://github.com/Open-Cascade-SAS/OCCT"
license=('spdx:LGPL-2.1-or-later WITH OCCT-exception-1.0')
depends=("${MINGW_PACKAGE_PREFIX}-tbb")
makedepends=("${MINGW_PACKAGE_PREFIX}-cc"
             "${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-ninja"
             "${MINGW_PACKAGE_PREFIX}-doxygen"
             "${MINGW_PACKAGE_PREFIX}-rapidjson"
             "${MINGW_PACKAGE_PREFIX}-nlohmann-json")
source=("https://github.com/Open-Cascade-SAS/OCCT/archive/refs/tags/V${_pkgver}/${_realname}-${pkgver}.tar.gz"
        0001-do-not-redefine-WIN32_WINNT.patch
        # https://github.com/Open-Cascade-SAS/OCCT/commit/22d437b7
        0002-fix-build-with-ffmpeg.patch
        # The following patches are ported from the Debian Science Team
        # https://salsa.debian.org/science-team/opencascade/-/tree/debian/experimental/debian/patches
        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/debian/experimental/debian/patches/0003-BRepFill_Filling-WireFromList-We-can-t-assume-that-a.patch
        "0003-BRepFill_Filling-WireFromList-We-can-t-assume-that-a.patch"
        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/debian/experimental/debian/patches/0004-BRepFill_Filling-Curve-constraints-confused-by-impli.patch
        "0004-BRepFill_Filling-Curve-constraints-confused-by-impli.patch"
        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/debian/experimental/debian/patches/0006-BRepOffset_Tool-TryProject-Check-return-of-BRepLib-B.patch
        "0006-BRepOffset_Tool-TryProject-Check-return-of-BRepLib-B.patch"
        "0007-static-build.patch"
        "0009-SSE2-detection-gcc.patch"
        "0010-Fix-TBB-detection.patch"
        "0013-ExpToCasExe-Fix-building-static.patch"
        "0014-aarch64-mm_alloc.patch")
sha256sums=('3cd080d3fc33ba0c6c157e110afe3e015859524c4694dbb09812ec9d61595639'
            '018829156bc3fc0f44c600c67cd21f141c0051c1f15fe6bdcf3f3246a19dc141'
            'e5ff9ba9f14918c297e48cd45fac8133638664934bad4e5075d798ff1809977f'
            '42edcf398e0670012bf2668bd493997a43ab82a24a1c889bfb080e5256776619'
            'c6f06b012f3ff6b4355151af4981593f87c12ccb2aaaa15c2cd65799f9efc336'
            'c3cdd50ddeace18259341a25041b90ed18e73898161b81242868ae6a53a2c848'
            '3efa1efaad5983100fc736e36a40ea0dba46cbdfe4e7f2f6e7853c582e127aa0'
            '226c000886a901d62cc09ca550009e4bbae84933a3b1bbded5c4f9f246734b08'
            '3d927cd0b2def78db8def263cd40bfc8520b8c8520bf5a7543f0775e02e5e86e'
            '6c1e3cfebecc398424d94de43d501d2718800bf101f90a156c086fad4ba042e0'
            'b375c36d5c8a3e12f428db905570fa3ae53ab444caf50ae674069749f78f1918')

# Helper macros to help make tasks easier #
apply_patch_with_msg() {
  for _fname in "$@"
  do
    msg2 "Applying ${_fname}"
    patch -Nbp1 -i "${srcdir}"/${_fname}
  done
}

prepare() {
  cd "${srcdir}"/OCCT-${_pkgver}
  apply_patch_with_msg \
    0001-do-not-redefine-WIN32_WINNT.patch \
    0002-fix-build-with-ffmpeg.patch \
    0003-BRepFill_Filling-WireFromList-We-can-t-assume-that-a.patch \
    0004-BRepFill_Filling-Curve-constraints-confused-by-impli.patch \
    0006-BRepOffset_Tool-TryProject-Check-return-of-BRepLib-B.patch \
    0007-static-build.patch \
    0009-SSE2-detection-gcc.patch \
    0010-Fix-TBB-detection.patch \
    0013-ExpToCasExe-Fix-building-static.patch \
    0014-aarch64-mm_alloc.patch

  find . -name "*.orig" -exec rm -f {} \;
}

build() {
  local common_config
  common_config=(
    -DBUILD_MODULE_Draw=Off
    -DBUILD_MODULE_Visualization=Off
    -DUSE_D3D=ON
	-DUSE_FREETYPE=Off
    -D3RDPARTY_DIR="${MINGW_PREFIX}"
    -DUSE_RAPIDJSON=ON
    -D3RDPARTY_RAPIDJSON_DIR="${MINGW_PREFIX}"
  )

  declare -a _extra_config
  if check_option "debug" "n"; then
    _extra_config+=("-DCMAKE_BUILD_TYPE=Release")
  else
    _extra_config+=("-DCMAKE_BUILD_TYPE=Debug")
  fi


  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake \
    -GNinja \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    "${_extra_config[@]}" \
    -DINSTALL_DIR_LAYOUT="Unix" \
    -DBUILD_LIBRARY_TYPE="Shared" \
    "${common_config[@]}" \
    -S OCCT-${_pkgver} \
    -B build-${MSYSTEM}

  ${MINGW_PREFIX}/bin/cmake --build build-${MSYSTEM}
}

package() {
  DESTDIR="${pkgdir}" ${MINGW_PREFIX}/bin/cmake --install build-${MSYSTEM}

  install -Dm644 "${srcdir}"/OCCT-${_pkgver}/LICENSE_LGPL_21.txt \
    "${pkgdir}"${MINGW_PREFIX}/share/licenses/${_realname}/LICENSE_LGPL_21.txt
  install -Dm644 "${srcdir}"/OCCT-${_pkgver}/OCCT_LGPL_EXCEPTION.txt \
    "${pkgdir}"${MINGW_PREFIX}/share/licenses/${_realname}/OCCT_LGPL_EXCEPTION.txt

  # Remove unnecessary batch scripts (#16304)
  rm -v "${pkgdir}${MINGW_PREFIX}/bin/"*.bat

  # Fix cmake files
  find "${pkgdir}${MINGW_PREFIX}/lib/cmake" -type f \( -name '*.cmake' \) \
      -exec sed -i -e "s|$(cygpath -m "${MINGW_PREFIX}")|\$\{_IMPORT_PREFIX\}|g" {} \;

  find "${pkgdir}${MINGW_PREFIX}/lib/cmake" -type f \( -name '*.cmake' \) \
      -exec sed -i -e 's|\${OCCT_INSTALL_BIN_LETTER}||g' {} \;

  find "${pkgdir}${MINGW_PREFIX}/lib/cmake" -type f \( -name '*.cmake' \) \
      -exec sed -i -e "s|$(cygpath -m "${MINGW_PREFIX}")/lib/||g" {} \;
}
