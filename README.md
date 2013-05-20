# Tufão - an asynchronous web framework for C++ built on top of Qt

Tufão is a web framework for C++ that makes use of Qt's object communication
system (signals & slots). It features:
  * High performance standalone server
  * Cross-plataform support
  * Good [documentation](http://vinipsmaker.github.com/tufao/ref/)
  * Support modern HTTP features
   * Persistent streams
   * Chunked entities
   * 100-continue status
   * WebSocket
  * HTTPS support
  * Flexible request router
  * Static file server with support for conditional requests, partial download
    and automatic mime detection
  * Plugin-based server to allow change the running code without restart the
    application
  * Flexible and secure session support
  * [QtCreator's plugin](https://github.com/vinipsmaker/tufao-qtcreatorplugin) to allow create new applications rapidly
  * Timeout support
  * C++11

It uses Ryan Dahl's HTTP parser to provide good performance.

You can generate documentation from the source code using Doxygen. The
documentation will be put on the doc folder in the html and tex formats. There
is also experimental support for Qt compressed help files.

## LICENSE

The library is under the LGPLv2 and public header files, documentation and
examples are under MIT license.

The Tufão logo is licensed under [Creative Commons Attribution 3.0 Unported]
(http://creativecommons.org/licenses/by/3.0/).

The library is dynamic linked against Qt and include code from Ryan Dahl's HTTP
parser. Qt library is licensed under LGPL and Ryan Dahl's HTTP parser is
licensed under MIT license.

So, you can create commercial applications (the *only* restriction is that *if*
you do any modifications to *Tufão*, these modifications must be redistributed).

## BUILD

Make sure you have Qt and CMake installed and with the PATH to its executables
set, then create a folder for the build and, from there, run:

    $ cmake OPTIONS path_to_source_dir
    $ make MAKEOPTIONS

OPTIONS can be null or have a combination of the following values:
  * -DCMAKE_INSTALL_PREFIX=${INSTALLDIR} sets the directory where to install
    Tufão.
  * -DCMAKE_BUILD_TYPE=Debug
  * -DCMAKE_BUILD_TYPE=Release
  * -DCMAKE_BUILD_TYPE=RelWithDebInfo
  * -DCMAKE_BUILD_TYPE=MinSizeRel
  * -DGENERATE_DOC=YES generate documentation using Doxygen

OPTIONS available on Windows:
  * -G"MinGW Makefiles" to generate Makefiles for use with MinGW environment
  * -G"Visual Studio 10" to generate project files for Visual Studio 10

MAKEOPTIONS can be null or have a combination of the following values:
  * install installs Tufão
  * DESTDIR=${PKGDIR} sets the directory where to install Tufão. This options
    should be used if you intend to package Tufão to set the package directory.
    To choose another installation directory, see options in OPTIONS, above.

Example:

    $ cmake -DCMAKE_INSTALL_PREFIX=/usr
    $ make DESTDIR=pkg install

NOTE:
Qt 5.0 or later is required for 1.x series. Qt 4.7 or later is required to 0.x
series.

NOTE:
If you intend to create a CPack-based installer, just run:

    # To create a binary distribution:
    cpack -C CPackConfig.cmake

    # To create a source distribution:
    cpack -C CPackSourceConfig.cmake

    # To create a Windows NSIS-based installer:
    cpack -GNSIS

### Documentation

To generate the documentation, just run doxygen using Doxyfile as configuration
file and the documentation will be generated in the folder doc. The
documentation is available in the following formats:
  * HTML: Always generated. It should be in the doc/html folder.
  * latex: You can use this format to generate a pdf. Just run make inside the
    doc/latex folder.
  * Qt Compressed Help file: If you have qhelpgenerator binary tool in the
    system PATH, then the file is generated when you run doxygen and should be
    in doc/qch/tufao.qch. If you have qhelpgenerator tool installed, but not
    configured in the system PATH, but still want to generate the documentation
    in this format, just run the tool using doc/html/index.qhp as input file.

### Tests

Tufão also have a lot of code to test its correctness. These codes are based on
QTestLib and generate a self-contained executable. There is also some CTest
rules and integration with the CMake build. To run a test, just execute:

    make test

Or, if you don't want to use Makefiles:

    ctest

## INSTALL

The default install prefix is /usr/local, if you don't intend to change it, just
run, after build:

    # make install

To install to a different prefix, run:

    $ cmake -DCMAKE_INSTALL_PREFIX=${DESTDIR}
    $ make install

## USAGE

To use Tufão in your Qt projects, just edit your .pro file and add the line:

    CONFIG += TUFAO1

You can find some examples in the examples folder.

Tufão has PKG-CONFIG support also. Its module name is 'tufao1'.

If you want use Tufão in other build system, just add the compiler option
-ltufao1.

You can also see Tufão documentation integrated in QtAssistant.

## NEWS

Version 1.0
  * The project finally have a logo (made by me in Inkscape)
  * Deprecated API was removed
  * Url and QueryString removed in favor of QUrl
  * Headers refactored to inherit from QMultiHash instead of QMultiMap
  * HttpServerResponse
   * Constructor's options argument is optional now
   * setOptions method added
   * Constructor takes a reference to a QIODevice instead a pointer
  * HttpServerRequest
   * Constructor takes a reference to a QAbstractSocket instead a pointer
   * socket method returns a reference instead a pointer
   * url returns a QUrl
   * data signal was changed and you must use readBody method to access body's content.
   * the upgrade's _head_ data is accessed from the request body from now on
   * now the object auto-disconnects slots from data and end signals right before emit ready
   * setCustomData and customData methods added
    * Now HttpServerRequestRouter use these methods to pass the list of captured
      texts
  * HttpServer uses reference instead of pointers in several places
  * AbstractHttpServerRequestRouter refactored to explore lambdas features.
  * Tufão's plugin system fully refactored
   * It's using JSON files as configuration
  * AbstractHttpServerRequestHandler::handleRequest
   * It uses references instead pointers
   * It receives 2 arguments instead of 3
  * One more abstraction to sessions created to explore lambdas
  * WebSocket
   * startServerHandshake is taking references instead pointers
  * LESS POINTERS and MORE REFERENCES
   * This change exposes a model more predictive and natural
   * I'm caring less about Qt style and more about C++ style
    * But don't worry, I'll maintain a balance
  * Using scoped enums
  * HttpFileServer uses/sends mime info
  * Interfaces don't inherit from QObject anymore, so you can use multiple
    inheritance to make the same class implement many interfaces
  * HttpUpgradeRouter introduced
   * HttpServer::setUpgradeHandler also
  * Updated QtCreator plugin to work with QtCreator 2.7.0 and Qt 5

Version 0.6:
  * HttpServerRequest
   * setUrl added
  * UrlRewriterHandler added
  * HttpUpgradeRouter added
  * headers can be "streamed" to QDebug objects

Version 0.5:
  * WebSocket
   * peerAddress() method added
   * peerSocket() method added

Version 0.4:
  * Using CMake build system
   * Changes to allow parallel Tufão installations (if major versions differs)
   * Added PKGCONFIG support
  * Session support
  * Better documentation

Version 0.3
  * More application templates in QtCreator Tufão's plugin
  * Class to serve static files with support for conditional requests and
    byte-range requests
  * Robust request router added
  * HTTP plugin server added
  * Using newer version of Ryan Dahl's HTTP parser
  * Changed license from public headers to MIT
  * MSVC support
  * Some minor improvements

Version 0.2:
  * Code is more stable
  * Documentation improved
  * HttpServerResponse is easier to use
  * WebSocket support
  * TUFAO_VERSION_MAJOR and TUFAO_VERSION_MINOR macros added
  * QtCreator plugin
  * New examples

## ROADMAP

Here is a small roadmap for Tufão:

1.1:
  * RPC support
  * Service discovery and description support

1.2:
  * Forms and file uploads
