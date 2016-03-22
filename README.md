Berg CMS
========

Berg Content Management System is a web based CMS for publishing a regular
printed news letter, e.g. a bimonthly parish newsletter. It is suitable
for small editorial committees.

The focus of the system is the next issue to publish. An issue consists
of several articles. These articles are preprocessed and then composed
by LaTeX. The outcome is a PDF file that can be downloaded and then
sent directly to the print shop.


Building from Source
====================

[![Build Status](https://travis-ci.org/leutloff/bergcms.png)](https://travis-ci.org/leutloff/bergcms)

The most up-to-date documentation will be the Continuous Integration build
on Travis CI. The file .travis.yml performs the build of the C++ based
modules, runs C++ and Perl based unit tests, installs everything on a
local Apache web server and will perform some GUI tests. Nevertheless these
are the steps to perform a build:

- Clone or download the repository.
- Execute the script update_and_build_submodules.sh to initialize, update and
  build the submodules.
- Install the [Boost C++ library](http://boost.org). Download the Boost C++ Library from
  [http://www.boost.org/users/download/](http://www.boost.org/users/download/)
  (start with version 1.58, when unsure). Then extract and build boost library.
  Execute the following commands in the extracted source:
      ./bootstrap.sh --with-libraries=atomic,chrono,date_time,exception,filesystem,iostreams,log,program_options,regex,signals,system,test,thread
      sudo ./b2 -j 4 link=shared runtime-link=shared install -d0 --prefix=/usr/local
- To test the build environment:
  Launch CMake GUI and point the source code the src directory and
  build the project. Alternatively the build can done using these commands:
      rm -rf build-local
      mkdir build-local && pushd build-local && cmake -D CMAKE_BUILD_TYPE=Release ../src
      make
      popd
- Run the script make_zip.sh. This will build all applications and put all
  the required files in a single ZIP file.
- Install the content of the ZIP file on the Web Server. An outline of the
  Apache configuration is shown in doc/etc_apache2. Use the script
  install_locally.sh to perform this step together with some tweaks with
  regard the file permissions. The used install path can be adapted to
  local differences by overriding shell variables in a file named
  install_locally.cfg.


Building with Visual Studio 2015
================================

Build Boost C++ Library
-----------------------

Build Boost from source http://www.boost.org/users/download/. Just follow the
"Getting Started"
(http://www.boost.org/doc/libs/1_60_0/more/getting_started/windows.html)
and follow section 5.1 "Simplified Build From Source". Execute the following
commands in the unpacked source directory:
    bootstrap.bat
    .\b2 --with-atomic --with-chrono --with-date_time  --with-exception --with-filesystem --with-iostreams --with-log --with-program_options --with-regex --with-signals --with-system --with-test --with-thread link=shared

When using the prebuilt binaries from
https://sourceforge.net/projects/boost/files/boost-binaries/ it is required
to add a symbolic link.
Create a symbolic link from lib to lib32-msvc-14.0 in the unpacked
directory. Start a command window as Administrator, go to directory and
enter `MKLINK /D lib lib32-msvc-14.0`.

Install CMake
-------------
Install CMake 3.5.0 or newer and launch the CMake GUI. Enter the Berg
CMS source directory `bergcms\src`. Add an entry `BOOST_ROOT`
pointing to the above installed directory and an entry setting
`USE_CTEMPLATE` to FALSE. Click on Configure and
Generate.

Build and execute the unit tests
--------------------------------

Open the generated Solution with Visual Studio and build it. It
is expected that the unit tests (bergunittests) are running successfully
on Windows, too.

Adaptation to a windows web server would be the next step to make it run on
Windows.


Installation/Deployment
=======================

The installation means copying the content of the archive to the web server
executing the web applications. This can be accomplished in different ways depending
on the requirements of the web server. The archive is build during the build
process as described above.

Part of the archive content is a deployment script. The script deploy.sh copies
the surrounding files to a web server using ncftpput. So the version deployed
to web server depends on the path where the deploy.sh script is executed.

The servers are configured in a file named remotehosts.cfg. The supported
parameters should be copied from the beginning of the deploy.sh script. Here are
some examples calling the deploy.sh script.

Showing the available options and components:
    ./deploy.sh -h

The script supports the deployment to two different servers. One is called test
and the other one is the production server. Deploying the static HTML files to
the test server can be done using this command:
    ./deploy.sh -t test -c html

Copying everything to the production server:
    ./deploy.sh -t prod -c all


GUI Tests and Unit Tests
========================

The test suite for the user interface is based on the Selenium IDE.
Get the Firefox plug-in Selenium IDE
from [http://seleniumhq.org/download/](http://seleniumhq.org/download/).
Installation is described at
[http://seleniumhq.org/docs/02_selenium_ide.html](http://seleniumhq.org/docs/02_selenium_ide.html).

The test suites and test cases are located in the folder testsuite.
Put the file www/cgi-bin/brg/testcase.pl only on testing instances.
The CGI script will copy a database according the selected test case.
This will destroy the existing database. The existence of the testcase.pl
file allows the test cases to modify the database content.

Additional to the GUI test the different parts of the system are tested with
different types of unit tests. You will find unit tests written in C++ and
Perl. New unit tests should be written in C++ when feasible.

The unit tests for the C++-Code are located in src/test. Execute the project
named test to execute the C++ test cases. These test cases are based on
boost unit test.

The Perl related unit tests are located in www/cgi-bin/brg/t. Use the script
www/cgi-bin/brg/t/run_tests.sh to execute all the Perl based tests.


History and License
===================

The system was originally written by Heiko Decker using Perl. Work started
in 2006 and is used for production of a bimonthly parish newsletter since 2009.
Since 2011 the actual maintainer is Christian Leutloff. New modules are
written in C++.

License is [AGPL](https://www.gnu.org/licenses/agpl-3.0) for C++ Code and
[Artistic](http://www.perlfoundation.org/artistic_license_2_0)/[GPL](https://www.gnu.org/licenses/gpl-3.0)
for the Perl Code.
LaTeX related files are licensed using the [LaTeX Project Public License](http://www.latex-project.org/lppl/lppl-1-3c.html).
Javascript Code can be used under the terms of the [MIT License](https://en.wikipedia.org/wiki/MIT_License).

Each file will state explicitly the license it belongs to.
If this is not the case this is considered a bug.
Copies of the Licenses are found in the LICENSES folder.


Coding Guidelines
=================

- Indentation must use spaces only. Do not use tabs for this purpose.
- Character encoding must be UTF-8 in every place.
- Line ending should be Unix style (LF only).
- Use tool based formatting where available.


Feedback
========

Feedback is welcome. Patches and pull requests are even more welcome.
