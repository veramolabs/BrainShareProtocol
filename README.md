# BrainShareProtocol
Description of the BrainShare Protocol

## BrainShare Protocol - v0 - WORK IN PROGRESS (2023-01-31)
The BrainShare Protocol is defined by the use of a specific [VerifiableCredential](https://www.w3.org/TR/vc-data-model/) and content format, and optionally specifying that credential in a [DID](https://www.w3.org/TR/did-core/) Document.

### VerifiableCredential Definition

#### `type`
The credential's `type` field MUST be `["VerifiableCredential", "BrainShareNode"]`

#### `@context`
The credential `@context` SHOULD include `"https://veramo.io/contexts/brainsharenode/v1"`

#### `credentialSubject`
As defined in the context, the `credentialSubject` MUST include a field `cuid`, which is a "content-addressable `URI`", such as a link to a piece of content on `ipfs` or `arweave`. 

#### Content
The content available at the `cuid` MUST be a Markdown file. The content MAY include code blocks that serve to embed other Verifiable Credentials, using the `jwt+vc` or `json+vc` code block modifier. 

Example:

```
'''json+vc
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  "id": "http://example.gov/credentials/3732",
  "type": ["VerifiableCredential", "UniversityDegreeCredential"],
  "issuer": "https://example.edu",
  "issuanceDate": "2010-01-01T19:23:24Z",
  "credentialSubject": {
    "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
    "degree": {
      "type": "BachelorDegree",
      "name": "Bachelor of Science and Arts"
    }
  },
  "proof": {
    "type": "Ed25519Signature2020",
    "created": "2021-11-13T18:19:39Z",
    "verificationMethod": "https://example.edu/issuers/14#key-1",
    "proofPurpose": "assertionMethod",
    "proofValue": "z58DAdFfa9SkqZMVPxAQpic7ndSayn1PzZs6ZjWp1CktyGesjuTSwRdo
                   WhAfGFCF5bppETSTojQCrfFPP2oumHKtz"
  }
}
'''
```

note: for rendering purposes the code block above uses single quotes instead of backticks

## Purpose

Because the content linked in the credential can include links to other credentials (including other BrainShareNode Credentials), this Credential type can be used as a "node" in a "knowledge graph".


### Service Definition

To make a BrainShare public/discoverable, it is useful to include a link to the "root" BrainShareNode in a DID Document Service.

```
{
  "service": [{
    "id":"did:example:123#brain-share-1",
    "type": "BrainShareRoot",
    "serviceEndpoint": "ipfs://zq781628712687"
  }]
}
```
