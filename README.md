# CMent

![cment logo](https://github.com/timlukins/CMent/raw/master/cment_logo.png "CMent Logo")

_An absurdly simple dependency manager for CMake._

*(Currently work in progress - a MVP of sorts - just added here to see if useful to others...)*

Resolves and builds dependencies for a CMake based project - where all dependencies are in turn CMake projects released on Github!

It is *NOT* a curated and centralised dependency manager.

For that, go and look at the excellent [Hunter](https://github.com/ruslo/hunter).

Instead it is _de-centralised_ and _developer driven_ utility.

(So a bit more like [Carthage](https://github.com/Carthage/Carthage) is to [CocoaPods](https://github.com/CocoaPods/CocoaPods) in the iOS world...)

As such, it aims to be lightweight and fulfill a number of CMake developer use-cases:

    * The need to have a local (i.e. non-system wide installed) library to hand for build.
    * The need to build and link a debug version of a library really easily and cleanly.
    * The need for a specific version of a library or libraries (or HEAD).
    * The need (above all) to ultimately dictate and trace components of a release more exactly.

It has effectively evolved by simply wrapping the _`ExternalProject`_ directive in CMake such as commonly known.

Example
========

CMent is itself build with CMent.

Here's its own '.cment' file...

    cment 1.0.0
        timlukins/ogdl-c == v1.0.1 "libogdl.a"
        timlukins/argparse == HEAD

Fairly obviously, the target itself (and version) is at the top, with each dependency repository (*each also CMake driven and hosted on github*) indented along with the release tag to use.

For a specific dependency the name of resulting artifact can be quoted (e.g. `"ogdl"`).

You can also specify `HEAD` instead of the release tag - that will pull down the latest developments on the `master` branch.

Run it with:

    > cment [file.cment]

This will generate a `CMent.cmake` file.

Simply include this in you `CMakeLists.txt` file like so (after any `add_executable`):

    include(CMent.cmake)

That's it.

If you go and look at this file you'll see that it has handily expanded and organised the commands for downloading and adding the libraries to the build path.

Now when you run `cmake` the dependencies should be resolved and downloaded into a separate 'dep' folder as pare of the build and stored there as a cache.

If the `CMakeLists.txt` includes any other `FindPackage` or `pkg_check_modules` like commands, these can still apply - but include the `CMake.cment` file before them.

For bugs and other proposed enhancements - just use the [link](https://github.com/timlukins/CMent/issues).

_"In the hope that it helps ease and improve your software."_.







