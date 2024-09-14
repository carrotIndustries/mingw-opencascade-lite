# Maintainer: Rafal Brzegowy <rafal.brzegowy@yahoo.com>

_realname=opencascade
pkgbase=mingw-w64-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}-lite")
provides=("${MINGW_PACKAGE_PREFIX}-${_realname}")
conflicts=("${MINGW_PACKAGE_PREFIX}-${_realname}")
pkgver=7.7.2
_pkgver2=V${pkgver//./_}
pkgrel=2
pkgdesc='Open CASCADE Technology, 3D modeling & numerical simulation (mingw-w64)'
arch=('any')
mingw_arch=('mingw64' 'ucrt64' 'clang64' 'clangarm64')
depends=("${MINGW_PACKAGE_PREFIX}-freetype"
         "${MINGW_PACKAGE_PREFIX}-tbb"
         "${MINGW_PACKAGE_PREFIX}-tcl"
         "${MINGW_PACKAGE_PREFIX}-tk")
makedepends=("${MINGW_PACKAGE_PREFIX}-cc"
             "${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-ninja"
             "${MINGW_PACKAGE_PREFIX}-doxygen"
             "${MINGW_PACKAGE_PREFIX}-qt5-base"
             "${MINGW_PACKAGE_PREFIX}-rapidjson")
license=('spdx:LGPL-2.1-or-later WITH OCCT-exception-1.0')
url='https://dev.opencascade.org'
msys2_repository_url="https://git.dev.opencascade.org/gitweb/?p=occt.git"
source=("opencascade-${pkgver}.tgz::https://git.dev.opencascade.org/gitweb/?p=occt.git;a=snapshot;h=refs/tags/${_pkgver2};sf=tgz"
        "0001-fix-compile-openvr.patch"

        # The following patches are ported from the Debian Science Team
        # https://salsa.debian.org/science-team/opencascade/-/tree/master/debian/patches

        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/master/debian/patches/0002-GeomPlate_BuildAveragePlane-BasePlan-Don-t-set-yvect.patch
        "0002-GeomPlate_BuildAveragePlane-BasePlan-Don-t-set-yvect.patch"

        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/master/debian/patches/0003-BRepFill_Filling-WireFromList-We-can-t-assume-that-a.patch
        "0003-BRepFill_Filling-WireFromList-We-can-t-assume-that-a.patch"

        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/master/debian/patches/0004-BRepFill_Filling-Curve-constraints-confused-by-impli.patch
        "0004-BRepFill_Filling-Curve-constraints-confused-by-impli.patch"

        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/master/debian/patches/0005-BRepFill_Filling-Don-t-even-attempt-to-build-with-em.patch
        "0005-BRepFill_Filling-Don-t-even-attempt-to-build-with-em.patch"

        # Source: https://salsa.debian.org/science-team/opencascade/-/blob/master/debian/patches/0006-BRepOffset_Tool-TryProject-Check-return-of-BRepLib-B.patch
        "0006-BRepOffset_Tool-TryProject-Check-return-of-BRepLib-B.patch"

        "0007-static-build.patch"
        "0008-clang-dllexport.patch"
        "0009-sse2-detetction-gcc.patch"
        "0010-Fix-TBB-detection.patch"
        "0011-Fix-linking-TKXDE.patch"
        "0013-ExpToCasExe-Fix-building-static.patch"
        "0014-aarch64-mm_alloc.patch"
        "0015-freetype.patch")

sha256sums=('2fb23c8d67a7b72061b4f7a6875861e17d412d524527b2a96151ead1d9cfa2c1'
            '0bbba5858e3e478759dac3d4802f821ec633304a63aeef542e0e4e0e0674e205'
            '9e72d389359899d959fa66b131c9871c15a136bb600c246fcf7e05910d277335'
            'f126465de547d606c4af2e95bd550ba7d5528d53a9b73693078952f9a40b3cae'
            '1f6062eece494ccec34b2b7fae0004d3970d477835f78a3fbacb0180a60f279f'
            'fa627467f355b541216a90ca9170fb87893652d6608c129bd913b19b7d1c9ee5'
            'd39ce849914e6349161675b9d412351684eb6af4bed1aa31afbdc54fa153a532'
            'c202fa3d21b3342595a15690be9e870488154a37cbfbea0a847fff62b7466f3a'
            '7143257ce2baad879df5dfb838c4730c461a6a0604fda223a614caf19ba16a46'
            '3bc45477d80f32626a301fc714e8060e08697ff3fc8ac5c10e3fdc70c77000f0'
            '3d927cd0b2def78db8def263cd40bfc8520b8c8520bf5a7543f0775e02e5e86e'
            '530d03546cb545e7b7290359fa569c0a7902ea5603a50b2834f1136b3949380f'
            '6c1e3cfebecc398424d94de43d501d2718800bf101f90a156c086fad4ba042e0'
            'daaa4243d6f88a32a3ff920af84d8add11a2582ba10cf720574871eb3503e179'
            '2e6d43b11054f239b27602d755e313bed6e614013c2af1ff99ff1bbda27a67b4')

# Helper macros to help make tasks easier #
apply_patch_with_msg() {
  for _fname in "$@"
  do
    msg2 "Applying ${_fname}"
    patch -Nbp1 -i "${srcdir}"/${_fname}
  done
}

prepare() {
  cd "${srcdir}"/occt-${_pkgver2}
  apply_patch_with_msg \
    0001-fix-compile-openvr.patch \
    0002-GeomPlate_BuildAveragePlane-BasePlan-Don-t-set-yvect.patch \
    0003-BRepFill_Filling-WireFromList-We-can-t-assume-that-a.patch \
    0004-BRepFill_Filling-Curve-constraints-confused-by-impli.patch \
    0005-BRepFill_Filling-Don-t-even-attempt-to-build-with-em.patch \
    0006-BRepOffset_Tool-TryProject-Check-return-of-BRepLib-B.patch \
    0007-static-build.patch \
    0008-clang-dllexport.patch \
    0009-sse2-detetction-gcc.patch \
    0010-Fix-TBB-detection.patch \
    0011-Fix-linking-TKXDE.patch \
    0013-ExpToCasExe-Fix-building-static.patch \
    0014-aarch64-mm_alloc.patch \
    0015-freetype.patch

  find . -name "*.orig" -exec rm -f {} \;
}

build() {
  local common_config
  common_config=(
    -DBUILD_MODULE_Draw=Off
    -DBUILD_MODULE_Visualization=Off
    -DUSE_D3D=1
    -D3RDPARTY_DIR="${MINGW_PREFIX}"
    -D3RDPARTY_TK_DIR="${MINGW_PREFIX}"
    -D3RDPARTY_TK_DLL_DIR="${MINGW_PREFIX}/bin/"
    -D3RDPARTY_TK_DLL="${MINGW_PREFIX}/bin/tk86.dll"
    -D3RDPARTY_TK_LIBRARY_DIR="${MINGW_PREFIX}/lib/"
    -D3RDPARTY_TK_LIBRARY="${MINGW_PREFIX}/lib/libtk86.dll.a"
    -D3RDPARTY_TCL_DIR="${MINGW_PREFIX}"
    -D3RDPARTY_TCL_DLL_DIR="${MINGW_PREFIX}/bin/"
    -D3RDPARTY_TCL_DLL="${MINGW_PREFIX}/bin/tcl86.dll"
    -D3RDPARTY_TCL_LIBRARY_DIR="${MINGW_PREFIX}/lib/"
    -D3RDPARTY_TCL_LIBRARY="${MINGW_PREFIX}/lib/libtcl86.dll.a"
    -D3RDPARTY_FREETYPE_DIR="${MINGW_PREFIX}"
    -D3RDPARTY_FREETYPE_DLL_DIR="${MINGW_PREFIX}/bin/"
    -D3RDPARTY_FREETYPE_DLL="${MINGW_PREFIX}/bin/libfreetype-6.dll"
    -D3RDPARTY_FREETYPE_LIBRARY_DIR="${MINGW_PREFIX}/lib/"
    -DUSE_RAPIDJSON=1
    -D3RDPARTY_RAPIDJSON_DIR="${MINGW_PREFIX}"
    -DUSE_TBB=1
    -D3RDPARTY_TBB_DLL_DIR="${MINGW_PREFIX}/bin/"
    -D3RDPARTY_TBB_DLL="${MINGW_PREFIX}/bin/libtbb12.dll"
    -D3RDPARTY_TBB_LIBRARY_DIR="${MINGW_PREFIX}/lib/"
    -D3RDPARTY_TBB_LIBRARY="${MINGW_PREFIX}/lib/libtbb12.dll.a"
    -D3RDPARTY_TBBMALLOC_DLL_DIR="${MINGW_PREFIX}/bin/"
    -D3RDPARTY_TBBMALLOC_DLL="${MINGW_PREFIX}/bin/libtbbmalloc.dll"
    -D3RDPARTY_TBBMALLOC_LIBRARY_DIR="${MINGW_PREFIX}/lib/"
    -D3RDPARTY_TBBMALLOC_LIBRARY="${MINGW_PREFIX}/lib/libtbbmalloc.dll.a"
  )

  if [[ ${MINGW_PACKAGE_PREFIX} == *-clang-i686 ]]; then
    common_config+=(
      -DBUILD_DOC_Overview=Off
    )
  fi

  declare -a _extra_config
  if check_option "debug" "n"; then
    _extra_config+=("-DCMAKE_BUILD_TYPE=Release")
  else
    _extra_config+=("-DCMAKE_BUILD_TYPE=Debug")
  fi

  mkdir -p "${srcdir}/build-${MSYSTEM}-shared" && cd "${srcdir}/build-${MSYSTEM}-shared"

  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -GNinja \
    "${_extra_config[@]}" \
    -DINSTALL_DIR_LAYOUT="Unix" \
    -DBUILD_LIBRARY_TYPE="Shared" \
    "${common_config[@]}" \
    -D3RDPARTY_FREETYPE_LIBRARY="${MINGW_PREFIX}/lib/libfreetype.dll.a" \
    -D3RDPARTY_OPENVR_LIBRARY_openvr_api="${MINGW_PREFIX}/lib/libopenvr_api.dll.a" \
    ../occt-${_pkgver2}

  ${MINGW_PREFIX}/bin/cmake --build .
}

package() {
  # Shared Install
  cd "${srcdir}/build-${MSYSTEM}-shared"
  DESTDIR="${pkgdir}" ${MINGW_PREFIX}/bin/cmake --install .

  install -Dm644 "${srcdir}/occt-${_pkgver2}/LICENSE_LGPL_21.txt" "${pkgdir}${MINGW_PREFIX}/share/licenses/${_realname}/LICENSE_LGPL_21.txt"
  install -Dm644 "${srcdir}/occt-${_pkgver2}/OCCT_LGPL_EXCEPTION.txt" "${pkgdir}${MINGW_PREFIX}/share/licenses/${_realname}/OCCT_LGPL_EXCEPTION.txt"

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
