# netlogo-actions

<!-- badges: start -->
[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-bd0000.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Contributor Covenant 3.0 Code of Conduct](https://img.shields.io/badge/Contributor%20Covenant-3.0-4baaaa.svg)](https://www.contributor-covenant.org/version/3/0/code_of_conduct/)
<!-- badges: end -->

## Overview

This repository provides [GitHub Actions](https://github.com/features/actions) workflows for running [NetLogo](https://ccl.northwestern.edu/netlogo/) simulations in CI/CD environments. You can use it to automate testing and execution of NetLogo models within your GitHub workflows.

At the moment, it includes only the `setup-netlogo` action, which installs NetLogo on the runner machine. Click [here](https://github.com/danielvartan/netlogo-check) to see a complete example of using this action to test NetLogo models.

> If you find this project useful, please consider giving it a star! &nbsp; [![GitHub Repository Stars](https://img.shields.io/github/stars/danielvartan/netlogo-actions)](https://github.com/danielvartan/netlogo-actions/)

> The continuous development of `netlogo-actions` depends on community support. If you can afford to do so, please consider becoming a sponsor. &nbsp; [![](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/danielvartan)

## `setup-netlogo`

This action sets up a NetLogo environment for use in GitHub Actions by:

- Downloading a specified version of NetLogo.
- Optionally caching the installation across workflow runs.
- Adding NetLogo to `PATH`.
- Setting the following environment variables:
  - `NETLOGO_HOME`: Path to the NetLogo installation directory.
  - `NETLOGO_CONSOLE`: Path to the NetLogo console executable.

### Inputs

The action accepts the following inputs:

- `version`: The NetLogo version to use (e.g., `"6.4.0"`). See the available versions [here](https://ccl.northwestern.edu/netlogo/oldversions.shtml) (default: `"7.0.2"`).
- `architecture`: The NetLogo system architecture to use (`"32"` or `"64"`) (default: `"64"`).
- `cache`: Whether NetLogo should be cached across runs or not (default:`"true"`).

### Basic Usage

A basic workflow to set up NetLogo and run a model might look like this:

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4

  - name: Setup NetLogo
    uses: danielvartan/netlogo-actions/setup@v1

  - name: Run NetLogo Model
    run: |
      NetLogo_Console \
        --model my-model.nlogox \
        --experiment my-experiment \
        --table output.csv
```

### With a Specific Version

You can also specify a particular NetLogo version to install:

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4

  - name: Setup NetLogo 6.4.0
    uses: danielvartan/netlogo-actions/setup@v1
    with:
      version: "6.4.0"

  - name: Test Installation
    run: NetLogo_Console --version
```

### Matrix Usage

The action also allows you to run workflows across multiple NetLogo versions using a matrix strategy.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        netlogo: ["6.4.0", "7.0.2"]
    name: NetLogo ${{ matrix.netlogo }}
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup NetLogo
        uses: danielvartan/netlogo-actions/setup@v1
        with:
          version: ${{ matrix.netlogo }}

      - name: Run Tests
        run: |
          NetLogo_Console \
            --model my-model.nlogox \
            --experiment test-experiment
```

## Supported Platforms

All actions support only Linux runners (e.g., `ubuntu-latest`). We do not plan to support other operating systems at this time.

## License

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-bd0000.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

```text
Copyright (C) 2025 Daniel Vartanian

The netlogo-actions is free software: you can redistribute it and/or
modify it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or (at your
option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with
this program. If not, see <https://www.gnu.org/licenses/>.
```

## Contributing

[![Contributor Covenant 3.0 Code of Conduct](https://img.shields.io/badge/Contributor%20Covenant-3.0-4baaaa.svg)](https://www.contributor-covenant.org/version/3/0/code_of_conduct/)

Contributions are always welcome! Whether you want to report bugs, suggest new features, or help improve the code or documentation, your input makes a difference.

Before opening a new issue, please check the [issues tab](https://github.com/danielvartan/netlogo-actions/issues) to see if your topic has already been reported.
