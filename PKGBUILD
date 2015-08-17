# Maintainer  : Anish Bhatt anish [at] gatech [dot] edu
# Contributor : Jesus Alvarez <jeezusjr@gmail.com>
# Contributor : sxe <sxxe@gmx.de>
# Based on the wine package in the community repository

pkgname=wine-silverlight
pkgver=1.7.43
pkgrel=1
pkgdesc="wine-staging, wine patched with extra funtionality including NPAPI plugin compatibility"
url="http://www.wine-staging.com/"
license=('LGPL2.1')
arch=('x86_64' 'i686')
options=(staticlibs !ccache)
install=wine-silverlight.install

# Change based on whether you are interested in experimental d3d9 support
enable_d3d9_support=0

# source=(http://prdownloads.sourceforge.net/wine/wine-${pkgver}.tar.bz2{,.sign}
source=(http://prdownloads.sourceforge.net/wine/wine-${pkgver}.tar.bz2
        "https://github.com/wine-compholio/wine-staging/archive/v${pkgver}.tar.gz"
        "https://github.com/NP-Hardass/wine-d3d9-patches/archive/wine-d3d9-${pkgver}.tar.gz"
        "30-win32-aliases.conf")

md5sums=('5158c559dedd9e7668a1fcb9d573b309'
         '62c4e94265e068c064988bee2fc016e9'
         '20de18e1f7ce4ea7fd39b785621ea3b7'
         '1ff4e467f59409272088d92173a0f801')

#validpgpkeys=(5AC1A08B03BD7A313E0A955AF5E6E9EEB9461DD7) # Alexandre Julliard

depends=('fontconfig'
         'libxcursor'
         'libxrandr'
         'libxdamage'
         'libxi'
         'gettext'
         'freetype2'
         'glu'
         'libsm'
         'gcc-libs'
         'attr'
         'desktop-file-utils')

makedepends=('libgl'
             'autoconf'
             'ncurses'
             'bison'
             'perl'
             'fontforge'
             'flex'
             'prelink'
             'gcc>=4.5.0-2'
             'giflib'
             'libpng'
             'gnutls'
             'libxinerama'
             'libxcomposite'
             'libxmu'
             'libxxf86vm'
             'libxml2'
             'libldap'
             'lcms'
             'mpg123'
             'openal'
             'v4l-utils'
             'alsa-lib'
             'mesa'
             'libcl'
             'opencl-headers'
             'pulseaudio'
             'samba')

optdepends=('dri2proto : for d3d9 support'
            'giflib'
            'libpng'
            'libldap'
            'gnutls'
            'lcms'
            'libxml2'
            'mpg123'
            'openal'
            'v4l-utils'
            'libpulse'
            'alsa-plugins'
            'alsa-lib'
            'libjpeg-turbo'
            'libxcomposite'
            'libxinerama'
            'libncurses'
            'libcl'
            'pulseaudio'
            'oss'
            'cups'
            'samba'
            'libtxc_dxtn')

if [[ $CARCH == x86_64 ]]; then

  depends=('lib32-fontconfig'
           'lib32-libxcursor'
           'lib32-libxrandr'
           'lib32-libxdamage'
           'lib32-libxi'
           'lib32-gettext'
           'lib32-glu'
           'lib32-libsm'
           'lib32-gcc-libs'
           'lib32-attr'
           'desktop-file-utils')

  makedepends=('autoconf'
               'ncurses'
               'bison'
               'perl'
               'fontforge'
               'flex'
               'prelink'
               'gcc-multilib>=4.5.0-2'
               'lib32-giflib'
               'lib32-libpng'
               'lib32-gnutls'
               'lib32-libxinerama'
               'lib32-libxcomposite'
               'lib32-libxmu'
               'lib32-libxxf86vm'
               'lib32-libxml2'
               'lib32-libldap'
               'lib32-lcms'
               'lib32-mpg123'
               'lib32-openal'
               'lib32-v4l-utils'
               'lib32-alsa-lib'
               'lib32-mesa'
               'lib32-libgl'
               'lib32-libcl'
               'attr'
               'samba'
               'pulseaudio'
               'opencl-headers')

  optdepends=('lib32-giflib'
              'lib32-libpng'
              'lib32-libldap'
              'lib32-gnutls'
              'lib32-lcms'
              'lib32-libxml2'
              'lib32-mpg123'
              'lib32-openal'
              'lib32-v4l-utils'
              'lib32-libpulse'
              'lib32-alsa-plugins'
              'lib32-alsa-lib'
              'lib32-libjpeg-turbo'
              'lib32-libxcomposite'
              'lib32-ncurses'
              'lib32-libcl'
              'oss'
              'cups'
              'samba'
              'lib32-libtxc_dxtn')
fi

makedepends+=('git')

_upname="wine-${pkgver}"

# Uncomment the line below if you want wine installed to /opt/wine-silverlight instead of replacing wine
#customprefix=1

if [[ $customprefix != 1 ]]; then
  _prefix="/usr"
  _sysconf="/etc"
  conflicts=('wine' 'wine-compholio')
  provides=('wine=${pkgver}' 'wine-compholio=${pkgver}')
else
  # change _prefix if you don't want to use the default custom prefix of /opt/wine-silverlight
  _prefix="/opt/${pkgname}"
  _sysconf="${_prefix}/etc"
  conflicts=('wine-compholio')
  provides=('wine-compholio=${pkgver}')
fi

prepare() {
  pushd "${srcdir}"

  # Get rid of old build dirs
  rm -rf ${pkgname}-{32,64}-build
  mkdir ${pkgname}-32-build

  if [[ $CARCH == x86_64 ]]; then
    mkdir ${pkgname}-64-build
  fi

  # These additional CPPFLAGS solve FS#27662 and FS#34195
  export CFLAGS="$CFLAGS -DHAVE_ATTR_XATTR_H=1"
  export CPPFLAGS="${CPPFLAGS/-D_FORTIFY_SOURCE=2/} -D_FORTIFY_SOURCE=0"

  pushd "${srcdir}"/"${_upname}"

  # use upstream Makefile to apply patches and to call make_requests / autoreconf
  # make -C "$srcdir/wine-staging-${pkgver}/patches/" DESTDIR="$srcdir/${_upname}" install
  # patchinstall.sh is the updated way to call above command
  "${srcdir}/wine-staging-${pkgver}/patches/patchinstall.sh" DESTDIR="${srcdir}/${_upname}" --all --no-autoconf

  # Apply d3d9 support patches if enabled.
  if [[ $enable_d3d9_support != 0 ]]; then
    patch -p1 < "${srcdir}/wine-d3d9-patches-wine-d3d9-${pkgver}/wine-d3d9.patch"
  fi

  # Picked up changed configuration after patching
  autoreconf -f
  tools/make_requests

  popd
  popd
}

build() {

  if [[ $enable_d3d9_support != 0 ]]; then
    _d3d9_config="--with-d3dadapter"
  else
    _d3d9_config=""
  fi

  pushd "${srcdir}"

  if [[ $CARCH == x86_64 ]]; then

    msg2 "Building Wine-64..."
    pushd "${srcdir}/${pkgname}-64-build"
    ../$_upname/configure \
      --prefix="${_prefix}" \
      --sysconfdir="${_sysconf}" \
      --libdir="${_prefix}/lib" \
      --with-x \
      --with-xattr \
      --without-gstreamer \
      --enable-win64 \
      --disable-tests \
      ${_d3d9_config}
    # Gstreamer was disabled for FS#33655
    make

    _wine32opts=(
      --libdir=/${_prefix}/lib32
      --with-wine64="${srcdir}/${pkgname}-64-build"
    )

    export PKG_CONFIG_PATH="${_prefix}/lib32/pkgconfig"
    popd
  fi

  msg2 "Building Wine-32..."
  pushd "${srcdir}/${pkgname}-32-build"
  ../$_upname/configure \
    --prefix="${_prefix}" \
    --sysconfdir="${_sysconf}" \
    --with-x \
    --with-xattr \
    --without-gstreamer \
    --disable-tests \
    "${_wine32opts[@]}" \
    ${_d3d9_config}

  # These additional flags solve FS#23277
  make CFLAGS+="-mstackrealign -mincoming-stack-boundary=2" CXXFLAGS+="-mstackrealign -mincoming-stack-boundary=2"
  popd
  popd
}

package() {
  msg2 "Packaging Wine-32..."
  cd "${srcdir}/${pkgname}-32-build"

  if [[ $CARCH == i686 ]]; then
    make prefix="${pkgdir}/${_prefix}" install
  else
    make prefix="${pkgdir}/${_prefix}" \
      libdir="${pkgdir}/${_prefix}/lib32" \
      dlldir="${pkgdir}/${_prefix}/lib32/wine" install

    msg2 "Packaging Wine-64..."
    cd "${srcdir}/${pkgname}-64-build"
    make prefix="${pkgdir}/${_prefix}" \
      libdir="${pkgdir}/${_prefix}/lib" \
      dlldir="${pkgdir}/${_prefix}/lib/wine" install
  fi

  # Font aliasing settings for Win32 applications
if [[ $customprefix != 1 ]]; then
  install -d "${pkgdir}"/etc/fonts/conf.{avail,d}
  install -m644 "${srcdir}/30-win32-aliases.conf" "${pkgdir}/etc/fonts/conf.avail"
  ln -s ../conf.avail/30-win32-aliases.conf "${pkgdir}/etc/fonts/conf.d/30-win32-aliases.conf"
fi

  # Provide symlinks in /opt/wine-compholio/bin

  if [[ "${_prefix}" != "/opt/wine-compholio" ]]; then
    mkdir -p "${pkgdir}/opt/wine-compholio/bin"
    for _file in $(ls "${pkgdir}/${_prefix}/bin"); do \
      ln -s "${_prefix}/bin/$_file" "${pkgdir}/opt/wine-compholio/bin/$_file"; \
    done
  fi

}
# vim:set ts=2 sw=2 tw=0 et:
