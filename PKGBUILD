
pkgdesc="ROS - Persistent storage of ROS data using MongoDB."
url='http://www.ros.org/'

pkgname='ros-hydro-warehouse-ros'
pkgver='0.8.6'
_pkgver_patch=0
arch=('i686' 'x86_64')
pkgrel=5
license=('BSD')
makedepends=('cmake' 'ros-build-tools')

ros_depends=(ros-hydro-rospy
  ros-hydro-roscpp
  ros-hydro-geometry-msgs
  ros-hydro-rostime
  ros-hydro-std-msgs
  ros-hydro-rostest)
depends=(${ros_depends[@]}
  mongodb
  mongo-cxx-driver-legacy-0.0-26compat
  curl)

_tag=release/hydro/warehouse_ros/${pkgver}-${_pkgver_patch}
_dir=warehouse_ros
source=("${_dir}"::"git+https://github.com/ros-gbp/warehouse-release.git"#tag=${_tag}
        'depends.patch')
md5sums=('SKIP'
         '3b526c61c1e6a86df8252d4af7339797')

build() {
  # Use ROS environment variables
  /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/hydro/setup.bash ] && source /opt/ros/hydro/setup.bash

  # Apply patch
  msg "Patching source code"
  cd ${srcdir}/${_dir}
  git apply ${srcdir}/depends.patch

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/hydro \
        -DPYTHON_EXECUTABLE=/usr/bin/python2 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
        -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
