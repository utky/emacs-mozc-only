# Maintainer: ilyaletre <ilyaletre at gmail dot com>
# This file derived from https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=mozc 
# which is maintained by Maintainer: ponsfoot <cabezon dot hashimoto at gmail dot com>

## If you do not want to add Japanese zip code into dictionary,
## comment out below.
_zipcode="yes"

## Mozc compile option
_bldtype=Release
#_bldtype=Debug

#*************************************************************
# Upstreams:
#
# mozc
# http://code.google.com/p/mozc/
#
# Japanese zip code data by Japan Post (Public Domain)
# http://www.post.japanpost.jp/zipcode/download.html
#
# Modified Japanese zip code data by Ibs (Public Domain)
# http://zipcloud.ibsnet.co.jp/
#*************************************************************

_zipcoderel=201703
_mozcrev=280e38fe3d9db4df52f0713acf2ca65898cd697a

pkgbase=mozc
pkgname=emcas-mozc
true && pkgname=('emacs-mozc')
pkgver=2.20.2673.102
pkgrel=2
arch=('i686' 'x86_64')
url="http://code.google.com/p/mozc/"
license=('BSD' 'custom')
makedepends=('python2' 'git' 'ninja' 'clang' 'qt5-base' 'emacs')
#source=("${_svndir}/${_svnmod}::svn+${_svntrunk}"
source=(
  mozc::git+https://github.com/google/mozc.git#commit=${_mozcrev}
  http://downloads.sourceforge.net/project/pnsft-aur/mozc/x-ken-all-${_zipcoderel}.zip
  http://downloads.sourceforge.net/project/pnsft-aur/mozc/jigyosyo-${_zipcoderel}.zip
)
sha1sums=('SKIP'
          '238b182e553ade5a92cb6502f5f81e2ca34ecc26'
          'd2ba37f4bd5d20c85c7e732a4a1d50704d384319')

pkgver() {
  . "${srcdir}/${pkgbase}/src/data/version/mozc_version_template.bzl"
  printf "%s.%s.%s.%s" $MAJOR $MINOR $BUILD $REVISION
}


prepare() {
  ln -sf `which python2` "${srcdir}/python"

  cd "${srcdir}/${pkgbase}/"

  git submodule update --init --recursive

  cd "${srcdir}/${pkgbase}/src/"

  # Extract liccense part of mozc
  head -n 29 server/mozc_server.cc > LICENSE

  # Generate zip code dictionary seed
  if [[ "$_zipcode" == "yes" ]]; then
    msg "Generating zip code dict seed..."
    cd "$srcdir"
    PYTHONPATH="${PYTHONPATH}:${srcdir}/${pkgbase}/src/" \
              python2 mozc/src/dictionary/gen_zip_code_seed.py \
              --zip_code=x-ken-all.csv \
              --jigyosyo=JIGYOSYO.CSV \
              >> "${srcdir}/${pkgbase}/src/data/dictionary_oss/dictionary09.txt"
    msg "Done."
  fi

}


build() {
  msg2 '====================================================='
  msg2 '               *** Build Info ***'
  msg2 ' The following package files will be generated:'
  for _p in ${pkgname[@]}
  do
    msg2 "  * ${_p}-${pkgver}-${pkgrel}-${CARCH}${PKGEXT}"
  done
  msg2 '====================================================='

  PATH="${srcdir}:${PATH}"

  cd "${srcdir}/${pkgbase}/src"

  msg "Starting make..."

  _targets="unix/emacs/emacs.gyp:mozc_emacs_helper "

  unset CC CC_host CC_target CXX CXX_host CXX_target LINK AR AR_host AR_target \
        NM NM_host NM_target READELF READELF_host READELF_target
  GYP_DEFINES="document_dir=/usr/share/licenses/${pkgbase}" \
    python2 build_mozc.py gyp --target_platform=Linux
  python2 build_mozc.py build -c $_bldtype $_targets


}

package_emacs-mozc() {
  pkgdesc="Mozc for Emacs"
  arch=('i686' 'x86_64')
  groups=('mozc-im')
  depends=('emacs')
  replaces=('emacs-mozc-bin')
  conflicts=('emacs-mozc-bin')
  install=emacs-mozc.install

  cd "${srcdir}/${pkgbase}/src"
  install -D -m 755 out_linux/${_bldtype}/mozc_emacs_helper "${pkgdir}/usr/bin/mozc_emacs_helper"
  install -d "${pkgdir}/usr/share/emacs/site-lisp/emacs-mozc/"
  install -m 644 unix/emacs/mozc.el "${pkgdir}/usr/share/emacs/site-lisp/emacs-mozc"
}

# Global pkgdesc and depends are here so that they will be picked up by AUR
pkgdesc="A Japanese Input Method for Chromium OS, Windows, Mac and Linux (the Open Source Edition of Google Japanese Input)"
depends=('qt5-base' 'zinnia')
