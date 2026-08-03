About pam-stubs-feedstock
=========================

Feedstock license: [BSD-3-Clause](https://github.com/conda-forge/pam-stubs-feedstock/blob/main/LICENSE.txt)

Home: https://github.com/linux-pam/linux-pam

Package license: BSD-3-Clause OR GPL-2.0-or-later

Summary: Link-time stub and headers for Linux-PAM (uses the system PAM at runtime)

pam-stubs provides the Linux-PAM public headers and a link-time-only stub
of libpam so that conda-forge packages (such as weston's VNC backend) can
compile and link against libpam.

conda-forge deliberately does not ship a runtime PAM: authentication is a
system concern driven by /etc/pam.d, and a conda libpam.so.0 placed on the
runtime search path would shadow the real one and break login/auth.

To square this, the stub is installed into $PREFIX/lib/stubs (used only at
link time via -L), never into $PREFIX/lib, following the same convention as
conda-forge's cuda-driver-dev package (which ships lib/stubs/libcuda.so and
leaves the real driver to the system). Downstream binaries record
"NEEDED libpam.so.0" with the correct versioned symbols; at runtime the
dynamic loader finds no libpam inside the conda prefix and falls through to
the user's system /lib*/libpam.so.0. The stub function bodies merely return
PAM_SYSTEM_ERR and are never executed.

Consumers link against the stub via pkg-config (pam.pc points its Libs at
the stubs directory, satisfying meson's dependency('pam')), or by adding
-L$PREFIX/lib/stubs to LDFLAGS — just as CUDA downstream feedstocks do
for -lcuda. As with cuda-driver-dev, no activation scripts are shipped.

Current build status
====================


<table><tr>
    <td>GitHub Actions</td>
    <td>
      <a href="https://github.com/conda-forge/pam-stubs-feedstock/actions/workflows/conda-build.yml">
        <img src="https://github.com/conda-forge/pam-stubs-feedstock/actions/workflows/conda-build.yml/badge.svg?event=push&branch=main">
      </a>
    </td>
  </tr>
</table>

Current release info
====================

| Name | Downloads | Version | Platforms |
| --- | --- | --- | --- |
| [![Conda Recipe](https://img.shields.io/badge/recipe-pam--stubs-green.svg)](https://anaconda.org/conda-forge/pam-stubs) | [![Conda Downloads](https://img.shields.io/conda/dn/conda-forge/pam-stubs.svg)](https://anaconda.org/conda-forge/pam-stubs) | [![Conda Version](https://img.shields.io/conda/vn/conda-forge/pam-stubs.svg)](https://anaconda.org/conda-forge/pam-stubs) | [![Conda Platforms](https://img.shields.io/conda/pn/conda-forge/pam-stubs.svg)](https://anaconda.org/conda-forge/pam-stubs) |

Installing pam-stubs
====================

Installing `pam-stubs` from the `conda-forge` channel can be achieved by adding `conda-forge` to your channels with:

```
conda config --add channels conda-forge
conda config --set channel_priority strict
```

Once the `conda-forge` channel has been enabled, `pam-stubs` can be installed with `conda`:

```
conda install pam-stubs
```

or with `mamba`:

```
mamba install pam-stubs
```

It is possible to list all of the versions of `pam-stubs` available on your platform with `conda`:

```
conda search pam-stubs --channel conda-forge
```

or with `mamba`:

```
mamba search pam-stubs --channel conda-forge
```

Alternatively, `mamba repoquery` may provide more information:

```
# Search all versions available on your platform:
mamba repoquery search pam-stubs --channel conda-forge

# List packages depending on `pam-stubs`:
mamba repoquery whoneeds pam-stubs --channel conda-forge

# List dependencies of `pam-stubs`:
mamba repoquery depends pam-stubs --channel conda-forge
```


About conda-forge
=================

[![Powered by
NumFOCUS](https://img.shields.io/badge/powered%20by-NumFOCUS-orange.svg?style=flat&colorA=E1523D&colorB=007D8A)](https://numfocus.org)

conda-forge is a community-led conda channel of installable packages.
In order to provide high-quality builds, the process has been automated into the
conda-forge GitHub organization. The conda-forge organization contains one repository
for each of the installable packages. Such a repository is known as a *feedstock*.

A feedstock is made up of a conda recipe (the instructions on what and how to build
the package) and the necessary configurations for automatic building using freely
available continuous integration services. Thanks to the awesome service provided by
[Azure](https://azure.microsoft.com/en-us/services/devops/), [GitHub](https://github.com/),
[CircleCI](https://circleci.com/), [AppVeyor](https://www.appveyor.com/),
[Drone](https://cloud.drone.io/welcome), and [TravisCI](https://travis-ci.com/)
it is possible to build and upload installable packages to the
[conda-forge](https://anaconda.org/conda-forge) [anaconda.org](https://anaconda.org/)
channel for Linux, Windows and OSX respectively.

To manage the continuous integration and simplify feedstock maintenance,
[conda-smithy](https://github.com/conda-forge/conda-smithy) has been developed.
Using the ``conda-forge.yml`` within this repository, it is possible to re-render all of
this feedstock's supporting files (e.g. the CI configuration files) with ``conda smithy rerender``.

For more information, please check the [conda-forge documentation](https://conda-forge.org/docs/).

Terminology
===========

**feedstock** - the conda recipe (raw material), supporting scripts and CI configuration.

**conda-smithy** - the tool which helps orchestrate the feedstock.
                   Its primary use is in the construction of the CI ``.yml`` files
                   and simplify the management of *many* feedstocks.

**conda-forge** - the place where the feedstock and smithy live and work to
                  produce the finished article (built conda distributions)


Updating pam-stubs-feedstock
============================

If you would like to improve the pam-stubs recipe or build a new
package version, please fork this repository and submit a PR. Upon submission,
your changes will be run on the appropriate platforms to give the reviewer an
opportunity to confirm that the changes result in a successful build. Once
merged, the recipe will be re-built and uploaded automatically to the
`conda-forge` channel, whereupon the built conda packages will be available for
everybody to install and use from the `conda-forge` channel.
Note that all branches in the conda-forge/pam-stubs-feedstock are
immediately built and any created packages are uploaded, so PRs should be based
on branches in forks, and branches in the main repository should only be used to
build distinct package versions.

In order to produce a uniquely identifiable distribution:
 * If the version of a package **is not** being increased, please add or increase
   the [``build/number``](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html#build-number-and-string).
 * If the version of a package **is** being increased, please remember to return
   the [``build/number``](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html#build-number-and-string)
   back to 0.

Feedstock Maintainers
=====================

* [@hmaarrfk](https://github.com/hmaarrfk/)

