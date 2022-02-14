### G2O-cmake not found

   cp ./cmake_modules/FindG2O.cmake /usr/share/cmake-3.16/Modules

### About upgrade Cmake
Reference to [the answer1](https://askubuntu.com/questions/355565/how-do-i-install-the-latest-version-of-cmake-from-the-command-line) and [the answer2](https://answers.ros.org/question/293119/how-can-i-updateremove-cmake-without-partially-deleting-my-ros-distribution/)

```
version=3.22
build=1
## don't modify from here
mkdir ~/temp
cd ~/temp
wget https://cmake.org/files/v$version/cmake-$version.$build.tar.gz
tar -xzvf cmake-$version.$build.tar.gz
cd cmake-$version.$build/
```

Install the extracted source by running
```
./bootstrap
make -j$(nproc)
sudo make install
```
Env variable setup:
```
export PATH=$HOME/cmake-install/bin:$PATH
export CMAKE_PREFIX_PATH=$HOME/cmake-install:$CMAKE_PREFIX_PATH
```

### install different version of library
take libpcl as an example, see [details](https://www.fatalerrors.org/a/multi-version-coexistence-of-pcl-library.html)
