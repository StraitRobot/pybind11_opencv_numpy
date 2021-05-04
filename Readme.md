# pybind11_opencv_numpy

Binding between cv::Mat and np.array. And a small code example of how it work. The code work for `OpenCV 2.4`, `OpenCV 3+` and `OpenCV 4+`

The code in this repository create a simple binding, function in c++ are implemented in `example.cpp` file and the script that use them is `example.py`.

```bash
/project folder
├── build
├── example
│   ├── exemple.so                               # generate with cmake
│   └── example.cpython-36m-x86_64-linux-gnu.so  # generate with setup.py (the name will depends of the python version use)
├── CMakeLists.txt
├── setup.py
├── ndarray_converter.cpp  # Effective implementation of the binding
├── ndarray_converter.h    # Header for binding functions
└── ...
```

## Generation with cmake/make

*Note* : This method support opencv 2.4, opencv 3 and opencv 4.

We use [vcpkg](https://github.com/Microsoft/vcpkg) to install [pybind11](https://github.com/pybind/pybind11) library


```
./vcpkg install pybind11
```

### Compile

```bash
mkdir build
cd build
# configure make with vcpkg toolchain
cmake .. -DCMAKE_TOOLCHAIN_FILE=${VCPKG_DIR}/scripts/buildsystems/vcpkg.cmake
# on Windows : cmake.exe .. -DCMAKE_TOOLCHAIN_FILE="$Env:VCPKG_DIR/scripts/buildsystems/vcpkg.cmake"
# generate the example.so library
cmake --build . --config Release
# move example.so library in example folder
cmake --install . --config Release
```

#### Numpy header

In case of error like: `'numpy/ndarrayobject.h' : No such file or directory`

You have to be sure numpy is install on the computer either with `python -m pip install numpy` or `sudo apt-get install python-numpy` for linux.
Even if numpy is installed, CMake was not able to find correctly numpy header during configuration. Probably because numpy is install on the user package. Its possible the explicitly set the directory with the following command :

```bash
cmake .. -DCMAKE_TOOLCHAIN_FILE=${VCPKG_DIR}/scripts/buildsystems/vcpkg.cmake -DNUMPY_INCLUDE_DIR="${PYTHON_USER_DIR}/LocalCache/local-packages/Python39/site-packages/numpy/core/include/"
```

## Generation with setup.py


### install pybind11

```
python3 -m pip3 install pybind11
```

### Compile

#### OpenCV 2.4+, OpenCV 3+

```
python3 setup.py build
```

#### OpenCV 4

In OpenCV 4, there a extra folder level for headers (ex: `opencv4/opencv2/core/core.hpp`). To be able to compile with `setup.py` we need a extra command to indicate header location.

```
python3 setup.py build_ext --include-dirs "/usr/local/include/opencv4"
python3 setup.py build
```

### Install

```
python3 setup.py install
```

or

```
mv build/lib.linux-x86_64-3.5/example/example.cpython-36m-x86_64-linux-gnu.so example/example.cpython-36m-x86_64-linux-gnu.so
```
