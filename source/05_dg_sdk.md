## SDKs

To access the feature-rich API, you can use raw http libraries to construct RPC requests and parse responses manually. But more user-friendly ways to access it is using classes and functions which wrap this API. For the common programming languages, such as Golang, Python, Javascript and Java, we provide SDKs for wrapping the API.

[This repo](https://github.com/coinexchain/polarbear/) contains a Golang library for private key management. You can generate, import and export private keys, or use them to sign transactions. The Golang SDK uses it directly. And the other SDKs use a dynmaically-linked file (.so) of this libary for private key management.