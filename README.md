# smolcert

smolcert is an implementation of [CBOR](https://cbor.io) based certificates inspired by
[CBOR Profile of X.509 Certificates](https://tools.ietf.org/id/draft-raza-ace-cbor-certificates-00.html).

The goal is to have a more compact and easier to parse (especially on embedded systems) certificate format
than X.509. The certificate format is specified as [CDDL](https://tools.ietf.org/html/rfc8610) in the file
`spec.cddl`. Generated binary encoded certificates can be verified against this specification.

Currently exist implementations in go and Rust, where the go implementation is the more complete,
with support for easy certificate creation, signing and validation. The Rust implementation currently
only supports serialization and deserialization of certificates, but no signing, verification etc.

## Example

```go
// Create a self signed certificate (i.e. as root certificate)
cert, privateKey, err := SelfSignedCertificate("root",
		time.Now().Add(time.Minute*-1),
		time.Now().Add(time.Minute),
		[]Extension{})
require.NoError(t, err)
err = validateCertificate(cert, cert.PubKey)
assert.NoError(t, err)
```

## Running tests

`go test` will only run tests which only depend on go. To test more you need specify tags for the test
command. If you want to also run the tests for the rust based implementation along with the go tests
you need to use the `rust` tag (`go test -tags rust`). If you want to verify that the serilization
implementation generates valid data you need to specify the `cddltest` tag (`go test -tags cddltest`).
Of course tags can be combined (`go test -tags rust,cddltest`).
