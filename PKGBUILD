pkgname=openmm
pkgver=7.4.2
pkgrel=1
pkgdesc="Toolkit for molecular simulation using high performance GPU code"
arch=('i686' 'x86_64')
url="http://openmm.org/"
license=('MIT' 'LGPL')
depends=('python' 'python-numpy' 'cython' 'fftw')
makedepends=('cmake' 'swig' 'doxygen')
source=("https://github.com/pandegroup/openmm/archive/${pkgver}.tar.gz")
sha1sums=('40dda055c4f8e254becb53c9c6f2e821a812a9bc')

#prepare() {
#  cd "${srcdir}"/openmm-${pkgver}
#  patch -p1 -i "${srcdir}"/01ae387d649c959812bb3d3bc451e54746b2ab59.patch
#  sed -i "s|sys.argv\[0\]|os.path.basename(sys.argv\[0\])|g" wrappers/python/src/swig_doxygen/swigInputBuilder.py 
#}

build() {
  cd "${srcdir}"
  mkdir -p build
  cd build
  cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DOPENMM_GENERATE_API_DOCS=ON \
    ../openmm-${pkgver}
  make
}
check () {
  msg2 "Testing openmm"
  cd "${srcdir}"/build
  make test
}


package() {
  cd "${srcdir}"/build
  msg2 "Installing openmm"
  make DESTDIR="${pkgdir}" install
  install -d "${pkgdir}"/usr/share/licenses/${pkgname}

  msg2 "Installing openmm python bindings"
  # Fix to install python wrapper
  sed -i 's:ENV{OPENMM_LIB_PATH} ":ENV{OPENMM_LIB_PATH} "$ENV{DESTDIR}:g' wrappers/python/pysetupinstall.cmake
  make DESTDIR="${pkgdir}" PythonInstall
  mv "${pkgdir}"/usr/licenses/*.txt "${pkgdir}"/usr/share/licenses/${pkgname}
  rm -rf "${pkgdir}"/usr/{bin,docs,examples,licenses}
}
