# Nginx Module: extended_status [![Build Status](https://travis-ci.org/nginx-modules/ngx_http_extended_status_module.svg?branch=master)](https://travis-ci.org/nginx-modules/ngx_http_extended_status_module)

This module provides additional status information by extending the build-in
Nginx `status` module with finer details.

*Note: This module is not distributed with the Nginx source. Details on
installation, including patching the Nginx source and compiling Nginx with this
module, as well as configuration instructions and examples, can be found below.*

Moving forward, we have decided to *heavily fork* this module into its own, new,
and distinct codebase at
[scribenet/nginx-servats-module](https://github.com/scribenet/nginx-servats-module).

This new repository will begin as simply a re-write of the code, with the
following set of requirements:

- Apply a rigid coding standard through its implementation.
- The addition of a simple testing framework that allows for Travis continuous
  integration.
- A brand-new internal theme (suitable for 2015), that is user-customizable.
- After reaching feature-parity with its predecessor, it will break patch
  compatibility by offing additional features and capabilities outside the scope
  currently offer by the `extended_status` module.

If you have no interest in the goals outlined above and would simply like the
original module, moving forward you will find the latest code at
[nginx-modules/ngx_http_extended_status_module](https://github.com/nginx-modules/ngx_http_extended_status_module).

Otherwise, we invite you to take a look at the development being done on the new
module at [scribenet/nginx-servats-module](https://github.com/scribenet/nginx-servats-module),
including the addition of [Travis CI support](https://nginx-servats-module.docs.scribe.tools/ci),
beautiful Read the Docs generated [documentation](https://nginx-servats-module.docs.scribe.tools/docs),
as well as the new look-and-feel.

*The original contents of this README.md follow.*

# Contents

1. [Installation](#installation)
  - [Patching the Nginx source](#patching-the-nginx-source)
  - [Compilation](#compilation)
  - [Patching unsupported versions](#patching-unsupported-versions)
2. [Configuration](#configuration)
  - [Definition](#definition)
  - [Working example](#working-example)
3. [Authors and Contributors](#authors-and-contributors)
4. [License](#license)

# Installation

Installation of this module requires applying a patch to the Nginx core source-
source. As this source-code changes with each release, there are *specific patch
files* provided for various versions of Nginx.

- [`0.8.54` and `0.8.55`](extended_status-0.8.54.patch)
- [`1.0.11`](extended_status-1.0.11.patch)
- [`1.4.2`](extended_status-1.4.2.patch)
- [`1.5.2`](extended_status-1.5.2.patch)
- [`1.6.2`](extended_status-1.6.2.patch)
- [`1.7.8`](extended_status-1.7.8.patch)
- [`1.7.10`](extended_status-1.7.10.patch)

### Patching the Nginx source

To ready the Nginx source-code, choose the `*.patch` file that corresponds with
the version of Nginx you are trying to compile. Next, run the following patch
command (replacing `x.x.x` with the correct version string):

```
patch -p1 < extended_status-x.x.x.patch
```

As an example, to patch Nginx `0.8.54`/`0.8.55`, `1.0.11`, or `1.7.10`, you
would run one of the following commands, respectively:

```
patch -p1 < extended_status-0.8.54.patch
patch -p1 < extended_status-1.0.11.patch
patch -p1 < extended_status-1.7.10.patch
```

Be sure to review the output of the `patch` command and verify it was successful.
Generally, if it encounters an error it will inform you by providing the specific
block(s) within the patch it could not successfully apply.

### Compilation

*Before proceeding with this step, be sure you applied the appropriate patch per
the above instructions. If you are attempting to compile an unsupported version
of Nginx, see the below guide creating a new patch.*

To compile Nginx, run the following set of commands. You must replace the
directory path in the `./configure` line, after `--add-module=`, to point to the
location of this source code. Running `make` performs the actual compilation and
running `make install` installs the compiled files to your system.

```
./configure --add-module=dir/path/to/nginx_extended_status_module
make
make install
```

*Note: It is likely you will want to configure additional aspects of Nginx.
Running `./configure --help` will provide you with a list of available options
to further customize the build process.*

### Patching unsupported versions

If you are attempting to use this module with a version of Nginx that does not
have a pre-made `patch` file, it is suggest you proceed as follows.

- Determine the closest matching patch file available (for example, if you
  wanted to compile this module against Nginx 1.7.6, you would choose the
  [1.7.8](extended_status-1.7.8.patch) patch file).
- Attempt to apply the patch per the above instructions.
- If the patch operation *does not succeed* and outputs errors about being
  unable to apply specific blocks of the patch file:
  - Create a copy of the patch file you started with (named appropriately for
    the version of Nginx you are trying to compile).
  - Review the source code and patch file to determine the required changes.
  - Update the patch file and re-attempt to apply it once you believe you have
    satisfied any errors.
- Otherwise, if the patch operation *succeeds without error*:
  - It is possible the patch is valid against your version of Nginx.
  - It is recommended you verify the results of the patch by looking over the
    resulting Nginx source code.
- Perform the installation as described below and verify Nginx both *compiles*
  and *runs*, and that the *module behaves as expected*.

*Please: If you take the time to do this, please either submit an issue stating
that a specific patch properly worked with a version of Nginx not listed or
submit a pull request with the new patch file so others can benefit from your
effort.*

## Configuration

This module allows for one Nginx configuration directive: `extended_status`. It
can be set to either *on* or *off*, defaulting to *off*. It is valid in the
*http*, *server*, and *location* Nginx configuration contexts.

### Definition

- *Syntax*: `extended_status on|off;`
- *Default state*: `off`
- *Valid contexts*: `http`, `server`, `location`

### Working example

```
location = /extended_status {
  extended_status on;
}
location = /tablesort.min.js {
  root html;
}
```

## Authors and Contributors

The following individuals authored the bulk of this module:

* Youngseok Choi <zealot33@gmail.com>
* 李金虎 <beagem@163.com>
* Heumgeun Kang <heumgeun.kang@nhn.com> (original author)

A list of all the individuals who have contributed can be found by visiting the
[contributors page](https://github.com/nginx-modules/ngx_http_extended_status_module/graphs/contributors).

## License

This Nginx module is released under the *BSD License* unless otherwise
explicitly stated. See the [LICENSE.md](LICENSE.md) file distributed with this
source code for additional licensing information.
