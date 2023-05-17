# Changelog

## :bookmark: 0.0.1-a04 (2023-05-17)

### hazy-c - Internet Simulator

* :star2: Added `hazyDatagramTransportDebugDiscardIncoming()`. Discards all incoming datagrams. Only used for debugging.
* :hammer_and_wrench: Made worst case a bit nicer. Reduced max jitter from 31 to 6, and decreased worst latency to 190 from 220.

### nimble-client-c - Nimble Protocol Client

* :star2: Checking for disconnect on client. If not gotten any valid datagrams for X (10) ticks or if no new authoritative steps encountered for Y (20) ticks, the connection is disconnected.
* :star2: Impending disconnect warning. Is triggered if connection is close to being disconnected. If triggered it will stay on for 20 ticks.  Useful for displaying in an application HUD.
* :star2: Dropping datagram warning. If triggered it will stay on for 20 ticks.  Useful for displaying in an application HUD.

### nimble-engine-client - Nimble Client with rollback and prediction

* :star2: Small gap in time interval warning for incoming authoritative steps. If triggered it will stay on for 20 ticks. Useful for displaying in an application HUD.
* :star2: Big gap in time interval warning for incoming authoritative steps. If triggered it will stay on for 20 ticks.

### nimble-server-lib - Nimble Server library

* :star2: Check for disconnects on the server. If steps haven't been received for 120 authoritative step compositions in a row, or if predicted steps frequently is not received in time, the client is disconnected.
* :boom: `nbd` prefix for functions changed to `nimbleServer`.
* :art: `req_step.c` split up into multiple files.
* :lady_beetle: Connection index was compared against participant connections instead of transport connections.

### nimble-steps-c

* :lady_beetle: nbsPendingStepRanges now closes last range properly.
* :vertical_traffic_light: Added test for receive mask and step ranges.
* :art: Receive mask calculation moved to separate logic.

## :bookmark: 0.0.1-a03 (2023-05-15)

### datagram-transport-c

* :star2: udp transport changed name to datagram transport, to clarify that it is an interface and not directly tied to UDP.
* :hammer_and_wrench: datagram transport receive now returns `ssize_t`.

### flood-c - Octet Streams

* :lady_beetle: soft error instead of execution halt for failed verification of markers.

### hazy-c - Internet Simulator

* :star2: use new datagram transport.
* :lady_beetle: log instead of halting on buffer overflow.
* :hammer_and_wrench: tweaked values for good, recommended and worst case.

### monotonic-time-c

* :lady_beetle: suspicious lower bits diff is now a soft error and does not halt execution.

### nimble-client-c - Nimble Protocol Client

* :star2: use new datagram transport
* :star2: Lagometer. A sliding window with latency and drop information for each packet received.

### nimble-engine-client - Nimble Client with rollback and prediction

* :star2: use new datagram transport.
* :lady_beetle: use time tick to handle cases where update was called too frequently.
* :lady_beetle: check for overflow of predicted buffer.
* :lady_beetle: temporary skip ahead. There is still a bug where it sometimes can not skip ahead, but this fix might help with some cases.

### nimble-server-lib - Nimble Server library

* :hammer_and_wrench: renamed from nbd to nimble server. Makes it eaiser to understand where the code is coming from.
* :book: clarify forced step in documentation.
* :star2: use multi transport.

### seer-c - Prediction library

* :hammer_and_wrench: rename to participantId instead of index.
