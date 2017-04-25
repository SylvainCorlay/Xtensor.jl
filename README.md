# ![xtensor-julia](http://quantstack.net/assets/images/xtensor-julia.svg)

[![Travis](https://travis-ci.org/QuantStack/xtensor-julia.svg?branch=master)](https://travis-ci.org/QuantStack/xtensor-julia)
[![Appveyor](https://ci.appveyor.com/api/projects/status/jpvvk2m35d1q8woq?svg=true)](https://ci.appveyor.com/project/QuantStack/xtensor-julia)
[![Documentation Status](http://readthedocs.org/projects/xtensor-julia/badge/?version=latest)](https://xtensor-julia.readthedocs.io/en/latest/?badge=latest)
[![Join the Gitter Chat](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/QuantStack/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Julia bindings for the [xtensor](https://github.com/QuantStack/xtensor) C++ multi-dimensional array library.

 - `xtensor` is a C++ library for multi-dimensional arrays enabling numpy-style broadcasting and lazy computing.
 - `xtensor-julia` enables inplace use of julia arrays in C++ with all the benefits from `xtensor`

     - C++ universal function and broadcasting 
     - STL - compliant APIs.
     - A broad coverage of numpy APIs (see [the numpy to xtensor cheat sheet](http://xtensor.readthedocs.io/en/latest/numpy.html)).

The Julia bindings for `xtensor` are based on the [CxxWrap.jl](https://github.com/JuliaInterop/CxxWrap.jl/) C++ library.

## Installation

```julia
Pkg.clone("https://github.com/QuantStack/xtensor-julia", "Xtensor");
Pkg.build("Xtensor")
```

## Usage

`xtensor-julia` offers a container type, `jltensor` wrapping Julia arrays inplace to provide an xtensor semantics

This container enables the numpy-style APIs of xtensor (see [the numpy to xtensor cheat sheet](http://xtensor.readthedocs.io/en/latest/numpy.html)).

### Example 1: Use an algorithm of the C++ standard library with Julia array.

**C++ code**

```cpp
#include <numeric>                        // Standard library import for std::accumulate
#include <cxx_wrap.hpp>                   // CxxWrap import to define Julia bindings
#include "xtensor-julia/jltensor.hpp"     // Import the jltensor container definition
#include "xtensor/xmath.hpp"              // xtensor import for the C++ universal functions

double sum_of_sines(xt::jltensor<double, 2>& m)
{
    auto sines = xt::sin(m);  // sines does not actually hold values.
    return std::accumulate(sines.begin(), sines.end(), 0.0);
}

JULIA_CPP_MODULE_BEGIN(registry)
    cxx_wrap::Module mod = registry.create_module("xtensor_julia_test");
    mod.method("sum_of_sines", sum_of_sines);
JULIA_CPP_MODULE_END
```

**Julia Code**

```julia
using xtensor_julia_test

arr = [[1.0 2.0]
       [3.0 4.0]]

s = sum_of_sines(arr)
s
```

**Outputs**

```
1.1350859243855171
```

## Running the C++ tests

From `deps/build`

```
cmake -D CxxWrap_DIR=/path/to/.julia/v0.5/CxxWrap/deps/usr/lib/cmake/ -D BUILD_TESTS=ON ..
```

## Dependencies on `xtensor` and `CxxWrap`

`xtensor-julia` depends on the `xtensor` and `CxxWrap` libraries

| `xtensor-jula`  | `xtensor` | `CxxWrap` |
|-----------------|-----------|-----------|
| master          |  ^0.9.0   | ^0.3.0    |

These dependencies are automatically resolved when using the Julia package manager.

## License

We use a shared copyright model that enables all contributors to maintain the
copyright on their contributions.

This software is licensed under the BSD-3-Clause license. See the [LICENSE](LICENSE) file for details.
