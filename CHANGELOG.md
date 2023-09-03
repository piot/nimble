# Changelog

## :bookmark: [v0.0.1-a08](https://github.com/piot/nimble/releases/tag/v0.0.1-a08) (2023-09-03)

New feature that enables a client to rejoin the game after being disconnected.
When a client joins a game, a secret is provided by the server. That secret can be provided
on future rejoins to retain most of the clients avatar state and similar.


### [assent-c](https://github.com/piot/assent-c) - Authoritative State

* :star2: convert between Step connect state and transmute input type
* :art: clean up CMakeLists.txt

### [clog](https://github.com/piot/clog) - Basic logging

* :lady_beetle: fix incorrect `clog_gmtime_s()` ([#7](https://github.com/piot/clog/pull/7))

### [flood-c](https://github.com/piot/flood-c) - Octet streams

* :hammer_and_wrench: only write and check markers when it is a debug info stream

### [imprint](https://github.com/piot/imprint) - memory allocator

* :art: clean up CMakeLists.txt

### [nimble-client-c](https://github.com/piot/nimble-client-c) - Nimble Protocol Client

* :star2: add connect request and response. ([#2](https://github.com/piot/nimble-client-c/pull/2))
* :star2: option to request debug streams using `wantsDebugStreams`
* :art: more optimal step id to send to server based on latency. [ab1d78f](https://github.com/piot/nimble-client-c/commit/ab1d78f)

### [nimble-engine-client](https://github.com/piot/nimble-engine-client) - Prediction and rollback

* :star2: add `connectionSecretIsProvided` and `connectionSecret` fields in join options ([#2](https://github.com/piot/nimble-engine-client/pull/2))

### [nimble-serialize-c](https://github.com/piot/nimble-serialize-c) - Nimble Protocol

* :star2: connect request and response that includes request for debug streams
* :star2: serialize secret for participant connection
* :art: proper command types for join game request and join game response
* :art: clean up CMakeLists.txt

### [nimble-server-lib](https://github.com/piot/nimble-server-lib) - Nimble Server Library

* :star2: include a random secret in join game response to enable possible reconnect to the same participant connection by the client.
* :star2: add `maxWaitingForReconnectTicks` field to setup.
* :star2: keep the participantConnection alive for reconnect (up to `maxWaitingForReconnectTicks``)
* :lady_beetle: have a more forgiving disconnect timeout until the first step (input) is received from the client (presumably is done with downloading the state)

### [nimble-steps-serialize-c](https://github.com/piot/nimble-steps-serialize-c) - Nimble Steps Serialize

* :star2: add connect state that can indicate one of 'normal', 'not provided in time' and 'waiting for reconnect' ([#2](https://github.com/piot/nimble-steps-serialize-c/pull/2))
* :art: clean up CMakeLists.txt

### [seer-c](https://github.com/piot/seer-c) - Prediction Library

* :star2: convert from input type to connect state ([#2](https://github.com/piot/seer-c/pull/2))

### [time-tick-c](https://github.com/piot/time-tick-c) - Tick updater

* :art: clean up code to increase readability

### [transmute-c](https://github.com/piot/transmute-c) - Ticks simulation with input

* :star2: input type that can be either Normal, NoInputInTime, or WaitingForReconnect [77e87f2](https://github.com/piot/transmute-c/commit/77e87f2)

## :bookmark: [v0.0.1-a07](https://github.com/piot/nimble/releases/tag/v0.0.1-a07) (2023-07-04)

Idiomatic separate cmake files for each library. Fix a lot of compile warnings. Improve doxygen documentation.

### [datagram-transport-c](https://github.com/piot/datagram-transport-c) - Datagram Transport Interface

* :triangular_flag_on_post:[breaking] `datagramTransportMultiReceiveFrom()` return `ssize_t` instead of `int`. [6c3d3b9](https://github.com/piot/datagram-transport-c/commit/6c3d3b9)

### [nimble-engine-client](https://github.com/piot/nimble-engine-client) - Prediction and rollback

* :fire: remove unused field `isHostingLocally`

### [secure-random-c](https://github.com/piot/secure-random-c) - Multi-platform Secure Random

* :see_no_evil: halt execution (`SIGABRT`) in `secureRandomUInt64()` on Emscripten. [#2](https://github.com/piot/secure-random-c/pull/2)

### [time-tick-c](https://github.com/piot/time-tick-c) - Tick updater

* :art: Smoother ticks. As long as possible, do one tick for each update (does not work if update rate is a lot higher or lower than the target tick rate). [#1](https://github.com/piot/time-tick-c/pull/1)

## :bookmark: [v0.0.1-a06](https://github.com/piot/nimble/releases/tag/v0.0.1-a06) (2023-06-22)

Minor compile fixes for emscripten.

### [clog](https://github.com/piot/clog) - Basic logging

* :lady_beetle: use `tc_snprintf` instead of `sprintf` ([#1](https://github.com/piot/clog/pull/1))

### [secure-random-c](https://github.com/piot/secure-random-c) - Multi-platform Secure Random

* :see_no_evil: `secureRandomUInt64()` on emscripten that only return 0

## :bookmark: [v0.0.1-a05](https://github.com/piot/nimble/releases/tag/v0.0.1-a05) (2023-06-14)

> [!IMPORTANT]\
> Hot fixes to alleviate skip ahead problems.


### [nimble-engine-client](https://github.com/piot/nimble-engine-client) - Prediction and rollback

* :hammer_and_wrench: Increase wait time between each skip ahead attempt

### [nimble-server-lib](https://github.com/piot/nimble-server-lib) - Nimble Server Library

* :hammer_and_wrench: Skip increase forcedStepInRowCounter if client transport connection is downloading game state

## :bookmark: [v0.0.1-a04](https://github.com/piot/nimble/releases/tag/v0.0.1-a04) (2023-05-17)

Focus on disconnect detection on client and server.

### [hazy-c](https://github.com/piot/hazy-c) - Internet Simulator

* :star2: Add `hazyDatagramTransportDebugDiscardIncoming()`. Discards all incoming datagrams. Only used for debugging.
* :hammer_and_wrench: Make worst case configuration a bit nicer. Reduce max jitter from 31 to 6, and decrease worst latency to 190 from 220.

### [nimble-client-c](https://github.com/piot/nimble-client-c) - Nimble Protocol Client

* :star2: Check for disconnect on client. If not gotten any valid datagrams for X (10) ticks or if no new authoritative steps encountered for Y (20) ticks, the connection is disconnected.
* :star2: Impending disconnect warning. Is triggered if connection is close to being disconnected. If triggered it will stay on for 20 ticks.  Useful for displaying in an application HUD.
* :star2: Dropping datagram warning. If triggered it will stay on for 20 ticks.  Useful for displaying in an application HUD.

### [nimble-engine-client](https://github.com/piot/nimble-engine-client) - Prediction and rollback

* :star2: Small gap in time interval warning for incoming authoritative steps. If triggered it will stay on for 20 ticks. Useful for displaying in an application HUD.
* :star2: Big gap in time interval warning for incoming authoritative steps. If triggered it will stay on for 20 ticks.

### [nimble-server-lib](https://github.com/piot/nimble-server-lib) - Nimble Server Library

* :triangular_flag_on_post:[breaking] Change from `nbd` prefix for functions to `nimbleServer`.
* :star2: Check for disconnects on the server. If steps haven't been received for 120 authoritative step compositions in a row, or if predicted steps frequently is not received in time, the client is disconnected.
* :lady_beetle: Connection index compare against participant connections instead of transport connections.
* :recycle: file `req_step.c` split up into multiple files.

### [nimble-steps-c](https://github.com/piot/nimble-steps-c) - Steps handling

* :lady_beetle: nbsPendingStepRanges should close last range properly
* :vertical_traffic_light: Add test for receive mask and step ranges.
* :recycle: Move receive mask calculation to separate logic file and struct.

## :bookmark: [v0.0.1-a03](https://github.com/piot/nimble/releases/tag/v0.0.1-a03) (2023-05-15)

### [datagram-transport-c](https://github.com/piot/datagram-transport-c) - Datagram Transport Interface

* :star2: udp transport changed name to datagram transport, to clarify that it is an interface and not directly tied to UDP
* :hammer_and_wrench: datagram transport receive now returns `ssize_t`.

### [flood-c](https://github.com/piot/flood-c) - Octet streams

* :lady_beetle: soft error instead of execution halt for failed verification of markers.

### [hazy-c](https://github.com/piot/hazy-c) - Internet Simulator

* :star2: use new datagram transport.
* :lady_beetle: log instead of halting on buffer overflow.
* :hammer_and_wrench: tweaked values for good, recommended and worst case.

### [monotonic-time-c](https://github.com/piot/monotonic-time-c) - Monotonic Time

* :lady_beetle: suspicious lower bits diff is now a soft error and does not halt execution.

### [nimble-client-c](https://github.com/piot/nimble-client-c) - Nimble Protocol Client

* :star2: use new datagram transport
* :star2: Lagometer. A sliding window with latency and drop information for each packet received.

### [nimble-engine-client](https://github.com/piot/nimble-engine-client) - Prediction and rollback

* :star2: use new datagram transport.
* :lady_beetle: use time tick to handle cases where update was called too frequently.
* :lady_beetle: check for overflow of predicted buffer.
* :lady_beetle: temporary skip ahead. There is still a bug where it sometimes can not skip ahead, but this fix might help with some cases.

### [nimble-server-lib](https://github.com/piot/nimble-server-lib) - Nimble Server Library

* :triangular_flag_on_post:[breaking] renamed some of the function name prefix from nbd to nimble server. Makes it eaiser to understand where the code is coming from.
* :star2: use multi transport
* :book: clarify forced step in documentation.

### [seer-c](https://github.com/piot/seer-c) - Prediction Library

* :hammer_and_wrench: rename to participantId instead of index.

