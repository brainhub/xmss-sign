xmss-sign command-line tool to generate XMSS keys and sign messages
===========================

This is a command-line tool to generate keys, sign, and verify messages using the
post-quantum stateful hash-based signature-scheme XMSS SHA256 with h=20 (`XMSS-SHA2_20_256`), as defined in
[rfc8391](https://tools.ietf.org/html/rfc8391).

The main goal of this project is to be binary-compatible with reference code [xmss-reference](https://github.com/XMSS/xmss-reference) `XMSS-SHA2_20_256` algorithm type regarding:
* public key
* signature.

We consider definition of the public key and signature in `xmss-reference` standard. 

Installation
----------

On Fedora, 

```
	$ yum install golang-bin
	$ go version
	go version go1.14.9 linux/amd64
```

Similarly, on other distributions.

Alternatively, you can install a Go (also `go`) from [Go](https://golang.org/dl). Versions 1.13-1.15 are known to work. 

Then 

```
	$ GO111MODULE=on go get github.com/brainhub/xmss-sign@v1.0.0
	$ ~/go/bin/xmss-sign
```

or


```
	$ git clone https://github.com/brainhub/xmss-sign.git
	$ cd xmss-sign
	$ go build
	$ ./xmss-sign
```

This creates the `xmss-sign` executable that the following description uses. 

gcc versions of Go `gcc-go` don't work with this code, at least on Linux Fedora Core 32. Make sure that you are running the `go` that you downloaded. 

Usage
-----

#### Generating a key pair

To generate an XMSS key pair, run

```
    xmss-sign generate
```

This will generate a SHA-256-based key pair with `h`=20, supporting up to 1 million signatures. 

By default, and default key file names can be overriden on as options, this command generate:

* `xmss-sha256_20.key` - the private key file
* `xmss-sha256_20.key.cache` - the cache file, corresponding to the private key file
* `xmss-sha256_20.pub` - the public file

You must keep the first two files secret. Never copy them and never restore them from a backup. 
Doing so have devastating consenquences to the keys you generated. 

#### Signing

To create an XMSSMT signature on `some-file`, run

```
    xmss-sign sign -f some-file
```

This will create an XMSS signature `some-file.sig` with the `xmss-sha256_20.key`. 

This will update the `xmss-sha256_20.key` and `xmss-sha256_20.key.cache`. It it critical to have a single "live" 
version of these files, and never revert them to earlier versions. 

A different secret key and signature output file can be specified as well. See

```
   xmss-sign sign -h
```

#### Verifying

To verify the XMSSMT signature `some-file.sig` on `some-file`, run

```
    xmss-sign verify -f some-file
```

It will look for the public key in the file `xmss-sha256_20.pub`.

A different public key and signature file can be specified as well. See 

```
   xmss-sign verify -h
```

See also
--------

[xmssmt](https://github.com/bwesterb/xmssmt), a command-line utility that allows more options. 

This project differs from `xmssmt` in that we support what can be described as "raw" public key and signatures, minimum-size keys without metadata. This refers to the absense of the header that includes a magic number and parameters. 
In this project the signature and key are expected to be a defined of a larger system that makes the details of XMSS algorithm used well-defined. 
The use of algorithm selection in this project is deprecated: currently this project only supports `XMSS-SHA2_20_256` algorithm type.

Future work
-----------

Verify compatibility / correctness of private key generation, to make sure it's "standard" for best interoperability.

