# Nimble Network Engine 🕹️

Nimble Network Engine (C library) is built for deterministic networked games and simulations, efficiently transmitting only the non-deterministic data — usually just player inputs. It supports late joiners by sending a complete game state when requested, guaranteeing that all participants remain perfectly synchronized for a fully deterministic experience.

> [!IMPORTANT]
> This Nimble C version is not production-ready. Will be replaced by [Nimble Rust](https://github.com/nimble-rust/nimble) in the future.

## ✨ Overview

Low level details of the [Nimble Protocol](https://github.com/piot/nimble-serialize-c/blob/main/docs/index.adoc) and how the
[server assembles the Steps](https://github.com/piot/nimble-server-lib/blob/main/docs/index.adoc) from the clients.

The package consists of the following main libraries:

* [Nimble Server Lib](https://github.com/piot/nimble-server-lib)
* [Nimble Engine Client](https://github.com/piot/nimble-engine-client)  which uses:
  * [Nimble Client](https://github.com/piot/nimble-client-c) for handling the protocol part.
  * [Rectify](https://github.com/piot/rectify-c) handles rollback and predicting using:
    * [Assent](https://github.com/piot/assent-c) that keeps track of the authoritative simulation and state.
    * [Seer](https://github.com/piot/seer-c) given an authoritative state and predicted Steps (input) it can predict the future.
    * [Transmute](https://github.com/piot/transmute-c). Simulation abstraction (used by both Assent and Seer).
        Can tick the simulation as well as get and set simulation state.

Shared libraries used by both Client And Server:

* [Nimble Serialize](https://github.com/piot/nimble-serialize-c). Shared serialization for the client and server.
* [Nimble Steps Serialize](https://github.com/piot/nimble-steps-serialize-c). Shared code for Step (user input) serialization.
* [Nimble Steps](https://github.com/piot/nimble-steps-c). Handling of steps in a circular buffer.

It also uses general libraries like:

* [BlobStream](https://github.com/piot/blob-stream). Reliable blob transfer over unreliable datagram transports.
* [BitArray](https://github.com/piot/bit-array). Primarily used by BlobStream.
* [Clog](https://github.com/piot/clog). Logging.
* [Discoid](https://github.com/piot/discoid-c). Circular buffer.
* [Flood](https://github.com/piot/flood-c). Octet streams.
* [Imprint](https://github.com/piot/imprint). Memory allocator.
* [MonotonicTime](https://github.com/piot/monotonic-time-c)
* [OrderedDatagram](https://github.com/piot/ordered-datagram-c). Discard duplicate and out of order packets.
* [Stats](https://github.com/piot/stats-c). Keep track of stats.
* [TinyLibc](https://github.com/piot/tiny-libc). Minimal c-library headers.
* [DatagramTransport](https://github.com/piot/datagram-transport-c). Unreliable datagram transport interface.
* [Lagometer](https://github.com/piot/lagometer-c). Lagometer stats.
* [TimeTick](https://github.com/piot/time-tick-c). Time to ticks.
* [SecureRandom](https://github.com/piot/secure-random-c). Secure random.

It is recommended that [Hazy](https://github.com/piot/hazy-c) is used for Internet Simulation.
It is included in the Nimble Engine, even though it is not a dependency.

## 📦 Installation

* Download the source file zip (nimble_source_code*.zip) from the latest [release](https://github.com/piot/nimble/releases).
* Extract it to a directory
* Use the include `CMakeLists.txt` in your project [add_sub_directory](https://cmake.org/cmake/help/latest/command/add_subdirectory.html)
* add `nimble` to your [target_link_libraries](https://cmake.org/cmake/help/latest/command/target_link_libraries.html)

### Example Test Transports

There is no Transport Layer implementation included in Nimble, it only relies on the
[DatagramTransport](https://github.com/piot/datagram-transport-c) interface.

Here are two different transports that are useful for testing:

* UDP Connections. Very thin layer on top of the datagram layer (UDP).
  [Client](https://github.com/piot/udp-connections-client-c) and [Server](https://github.com/piot/udp-server-connections).

## Example Applications

Examples using Nimble Engine:

* [Nimble Ball](https://github.com/piot/nimble-ball)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

*Copyright (c) Peter Bjorklund - All rights reserved.*

## Contributions

Currently, we are not accepting external contributions to this project.
