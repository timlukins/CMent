# CMent

![cment logo](https://github.com/timlukins/CMent/raw/master/cment_logo.png "CMent Logo")

_An absurdly simple dependency manager for CMake._

**(15th Jan, 2017 - Currently work in progress - a MVP of sorts - just added here to see if useful to others...)**

Resolves and builds dependencies for a CMake based project...where all dependencies are in turn CMake built projects released on Github!

It is **not** a curated and centralised dependency manager. For that, you might want to look at the excellent [Hunter](https://github.com/ruslo/hunter).

Instead, `cment` is a _de-centralised_ and _developer driven_ tool. (So, a bit more like [Carthage](https://github.com/Carthage/Carthage) is to [CocoaPods](https://github.com/CocoaPods/CocoaPods)...)

Underneath, it leverages the use of `ExternalProject` with a few clever tricks and checks to basically simplify and reduce error. T

As such, it aims to be lightweight and to fulfill a number of common use-cases:

* The need to simply resolve local (i.e. non-system wide installed) libraries for build.
* The need to build and link to a isolated debug version of a library cleanly.
* The need to check and enforce the build order of depedencies easily.
* The need to include a specific version of a library (or development branch HEAD).
* The need (probably above all else) to dictate and trace components of a release exactly.

### Usage

CMent is itself build with CMent! 

The target and its dependencies are specified in a simple file based on a [Ordered Graph Data Language](http://ogdl.org) format. 

Here's its own `.cment` file...

    cment 1.0.0
        timlukins/ogdl-c == v1.0.1 "ogdl"
        timlukins/argparse

Fairly obviously, the target itself (and version) are at the top.

Then each dependency repository (*which must also be CMake based and hosted on github*) is included along with a specifier for the release tag to use.

Dependencies are **indented** to indicate respective sub-dependencies.

For a specific dependency the name of resulting artifact can also be quoted (e.g. `"ogdl"`) - which is useful if it differs from the name of the repository.

If you ommit the release tag it will instead aim to pull down the latest developments on the `HEAD` of the `master` branch.
  
Run `cment" on this file with:

    cment [file.cment]

And this will generate a `CMent.cmake` file.

Finally, simply include this in your `CMakeLists.txt` file like so (after any `add_executable` or `add_library`):

```
include(CMent.cmake)
```

That's it.

### Caveats

If you go and look at the generated file you'll see that it has handily expanded and organised the commands for downloading and adding the libraries to the build path (based on the cunning use of `ExternalProject`).

Now when you run `cmake` the dependencies should be resolved and downloaded into a separate `dep` folder as part of the build and stored there as a cache (you'll need to delete this if you ever want to build from scratch).

If the `CMakeLists.txt` includes any other `FindPackage` or `pkg_check_modules` like commands, these can still apply - but include the `CMake.cment` file before them, so that the local downloaded versions in the `dep` cache are used.

If you want to build a debug version, then the same flags should automatically be passed down to the dependencies.

Building and linking archive libraries is fine - but if the dependencies are built as shared you will have to update your `LD_LIBRARY_PATH` to point at the `./deps/lib/` folder first (i.e. before system wide libraries).

Obviously, if you change the project (i.e. need a new version of a library) you must update the `.cment` file and regenerate the `CMent.cmake` file from it.

For bugs and other proposed enhancements - just use the [link](https://github.com/timlukins/CMent/issues).

**_"In the hope that it eases and improves the development of your software."_**







