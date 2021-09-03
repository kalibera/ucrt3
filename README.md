# Github actions with experimental UCRT/UTF-8 builds of R

This is a demonstration of the use of github actions for checking packages
on Windows using an experimental build of R using UCRT as the C runtime and
UTF-8 as the native encoding.  More information about the experimental
builds is available in this [blog
post](https://developer.r-project.org/Blog/public/2021/03/12/windows/utf-8-toolchain-and-cran-package-checks),
[this howto](https://svn.r-project.org/R-dev-web/trunk/WindowsBuilds/winutf8/ucrt3/howto.html)
and the builds and package checks are available from
[here](https://www.r-project.org/nosvn/winutf8/ucrt3/).

[r-install](actions/r-install/action.yml) installs the experimental build of R.

See [codetools](https://github.com/kalibera/codetools) for an example how to
use it.  The [codetools](https://cran.r-project.org/web/packages/codetools)
package does not include native code, so it does not have to install the
toolchain.

[toolchain-install](actions/toolchain-install/action.yml) installs the
toolchain. One may choose the "full" version or just "base", which only has
libraries needed to build R and recommended packages, so it is smaller in
size.

See [pkg-check-test (tiff)](https://github.com/kalibera/pkg-check-test/) for
an example how to use it.  This example builds the
[tiff](https://cran.r-project.org/web/packages/tiff) package and is based on
Simon Urbanek's [pkg-check-test](https://github.com/s-u/pkg-check-test/).
It depends on two other packages,
[png](https://cran.r-project.org/web/packages/png) and
[jpeg](https://cran.r-project.org/web/packages/jpeg) and requires at least
the base toolchain to build its native code.

Both actions use an action for checking R packages by Simon Urbanek,
currently in a slightly modified version
[R-actions](https://github.com/kalibera/R-actions).
