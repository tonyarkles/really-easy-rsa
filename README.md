
# Really Easy RSA

Three tools to manage some certificates for you:

`make-ca` generates a Certificate Authority key and certificate

`make-x509` generates a key and a Certificate Signing Request for you

`sign-x509` takes you CA key and signs a CSR

# Usage: make-ca

First, choose a path where you're going to store your files. The
CA-related scripts assume this directory will be in an environment
variable called RER_ROOT.

```
$ export RER_ROOT=/usr/local/pki
$ mkdir -p "$RER_ROOT"
```

Next, copy ca.cnf from this directory into RER_ROOT:

```
$ cp ca.cnf "$RER_ROOT"
```

Edit the ca.cnf in your RER_ROOT. In particular, look for the CHANGEME
lines. `make-ca` will refuse to run if they're in there. Fill them in
with your own info. 

```
$ vi "$RER_ROOT/ca.cnf"
```

Now run `make-ca`. It doesn't take any command-line options.

```
$ ./make-ca
```

You'll end up with `cakey.pem` (the private key) and `ca.crt` (the
public certificate) in the `$RER_ROOT` directory.

# Usage: make-x509

This is used to generate client certificates. It takes two
command-line options: `-h` to specify the hostname / Common Name for
the certificate, and `-p` for the filename prefix.

As an example:

```
$ ./make-x509 -h www.persea.ca -p foobar
```

Will generate a Certificate Signing Request for `www.persea.ca`. This
will end up in two files: `foobar.key` is the private key, and
`foobar.csr` is the CSR that you need to get signed.

# Usage: sign-x509

This is used to sign a CSR. It takes one argument `-p`, which acts
just like the `-p` argument for `make-x509`. Carrying on that example,
`-p foobar` will take a CSR called `foobar.csr` and generate a
certificate called `foobar.crt`. The `.crt` file should be given back
to the client.

```
$ ./sign-x509 -p foobar
```

