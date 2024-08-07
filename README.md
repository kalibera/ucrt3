# Github actions with UCRT/UTF-8 builds of R

This is a demonstration of the use of github actions for checking packages
on Windows using an experimental "ucrt3" build of R using a development version
of Rtools (since Rtools42). More information about the experimental
builds is available in this
[howto](https://svn.r-project.org/R-dev-web/trunk/WindowsBuilds/winutf8/ucrt3/howto.html).

[r-install](actions/r-install/action.yml) installs the "ucrt3" build of R.

See [codetools](https://github.com/kalibera/codetools) for an example how to
use it.  The [codetools](https://cran.r-project.org/web/packages/codetools)
package does not include native code, so it does not have to install the
toolchain.

[toolchain-install](actions/toolchain-install/action.yml) installs the
toolchain. One may choose the "full" version or just "base", which only has
libraries needed to build R and recommended packages, so it is smaller in
size.

See [pkg-check-test (tiff)](https://github.com/kalibera/pkg-check-test/) for
an example how to use it for checking a package.  This example builds the
[tiff](https://cran.r-project.org/web/packages/tiff) package and is based on
Simon Urbanek's [pkg-check-test](https://github.com/s-u/pkg-check-test/),
with a [patch](https://svn.r-project.org/R-dev-web/trunk/WindowsBuilds/winutf8/ucrt3/r_packages/patches/CRAN/tiff.diff)
applied to support the UCRT builds.
This package depends on two other packages,
[png](https://cran.r-project.org/web/packages/png) and
[jpeg](https://cran.r-project.org/web/packages/jpeg) and requires at least
the "base" toolchain to build its native code.

Both actions use an action for checking R packages by Simon Urbanek
(original version [here](https://github.com/s-u/R-actions)), currently in a
slightly modified version
[R-actions](https://github.com/kalibera/R-actions).

Please check release notes for each individual github release of the toolchain
and the included diff file. Some releases include R builds which automatically apply
[patches](https://www.r-project.org/nosvn/winutf8/ucrt3/patches/) to packages at
installation time. They also typically use binary builds of dependent packages
(patched the same way, of [CRAN](https://www.r-project.org/nosvn/winutf8/ucrt3/CRAN/bin/)  and
[Bioconductor](https://www.r-project.org/nosvn/winutf8/ucrt3/BIOC/bin/) packages). The check
action in [R-actions](https://github.com/kalibera/R-actions) now disable the patching
explicitly for the package being checked, while it is being checked, but the patching
still applies during installation of dependencies (and first round of installing the
package itself). So, checks for a package may provide surprising results when there
is a patch for the package (e.g. installation passes but then check fails, or there
are messages about issues applying a patch). On the other hand, the benefit is that
one may test changes to a package even when CRAN versions of dependent packages
have not yet been fixed.
