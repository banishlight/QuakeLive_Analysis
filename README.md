# QuakeLive_Analysis
The parser is heavily influenced by [this library written in scalla](https://github.com/vincasmiliunas/Quake-Live-Demo-Parser), however after after having issues building the project due to its age (uses old Java version etc) I decided to rewrite in C++ in something that is a little more stable.  It seems simpler and more productive to learn how it works and rewrite it rather than fighting with old libraries and maven.

The analytics on the parsed data will be done in Python using matplotlib

Looks like [this](https://github.com/mightycow/uberdemotools/blob/develop/CUSTOM_PARSING.md) repo has details on how to parse quake demos.  This will be used as the basis for parsing the demos.


## Parsing
Majority of this is directly from the Uber Tools parsing explanation:
```
Quake demos are sequences of message bundles (let's call them
 packets) that the server sends to the client. It is, in 
 essence, a dump of the client's incoming network stream.
```
The packets are formatted like so:
```
[packet header] [packet data] [packet header] [packet data] ... [EoS packet header]
```
When all the bits of the header are set to 1(int -1), you have recieved the end of stream header packet.

3 types of headers:

| Header Type | Frequency | Descripton|
| ---- | ---- | ---- |
| Gamestate | Only sent when a map is loaded (or reloaded) | Baseline Entities + config settings |
| Snapshot | Sent continuously | Delta-encoded entity states + player states |
| Command | Sent irregularly | A command encoded as strings |

