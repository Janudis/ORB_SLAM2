# System Dependencies for Ubuntu 22.04

Below is a list of all the external libraries & system tweaks needed to build and run **ORB_SLAM2** on Ubuntu 22.04 (GCC 11+, CMake 3.16+).

---

## 1. C++ Toolchain

- **GCC ≥ 9** (Tested with GCC 11.4.0)
- **CMake ≥ 3.16**

Install via:
```bash
sudo apt install build-essential cmake
```

---

## 2. Eigen3
Header‑only library.

Version: 3.4.0.

Install Eigen3 via:
```bash
sudo apt install libeigen3-dev
```

(No special version modifications required)

---

## 3. OpenCV

ORB_SLAM2 builds seamlessly against both 3.x and 4.x series.

 On this machine i have OpenCV 4.11.0 installed via apt:

Either install from the official repositories:
```bash
sudo apt install libopencv-dev        # provides OpenCV 4.11.0 on 22.04
```

Or build from source (recommended OpenCV 3.4.x):
```bash
git clone https://github.com/opencv/opencv.git -b 3.4
cd opencv && mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
```

Make sure `CMakeLists.txt` uses:
```cmake
find_package(OpenCV 3.0 REQUIRED)
```

---

## 4. Pangolin

You must build Pangolin from source with X11/GLX (no EGL):

```bash
sudo apt install libglew-dev libglfw3-dev libboost-all-dev
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin && mkdir build && cd build
cmake .. \
  -DCMAKE_BUILD_TYPE=Release \
  -DBUILD_SHARED_LIBS=ON      \
  -DPANGOLIN_ENABLE_EGL=OFF   \
  -DPANGOLIN_ENABLE_X11=ON    \
  -DPANGOLIN_ENABLE_GLX=ON
make -j$(nproc)
sudo make install
```

---

## 5. sigslot (for Pangolin’s signal_slot)

Pangolin's bundled `sigslot/signal.hpp` requires C++14:

Remove old headers:
```bash
sudo rm -rf /usr/local/include/sigslot
```

Clone sigslot v1.2 and install headers:
```bash
git clone https://github.com/palacaze/sigslot.git
sudo cp -r sigslot/include/sigslot /usr/local/include/
```