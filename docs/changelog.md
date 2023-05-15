# 🔖 0.0.1-a02 (2023-05-15)

## datagram-transport-c

* :star2: udp transport changed name to datagram transport, to clarify that it is an interface and not directly tied to UDP.
* :hammer_and_wrench: datagram transport receive now returns `ssize_t`.

## flood-c - Octet Streams

* :lady_beetle: soft error instead of execution halt for failed verification of markers.

## hazy - Internet Simulator

* :star2: use new datagram transport.
* :lady_beetle: log instead of halting on buffer overflow.
* :hammer_and_wrench: tweaked values for good, recommended and worst case.

## monotonic-time-c

* :lady_beetle: suspicious lower bits diff is now a soft error and does not halt execution.

## nimble-client-c - Nimble Protocol Client

* :star2: use new datagram transport
* :star2: Lagometer. A sliding window with latency and drop information for each packet received.

## nimble-engine-client - Nimble Client with rollback and prediction

* :star2: use new datagram transport.
* :lady_beetle: use time tick to handle cases where update was called too frequently.
* :lady_beetle: check for overflow of predicted buffer.
* :lady_beetle: temporary skip ahead. There is still a bug where it sometimes can not skip ahead, but this fix might help with some cases.

## nimble-server-lib - Nimble Server library

* :hammer_and_wrench: renamed from nbd to nimble server. Makes it eaiser to understand where the code is coming from.
* :book: clarify forced step in documentation.
* :star2: use multi transport.

## seer-c - Prediction library

* :hammer_and_wrench: rename to participantId instead of index.
