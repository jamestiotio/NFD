# Notes for NFD developers

If you are new to the NDN software community, please read our
[Contributor's Guide](https://github.com/named-data/.github/blob/main/CONTRIBUTING.md).

## Code style

NFD code is subject to [NFD code style](https://redmine.named-data.net/projects/nfd/wiki/CodeStyle).

## Licensing

Contributions to NFD must be licensed under the GPL v3 or a compatible license.
If you choose the GPL v3, please use the following license boilerplate in all `.hpp`
and `.cpp` files:

```cpp
/* -*- Mode:C++; c-file-style:"gnu"; indent-tabs-mode:nil; -*- */
/*
 * Copyright (c) [Year(s)], [Copyright Holder(s)].
 *
 * This file is part of NFD (Named Data Networking Forwarding Daemon).
 * See AUTHORS.md for complete list of NFD authors and contributors.
 *
 * NFD is free software: you can redistribute it and/or modify it under the terms
 * of the GNU General Public License as published by the Free Software Foundation,
 * either version 3 of the License, or (at your option) any later version.
 *
 * NFD is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
 * without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR
 * PURPOSE.  See the GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License along with
 * NFD, e.g., in COPYING.md file.  If not, see <http://www.gnu.org/licenses/>.
 */
```

If you are affiliated to an NSF-supported NDN project institution, please use the [NDN Team License
Boilerplate](https://redmine.named-data.net/projects/nfd/wiki/NDN_Team_License_Boilerplate_(NFD)).

## Logging

Fine-grained per-module logging can be configured via the `NDN_LOG` environment variable.
This is especially useful when running unit tests or tools such as `nfdc` that do not
have a configuration file.  See `ndn-log(7)` manual page for syntax and examples.

## Unit tests

To run unit tests, NFD needs to be configured and build with unit test support:

```shell
./waf configure --with-tests # --debug is also strongly recommended while developing
./waf
```

The simplest way to run the tests is to launch the compiled binary without any parameters:

```shell
# Run core tests
./build/unit-tests-core

# Run NFD daemon tests
./build/unit-tests-daemon

# Run tools tests (e.g., nfdc)
./build/unit-tests-tools
```

The [Boost.Test framework](https://www.boost.org/doc/libs/1_71_0/libs/test/doc/html/index.html)
is very flexible and allows a number of run-time customization of what tests should be run.
For example, it is possible to choose to run only a specific test suite, only a specific
test case within a suite, specific test cases across multiple test suites, and so on:

```shell
# Run only the PIT tests inside the Table test suite of NFD daemon tests
./build/unit-tests-daemon -t Table/TestPit

# Run only the test case "Find" from the previous test suite
./build/unit-tests-daemon -t Table/TestPit/Find

# Run the "Basic" test case from all NFD RIB test suites
./build/unit-tests-daemon -t Rib/*/Basic
```

By default, Boost.Test framework will produce verbose output only when a test case fails.
If it is desired to see verbose output (result of each test assertion), add `-l all`
option to `./build/unit-tests` command.  To see test progress, you can use `-l test_suite`,
or `-p` to show a progress bar:

```shell
# Show report all log messages including the passed test notification
./build/unit-tests-daemon -l all

# Show test suite messages
./build/unit-tests-daemon -l test_suite

# Show nothing
./build/unit-tests-daemon -l nothing

# Show progress bar
./build/unit-tests-tools -p
```

There are many more command line options available, information about which can be obtained
either from the command line using the `--help` switch, or online on the
[Boost.Test website](https://www.boost.org/doc/libs/1_71_0/libs/test/doc/html/boost_test/runtime_config.html).
