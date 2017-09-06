## Key Concepts

TLS handshake enables a client & server to establish the secret keys with which they communicate

## Protocol

TLS protocols employ a 3 layer strategy

* **Message Layer** _(Handshake | Change Cipher Spec | Alert | Application Data)_
* **TLS Record Layer**
* **TCP Layer**

#### TLS Record Protocol

A simple framing layer with the following record format
```
struct {
  ContentType type;
  ProtocolVersion version;
  uint16 length;
  opaque payload[length];
} TLSRecord;
```

For TLS records:
* Each record is seperatly encrypted and MACed
* A sequence number is incorporated into the MAC
* Encryption state is chained between records

#### TLS Handshake Protocol
