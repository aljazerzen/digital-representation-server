## Problem

> Internet would benefit from distributed protocols for commonly used services.

Examples of commonly used services:
- authentication
- communication (email, instant messaging)
- payments
- product browsing and ordering (online shop)
- social media

Current average implementation of commonly used services:
- lacks control over stored user data
- cannot interact with other implementations of the service
- has no standard format for specifying addresses, locations or other objects
- is difficult to transition to another service implementation

Additionally, every implementation induces development and maintenance costs.

## Solution

**Digital Representative Server** is an idea of a protocol bundle, whose goal is to solve problems outlined above and simplify architecture of most websites and online services.

Core proposal is a server that would represent an individual or a company in the digital world and interact with online services for them. It would be like an email server, only that it could provide multiple services.

----

The whole concept is currently under development. 

If you would like to contribute, please open an issue at [GitHub](https://github.com/aljazerzen/digital-representative/issues), or send me an email to [aljaz.erzen@gmail.com](mailto://aljaz.erzen@gmail.com)
