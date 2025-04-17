# System Dependencies for Ubuntu 22.04

Below is a list of all the external libraries & system tweaks needed to build and run **ORB_SLAM2** on Ubuntu 22.04 (GCC 11+, CMake 3.16+).

---

## 1. C++ Toolchain

- **GCC ≥ 9** (Tested with GCC 11.4)
- **CMake ≥ 3.16**

Install via:
```bash
sudo apt install build-essential cmake
```

---

## 2. Eigen3

Install Eigen3 via:
```bash
sudo apt install libeigen3-dev
```

(No special version modifications required)

---

## 3. OpenCV

ORB_SLAM2 explicitly requires OpenCV 3.x (OpenCV 4.x will **not** work).

Either install from the official repositories:
```bash
sudo apt install libopencv-dev=3.2.*
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

Install required dependencies first:
```bash
sudo apt install libglew-dev libglfw3-dev libboost-all-dev
```

Clone and build Pangolin from source:
```bash
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin && mkdir build && cd build
cmake ..
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

Ensure CMakeLists specifies C++14:
```cmake
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```