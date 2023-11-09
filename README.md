<!--

@license Apache-2.0

Copyright (c) 2020 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# Add-on Arguments

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> C API for validating, extracting, and transforming (to native C types) function arguments provided to a strided array Node-API add-on interface.

<!-- Section to include introductory text. Make sure to keep an empty line after the intro `section` element and another before the `/section` close. -->

<section class="intro">

</section>

<!-- /.intro -->

<!-- Package usage documentation. -->

<section class="installation">

## Installation

```bash
npm install @stdlib/strided-napi-addon-arguments
```

</section>

<section class="usage">

## Usage

```javascript
var headerDir = require( '@stdlib/strided-napi-addon-arguments' );
```

#### headerDir

Absolute file path for the directory containing header files for C APIs.

```javascript
var dir = headerDir;
// returns <string>
```

</section>

<!-- /.usage -->

<!-- Package usage notes. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="notes">

</section>

<!-- /.notes -->

<!-- Package usage examples. -->

<section class="examples">

## Examples

```javascript
var headerDir = require( '@stdlib/strided-napi-addon-arguments' );

console.log( headerDir );
// => <string>
```

</section>

<!-- /.examples -->

<!-- C interface documentation. -->

* * *

<section class="c">

## C APIs

<!-- Section to include introductory text. Make sure to keep an empty line after the intro `section` element and another before the `/section` close. -->

<section class="intro">

</section>

<!-- /.intro -->

<!-- C usage documentation. -->

<section class="usage">

### Usage

```c
#include "stdlib/strided/napi/addon_arguments.h"
```

<!-- lint disable maximum-heading-length -->

#### stdlib_strided_napi_addon_arguments( env, argv, nargs, nin, \*arrays\[], \*shape, \*strides, \*types, \*err )

Validates, extracts, and transforms (to native C types) function arguments provided to a strided array Node-API add-on interface.

```c
#include <node_api.h>
#include <stdint.h>
#include <assert.h>

// ...

/**
* Receives JavaScript callback invocation data.
*
* @param env    environment under which the function is invoked
* @param info   callback data
* @return       Node-API value
*/
napi_value addon( napi_env env, napi_callback_info info ) {
    napi_status status;

    // ...

    int64_t nargs = 10;
    int64_t nin = 2;

    // Get callback arguments:
    size_t argc = 10;
    napi_value argv[ 10 ];
    status = napi_get_cb_info( env, info, &argc, argv, NULL, NULL );
    assert( status == napi_ok );

    // ...

    // Initialize destination arrays:
    uint8_t *arrays[ 3 ];
    int64_t strides[ 3 ];
    int64_t shape[ 1 ];
    int32_t types[ 3 ];

    // Process the provided arguments:
    napi_value err;
    status = stdlib_strided_napi_addon_arguments( env, argv, nargs, nin, arrays, shape, strides, types, &err );
    assert( status == napi_ok );

    // ...

}

// ...
```

The function accepts the following arguments:

-   **env**: `[in] napi_env` environment under which the function is invoked.
-   **argv**: `[in] napi_value*` strided function arguments.
-   **nargs**: `[in] int64_t` total number of expected arguments.
-   **nin**: `[in] int64_t` number of input strided array arguments.
-   **arrays**: `[out] uint8_t**` destination array for storing pointers to both input and output strided byte arrays.
-   **shape**: `[out] int64_t*` destination array for storing the array shape (dimensions).
-   **strides**: `[out] int64_t*` destination array for storing array strides (in bytes) for each strided array.
-   **types**: `[out] int32_t*` destination array for storing strided array argument [data types][@stdlib/strided/dtypes].
-   **err**: `[out] napi_value*` pointer for storing a JavaScript error.

```c
napi_status stdlib_strided_napi_addon_arguments( const napi_env env, const napi_value *argv, const int64_t nargs, const int64_t nin, uint8_t *arrays[], int64_t *shape, int64_t *strides, int32_t *types, napi_value *err );
```

The function returns a `napi_status` status code indicating success or failure (returns `napi_ok` if success).

</section>

<!-- /.usage -->

<!-- C API usage notes. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="notes">

### Notes

-   The function assumes the following argument order:

    ```text
    [ N, id1, ia1, is1, id2, ia2, is2, ..., od1, oa1, os1, od2, oa2, os2, ... ]
    ```

    where

    -   `N` is the number of elements over which to iterate.
    -   `id#` is an input strided array [data type][@stdlib/strided/dtypes] (enumeration constant).
    -   `ia#` is an input strided array.
    -   `is#` is a corresponding input strided array stride (in units of elements).
    -   `od#` is an output strided array [data type][@stdlib/strided/dtypes] (enumeration constant).
    -   `oa#` is an output strided array.
    -   `os#` is a corresponding output strided array stride (in units of elements).

-   The function may return one of the following JavaScript errors:

    -   `TypeError`: first argument must be an integer.
    -   `TypeError`: input array [data type][@stdlib/strided/dtypes] argument must be an integer.
    -   `TypeError`: output array [data type][@stdlib/strided/dtypes] argument must be an integer.
    -   `TypeError`: input array stride argument must be an integer.
    -   `TypeError`: output array stride argument must be an integer.
    -   `TypeError`: input array argument must be a typed array.
    -   `TypeError`: output array argument must be a typed array.
    -   `RangeError`: input array argument must have sufficient elements based on the associated stride and the number of indexed elements.
    -   `RangeError`: output array argument must have sufficient elements based on the associated stride and the number of indexed elements.

</section>

<!-- /.notes -->

<!-- C API usage examples. -->

<section class="examples">

</section>

<!-- /.examples -->

</section>

<!-- /.c -->

<!-- Section to include cited references. If references are included, add a horizontal rule *before* the section. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="references">

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2023. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/strided-napi-addon-arguments.svg
[npm-url]: https://npmjs.org/package/@stdlib/strided-napi-addon-arguments

[test-image]: https://github.com/stdlib-js/strided-napi-addon-arguments/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/strided-napi-addon-arguments/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/strided-napi-addon-arguments/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/strided-napi-addon-arguments?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/strided-napi-addon-arguments.svg
[dependencies-url]: https://david-dm.org/stdlib-js/strided-napi-addon-arguments/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/strided-napi-addon-arguments/main/LICENSE

[@stdlib/strided/dtypes]: https://github.com/stdlib-js/strided-dtypes

</section>

<!-- /.links -->
