# Nimble Network Engine

Designed for deterministic networked games and simulations, where only the undeterministic data (usually only user input from the player) is sent.
Supports late joining by sending a complete game state on connect.

## Overview

The package consists of the following main libraries:

* [Nimble Server Lib](https://github.com/piot/nimble-server-lib)
* [Nimble Client](https://github.com/piot/nimble-client-c)

Shared libraries:

* [Nimble Serialize](https://github.com/piot/nimble-serialize-c). Shared serialization for the client and server.
* [Nimble Steps Serialize](https://github.com/piot/nimble-steps-serialize-c). Shared code for Step (user input) serialization.
* [Nimble Steps](https://github.com/piot/nimble-steps-c). Handling of steps in a circular buffer.

It also uses general libraries like:

* [BitArray](https://github.com/piot/bit-array)
* [BlobStream](https://github.com/piot/blob-stream). Reliable blob transfer over unreliable datagram transports.
* [Clog](https://github.com/piot/clog). Logging.
* [Discoid](https://github.com/piot/discoid-c). Circular buffer.
* [Flood](https://github.com/piot/flood-c). Octet streams.
* [Imprint](https://github.com/piot/imprint). Memory allocator.
* [MonotonicTime](https://github.com/piot/monotonic-time-c)
* [OrderedDatagram](https://github.com/piot/ordered-datagram-c). Discard duplicate and out of order packets.
* [Stats](https://github.com/piot/stats-c). Keep track of stats.
* [TinyLibc](https://github.com/piot/tiny_libc). Minimal c-library.
