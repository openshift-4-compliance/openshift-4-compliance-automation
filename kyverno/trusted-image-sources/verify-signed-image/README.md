# Kyverno Disallow Unsigned images

The policy disallows creating pods with unsigned images. The policy verifies that the image used by the pod is signed using an organizational private key. If an image is not signed using the organizational private key, Kyverno blocks pod creation.

## Using Cosign

The policy uses a keypair which is produced using the [cosign](https://github.com/sigstore/cosign) tool. In order to use _cosign_ to sign your image, follow the next instructions -

### Generate a keypair

```
$ cosign generate-key-pair
Enter password for private key:
Enter again:
Private key written to cosign.key
Public key written to cosign.pub
```

### Sign a container image in a registry

```
$ cosign sign --key cosign.key docker.io/mkotelni/cosign:0.1
Enter password for private key:
Pushing signature to: index.docker.io/mkotelni/cosign
```

### Verify a container against a public key

```
$ cosign verify --key cosign.pub docker.io/mkotelni/cosign:0.1

Verification for index.docker.io/mkotelni/cosign:0.1 --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - The signatures were verified against the specified public key
  - Any certificates were verified against the Fulcio roots.

[{"critical":{"identity":{"docker-reference":"index.docker.io/mkotelni/cosign"},"image":{"docker-manifest-digest":"sha256:57c1e4ff150e2782a25c8cebb80b574f81f06b74944caf972f27e21b76074194"},"type":"cosign container image signature"},"optional":null}]
```

The Kyverno policy is using a similar check in order to validate the container image signature. If the signature is not validated against the public key, the container fails to deploy.