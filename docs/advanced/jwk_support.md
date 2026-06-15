Json Web Key (JWK) support
==================

JSON Web Key (JWK) files are increasingly used in wallet, identity, and interoperability flows. Web3j provides a `Secp256k1JWK` type for generating JWK documents from secp256k1 keys.

The generated JWK uses:

- `kty = EC`
- `crv = secp256k1`
- public coordinates `x` and `y`
- optional private coordinate `d`

Asymmetric keys in Web3j
-----------------------------

Web3j key generation already uses secp256k1 for EVM addresses. `Secp256k1JWK` adds a simple builder so you can turn those keys into JWK output when another system expects that representation.

Generate public JWK:
-----------------------------

```java
KeyPair keyPair = Keys.createSecp256k1KeyPair();
BCECPublicKey publicKey = (BCECPublicKey) keyPair.getPublic();
Secp256k1JWK jwk = new Secp256k1JWK.Builder(publicKey).build();

ObjectMapper mapper = new ObjectMapper();
System.out.println(mapper.writeValueAsString(jwk));
```

Generate private JWK:
-----------------------------

```java
KeyPair keyPair = Keys.createSecp256k1KeyPair();
BCECPublicKey publicKey = (BCECPublicKey) keyPair.getPublic();
BCECPrivateKey privateKey = (BCECPrivateKey) keyPair.getPrivate();
Secp256k1JWK jwk = new Secp256k1JWK.Builder(publicKey).withPrivateKey(privateKey).build();

ObjectMapper mapper = new ObjectMapper();
System.out.println(mapper.writeValueAsString(jwk));
```

The builder also validates explicit `x`, `y`, and `d` values if you need to assemble a JWK from existing coordinates rather than from Bouncy Castle key objects.
