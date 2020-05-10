# QtRaspberryPi4

wget http://download.qt.io/official_releases/qt/5.12/5.12.7/single/qt-everywhere-src-5.12.7.tar.xz
md5sum qt-everywhere-src-5.12.7.tar.xz
tar xf qt-everywhere-src-5.12.7.tar.xz
git clone https://github.com/oniongarlic/qt-raspberrypi-configuration.git

cd qt-raspberrypi-configuration && make install DESTDIR=../qt-everywhere-src-5.12.7

sudo apt-get update

sudo apt-get install build-essential libfontconfig1-dev libdbus-1-dev libfreetype6-dev libicu-dev libinput-dev libxkbcommon-dev libsqlite3-dev libssl-dev libpng-dev libjpeg-dev libglib2.0-dev libraspberrypi-dev
sudo apt-get install mesa-utils libegl1-mesa libegl1-mesa-dev libgbm-dev libgbm1 libgl1-mesa-dev libgl1-mesa-dri libgl1-mesa-glx libglu1-mesa libglu1-mesa-dev 


sudo apt-get install libxfixes-dev

cd qt-everywhere-src-5.12.7

PKG_CONFIG_LIBDIR=/usr/lib/arm-linux-gnueabihf/pkgconfig:/usr/share/pkgconfig CFLAGS="-march=armv8-a -mtune=cortex-a72 -mfpu=crypto-neon-fp-armv8" CXXFLAGS="-march=armv8-a -mtune=cortex-a72 -mfpu=crypto-neon-fp-armv8" ../qt-everywhere-src-5.12.7/configure -kms -platform linux-rpi4-v3d-g++ -v -opengl es2 -eglfs -no-gtk -opensource -confirm-license -release -reduce-exports -force-pkg-config -nomake examples -no-compile-examples -skip qtwayland -skip qtwebengine -no-feature-geoservices_mapboxgl -qt-pcre -no-pch -ssl -evdev -system-freetype -fontconfig -glib -prefix /opt/Qt5.12 -qpa eglfs

sudo make -j2

WORK=$PWD
cd ./qtlocation/src/3rdparty/clip2tri/
sudo make
cd ./qtlocation/src/3rdparty/clipper/
sudo make
cd ./qtlocation/src/3rdparty/poly2tri/
sudo make
cd $WORK
sudo make install
cd ./qtvirtualkeyboard/src/
sudo apt-get install libxcb1-dev
sudo apt-get install libxcb-keysyms1-dev libxcb-image0-dev libxcb-shm0-dev libxcb-icccm4-dev libxcb-sync0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-randr0-dev libxcb-render-util0-dev

make
sudo nano ../qt-everywhere-src-5.12.7/qtscript/src/3rdparty/javascriptcore/JavaScriptCore/wtf/Platform.h

#define WTF_CPU_ARM_TRADITIONAL 1
#define WTF_CPU_ARM_THUMB2 0
/* CPU(ARM_TRADITIONAL) - Thumb2 is not available, only traditional ARM (v4 or greater) */
/* CPU(ARM_THUMB2) - Thumb2 instruction set is available */
/* Only one of these will be defined. */
/* #if !defined(WTF_CPU_ARM_TRADITIONAL) && !defined(WTF_CPU_ARM_THUMB2) */
/* #  if defined(thumb2) || defined(__thumb2__) \ */
/*    || ((defined(__thumb) || defined(__thumb__)) && WTF_THUMB_ARCH_VERSION == 4) */
/* #    define WTF_CPU_ARM_TRADITIONAL 0 */
/* #    define WTF_CPU_ARM_THUMB2 1 */
/* #  elif WTF_ARM_ARCH_AT_LEAST(4) */
/* #    define WTF_CPU_ARM_TRADITIONAL 1 */
/* #    define WTF_CPU_ARM_THUMB2 0 */
/* #  else */
/* #    error "Not supported ARM architecture" */
/* #  endif
/* #elif CPU(ARM_TRADITIONAL) && CPU(ARM_THUMB2) Sanity Check */
/* #  error "Cannot use both of WTF_CPU_ARM_TRADITIONAL and WTF_CPU_ARM_THUMB2 platforms" */
/* #endif !defined(WTF_CPU_ARM_TRADITIONAL) && !defined(WTF_CPU_ARM_THUMB2) */

cd ../qt-everywhere-src-5.12.7/qtscript/src/3rdparty/javascriptcore/JavaScriptCore
sudo make
cd $WORK
sudo make install

/opt/Qt5.12/bin/qmake --version

Install QT Creator
wget http://download.qt.io/official_releases/qtcreator/4.9/4.9.0/qt-creator-opensource-src-4.9.0.tar.xz
tar xf qt-creator-opensource-src-4.9.0.tar.xz qt-creator-opensource-src-4.9.0/
cd qt-creator-opensource-src-4.9.0/
/opt/Qt5.12/bin/qmake
make
sudo make install INSTALL_ROOT=/opt/QtCreator4.9
/opt/QtCreator4.9/bin/qtcreator
