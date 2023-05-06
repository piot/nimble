# Nimble Network Engine

Designed for deterministic networked games and simulations, where only the undeterministic data (usually only user input from the player) is sent.
Supports late joining by sending a complete game state on connect.

## Overview

Low level details of the [Nimble Protocol](https://github.com/piot/nimble-serialize-c/blob/main/docs/index.adoc) and how the [server assembles the Steps](https://github.com/piot/nimble-server-lib/blob/main/docs/index.adoc) from the clients.

The package consists of the following main libraries:

* [Nimble Server Lib](https://github.com/piot/nimble-server-lib)
* [Nimble Engine Client](https://github.com/piot/nimble-engine-client)  which uses:
  * [Nimble Client](https://github.com/piot/nimble-client-c) for handling the protocol part.
  * [Rectify](https://github.com/piot/rectify-c) handles rollback and predicting using:
    * [Assent](https://github.com/piot/assent-c) that keeps track of the authoritative simulation and state.
    * [Seer](https://github.com/piot/seer-c) given an authoritative state and predicted Steps (input) it can predict the future.
    * [Transmute](https://github.com/piot/transmute-c). Simulation abstraction (used by both Assent and Seer). Can tick the simulation as well as get and set simulation state.

Shared libraries used by both Client And Server:

* [Nimble Serialize](https://github.com/piot/nimble-serialize-c). Shared serialization for the client and server.
* [Nimble Steps Serialize](https://github.com/piot/nimble-steps-serialize-c). Shared code for Step (user input) serialization.
* [Nimble Steps](https://github.com/piot/nimble-steps-c). Handling of steps in a circular buffer.

It also uses general libraries like:

* [BlobStream](https://github.com/piot/blob-stream). Reliable blob transfer over unreliable datagram transports.
* [BitArray](https://github.com/piot/bit-array)
* [Clog](https://github.com/piot/clog). Logging.
* [Discoid](https://github.com/piot/discoid-c). Circular buffer.
* [Flood](https://github.com/piot/flood-c). Octet streams.
* [Imprint](https://github.com/piot/imprint). Memory allocator.
* [MonotonicTime](https://github.com/piot/monotonic-time-c)
* [OrderedDatagram](https://github.com/piot/ordered-datagram-c). Discard duplicate and out of order packets.
* [Stats](https://github.com/piot/stats-c). Keep track of stats.
* [TinyLibc](https://github.com/piot/tiny-libc). Minimal c-library headers.
* [UdpTransport](https://github.com/piot/udp-transport). Unreliable datagram transport interface.

It is recommended that [Hazy](https://github.com/piot/hazy-c) is used for Internet Simulation. It is included in the Nimble Engine, even though it is not a dependency.

Examples using Nimble Engine:

* [Nimble Ball](https://github.com/piot/nimble-ball).
  * [Nimble Ball Presentation](https://github.com/piot/nimble-ball-presentation)
    * [Sdl Render](https://github.com/piot/sdl-render)
  * [Nimble Ball Simulation](https://github.com/piot/nimble-ball-simulation)
