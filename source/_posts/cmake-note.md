---
title: cmake_note
date: 2025-05-28 04:17:36
tags:
---

# Note of cmake quick start

## 1.Install cmake

```bash
sudo pacman -S cmake
```

## 2.Write your cpp program

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello cpp!" << std::endl;

    int a = 10;
    int b = a * 2;
    std::cout << "b is " << b << std::endl;

    return 0;
}
```

## 3.Write CMakeLists.txt in project directory

```cmake
cmake_minimum_required(VERSION 3.15)

# set the project name
project(Maintest)

# add the executable
add_executable(Maintest main.cpp)

```

## 4.Make directory `build` and into it to init cmake, then build the project.

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

Now, you can see all the build files in `build` directory, you can run program like this

```bash
./Maintest
```
