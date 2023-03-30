# TiDID-Method-Specification
*Tianhe Distributed Identity Method Specification*

## Introduction
[Tianhe Distributed Identity](https://tianhecloud.com/product/identityAuth) (TiDID) is a blockchain-based public identity authentication basic service, providing distributed entity identity and management functions, and is committed to becoming a basic service for a smart society, realizing value exchange through trusted authentication identities.

## Status of this document
This is a draft document following the W3C DIDs Recommendation and will be continuously improved in the next one to two years.

## Tianhe Distributed Identity Method Specification
The DID identifier of Tianhe Distributed Identity is defined as: `ti`. The DID method format of the `did:ti` identification system is composed in the following format:

```
# TiDID
did:ti:<method-specific-id>

# method-specific-id 
method-specific-id = base58(ripemd160(sha256(<Public Key>)))
```
*（Reference to Bitcoin's design）*

To improve interoperability, the identifier is calculated from the public key, then is published in decentralized blockchain. An example of DID is `did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W`.

## DID Document
An example of DID document of `did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W` is
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
  "created": 1676454887,
  "updated": null,
  "verificationMethod": [
    {
      "id": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
      "type": "Secp256k1",
      "controller": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
      "publicKeyHex": "305c300d06092a864886f70d0101010500034b003048024100ca8724f210c33858"
    }
  ],
  "authentication": [
       "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W"
  ],
  "service": []
}
```

## TiDID Operations
`did:ti` method operations are implemented by the Tianhe Distributed Identity (TiDID) service that generates the identifier and creates DID documents and saved in [TiChain](https://tianhecloud.com/safeServices/safeTHChain). Users should create an account for the TiDID service before doing TiDID operations. The public request parameters could be found in Tianhe Service Unified API Documentation.

### Create
```
# endpoint
the TiDID service api url

# Input
{
    AK/SK, PUBICKEY
}

# Output
{TiDID}
```
example：
```
POST / HTTP/1.1
Host: did.tianhecloudapi.com
Content-Type: application/json
X-TC-Action: CreateTiDid
<public request parameters>

{
    "PublicKey": "7430488790716037455843285301209475352340487000630103610741044889217493578632422416688836099438173061252937088504002356479929639821721792771207629422355422",
}
```
response：
```
{
    "Response": {
        "Did": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
        "sequenceNo": "TIDID-BV8pb-1yUYw-timestamp-servicename-23"
    }
}
```
### Read
```
# endpoint
the TiDID service api url

# Input
{
    AK/SK, TiDID
}

# Output
{TDID Document}
```
example：
```
POST / HTTP/1.1
Host: did.tianhecloudapi.com
Content-Type: application/json
X-TC-Action: GetDidDocument
<public request parameters>

{
    "Did": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W"
}
```
response：
```
{
    "Response": {
        "Name": "test",
        "Document": "...",
        "sequenceNo": "TIDID-DAUqv-TO6sB-timestamp-servicename-40"
    }
}
```

### Update
DID Document does not support update.

### Deactivate
```
# endpoint
the TiDID service api url

# Input
{
    AK/SK, TiDID
}

# Output
{sequence No.}
```
example：
```
POST / HTTP/1.1
Host: did.tianhecloudapi.com
Content-Type: application/json
X-TC-Action: DeactivateTiDid
<public request parameters>

{
    "Did": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W"
}
```
response：
```
{
    "Response": {
        "sequenceNo": "TIDID-wu5o5-w1Kgc-timestamp-servicename-73"
    }
}
```

## Security considerations
1. All relevant operations of the TiDID method require data interaction through Tianhe Cloud's unified API platform. Users need to use AK/SK authentication before using TiDID operations. AK/SK authentication helps to further improve the security of TDID business.
2. TiDID uses the TiChain blockchain network as a verifiable data registry. TiChain is a blockchain independently developed by Tianhe Guoyun based on secure software and hardware and integrating the underlying mechanism of trusted data calculation and transfer. A cloud account with a TiChain node is required to call the operation method of TiDID.
3. Similar to Bitcoin, the identity of TiDID is calculated by the public key through the address conversion algorithm.

## Privacy considerations
1. The TiDID service and its DID documents do not contain the user's private data, but only the user's public key data. The DID Subject cannot be identified through the DID document.
2. Users issue verifiable credentials through TiDID to achieve the purpose of use while protecting their private information. The VC scheme guarantees the privacy and security of users.
3. The user's verifiable credentials are encrypted and stored in the cloud storage with a symmetric encryption key such as SK or other negotiated passwords, and only the entity with the key can decrypt and view it.
4. User verifiable credentials are stored under the cloud account and can only be read through cloud AK/SK authentication.









