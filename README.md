# BrainShareProtocol
Description of the BrainShare Protocol

## BrainShare Protocol - v0.1 - WORK IN PROGRESS (2023-09-131)
The BrainShare Protocol is defined by the use of a specific [VerifiableCredential](https://www.w3.org/TR/vc-data-model/) and content format, and optionally specifying that credential in a [DID](https://www.w3.org/TR/did-core/) Document.

### VerifiableCredential Definition

#### `type`
The credential's `type` field MUST be `["VerifiableCredential", "BrainSharePost"]`

#### `credentialSubject`
As defined in the context, the `credentialSubject` MUST include a field `message`, which is a Markdown string that can also contain a special codeblock type allowing it to embed or reference other verifiable credentials, using the following formats:

```vc+jwt
neyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJ2YyI6eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSJdLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwiQnJhaW5zaGFyZVBvc3QiXSwiY3JlZGVudGlhbFN1YmplY3QiOnsicG9zdCI6ImZmZmYyMjIyMiJ9fSwic3ViIjoiZGlkOnBlZXI6Mi5FejZMU3J1U3JIYkY3dXBra1RCRWZkTTRxVVVnbzJ4dmdQc2hDNzNyTDdXRkRGbmZmLlZ6Nk1raFBCcGE3Vno2OEp3NU11c2d4S01haUp4Z1UzclNQRkNnUnZISzY4UmVKTXcuU2V5SnBaQ0k2SWpFeU16UWlMQ0owSWpvaVpHMGlMQ0p6SWpvaVpHbGtPbmRsWWpwa1pYWXRaR2xrWTI5dGJTMXRaV1JwWVhSdmNpNW9aWEp2YTNWaGNIQXVZMjl0SWl3aVpHVnpZM0pwY0hScGIyNGlPaUpoSUVSSlJFTnZiVzBnWlc1a2NHOXBiblFpZlEiLCJuYmYiOjE2OTQ1NjMxMjcsImlzcyI6ImRpZDpwZWVyOjIuRXo2TFNydVNySGJGN3Vwa2tUQkVmZE00cVVVZ28yeHZnUHNoQzczckw3V0ZERm5mZi5WejZNa2hQQnBhN1Z6NjhKdzVNdXNneEtNYWlKeGdVM3JTUEZDZ1J2SEs2OFJlSk13LlNleUpwWkNJNklqRXlNelFpTENKMElqb2laRzBpTENKeklqb2laR2xrT25kbFlqcGtaWFl0Wkdsa1kyOXRiUzF0WldScFlYUnZjaTVvWlhKdmEzVmhjSEF1WTI5dElpd2laR1Z6WTNKcGNIUnBiMjRpT2lKaElFUkpSRU52YlcwZ1pXNWtjRzlwYm5RaWZRIn0.kF72OisEL5jYR6R3O4VTZU751gfL44kfshW7Q0WXzTH2Q1jNro13gkUOpGEf9Ei5UkPWg-xRXNS7qEuwbQmtBw
```

```vc+json+ld
TODO: POST JSON-LD CREDENTIAL HERE
```

```vc+multihash
qz1311d142sdf894189a142
```

### WikiLinks & Index

The Markdown can also include "WikiLinks" in order to reference other BrainShare posts by name rather than hash. This can allow a Post to stay "up to date" even when referenced posts are updated (this might not always be preferrable, but likely so when building a "wiki"). To reference a post this way, the following syntax can be used:

```
This is a link to the great post called [[did:example:123#brain-share-1 My Great Post]]
```

This post is discovered by checking an "index" file specified in the Service Endpoint referenced (e.g. `did:example:123#brain-share-1`). This index file MUST be a simple mapping of strings to arrays of multihash strings. The array contains all revisions (in reverse chronological order) of the named Post.

Index Example:

```
{
  "My Great Post": ["qz2342342424", "qa897987"],
  "Science": ["qz1234555555"] 
}
```

Credential Example:

```
'''json+vc
{
  "credentialSubject": {
    "post": "Referencing another post:\n\n```vc+jwt\neyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJ2YyI6eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSJdLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwiQnJhaW5zaGFyZVBvc3QiXSwiY3JlZGVudGlhbFN1YmplY3QiOnsicG9zdCI6ImZmZmYyMjIyMiJ9fSwic3ViIjoiZGlkOnBlZXI6Mi5FejZMU3J1U3JIYkY3dXBra1RCRWZkTTRxVVVnbzJ4dmdQc2hDNzNyTDdXRkRGbmZmLlZ6Nk1raFBCcGE3Vno2OEp3NU11c2d4S01haUp4Z1UzclNQRkNnUnZISzY4UmVKTXcuU2V5SnBaQ0k2SWpFeU16UWlMQ0owSWpvaVpHMGlMQ0p6SWpvaVpHbGtPbmRsWWpwa1pYWXRaR2xrWTI5dGJTMXRaV1JwWVhSdmNpNW9aWEp2YTNWaGNIQXVZMjl0SWl3aVpHVnpZM0pwY0hScGIyNGlPaUpoSUVSSlJFTnZiVzBnWlc1a2NHOXBiblFpZlEiLCJuYmYiOjE2OTQ1NjMxMjcsImlzcyI6ImRpZDpwZWVyOjIuRXo2TFNydVNySGJGN3Vwa2tUQkVmZE00cVVVZ28yeHZnUHNoQzczckw3V0ZERm5mZi5WejZNa2hQQnBhN1Z6NjhKdzVNdXNneEtNYWlKeGdVM3JTUEZDZ1J2SEs2OFJlSk13LlNleUpwWkNJNklqRXlNelFpTENKMElqb2laRzBpTENKeklqb2laR2xrT25kbFlqcGtaWFl0Wkdsa1kyOXRiUzF0WldScFlYUnZjaTVvWlhKdmEzVmhjSEF1WTI5dElpd2laR1Z6WTNKcGNIUnBiMjRpT2lKaElFUkpSRU52YlcwZ1pXNWtjRzlwYm5RaWZRIn0.kF72OisEL5jYR6R3O4VTZU751gfL44kfshW7Q0WXzTH2Q1jNro13gkUOpGEf9Ei5UkPWg-xRXNS7qEuwbQmtBw\n```",
    "id": "did:peer:2.Ez6LSruSrHbF7upkkTBEfdM4qUUgo2xvgPshC73rL7WFDFnff.Vz6MkhPBpa7Vz68Jw5MusgxKMaiJxgU3rSPFCgRvHK68ReJMw.SeyJpZCI6IjEyMzQiLCJ0IjoiZG0iLCJzIjoiZGlkOndlYjpkZXYtZGlkY29tbS1tZWRpYXRvci5oZXJva3VhcHAuY29tIiwiZGVzY3JpcHRpb24iOiJhIERJRENvbW0gZW5kcG9pbnQifQ"
  },
  "issuer": {
    "id": "did:peer:2.Ez6LSruSrHbF7upkkTBEfdM4qUUgo2xvgPshC73rL7WFDFnff.Vz6MkhPBpa7Vz68Jw5MusgxKMaiJxgU3rSPFCgRvHK68ReJMw.SeyJpZCI6IjEyMzQiLCJ0IjoiZG0iLCJzIjoiZGlkOndlYjpkZXYtZGlkY29tbS1tZWRpYXRvci5oZXJva3VhcHAuY29tIiwiZGVzY3JpcHRpb24iOiJhIERJRENvbW0gZW5kcG9pbnQifQ"
  },
  "type": [
    "VerifiableCredential",
    "BrainsharePost"
  ],
  "@context": [
    "https://www.w3.org/2018/credentials/v1"
  ],
  "issuanceDate": "2023-09-13T01:20:24.000Z",
  "proof": {
    "type": "JwtProof2020",
    "jwt": "eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9.eyJ2YyI6eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSJdLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwiQnJhaW5zaGFyZVBvc3QiXSwiY3JlZGVudGlhbFN1YmplY3QiOnsicG9zdCI6IlJlZmVyZW5jaW5nIGFub3RoZXIgcG9zdDpcblxuYGBgand0K3ZjXG5leUpoYkdjaU9pSkZaRVJUUVNJc0luUjVjQ0k2SWtwWFZDSjkuZXlKMll5STZleUpBWTI5dWRHVjRkQ0k2V3lKb2RIUndjem92TDNkM2R5NTNNeTV2Y21jdk1qQXhPQzlqY21Wa1pXNTBhV0ZzY3k5Mk1TSmRMQ0owZVhCbElqcGJJbFpsY21sbWFXRmliR1ZEY21Wa1pXNTBhV0ZzSWl3aVFuSmhhVzV6YUdGeVpWQnZjM1FpWFN3aVkzSmxaR1Z1ZEdsaGJGTjFZbXBsWTNRaU9uc2ljRzl6ZENJNkltWm1abVl5TWpJeU1pSjlmU3dpYzNWaUlqb2laR2xrT25CbFpYSTZNaTVGZWpaTVUzSjFVM0pJWWtZM2RYQnJhMVJDUldaa1RUUnhWVlZuYnpKNGRtZFFjMmhETnpOeVREZFhSa1JHYm1abUxsWjZOazFyYUZCQ2NHRTNWbm8yT0VwM05VMTFjMmQ0UzAxaGFVcDRaMVV6Y2xOUVJrTm5VblpJU3pZNFVtVktUWGN1VTJWNVNuQmFRMGsyU1dwRmVVMTZVV2xNUTBvd1NXcHZhVnBITUdsTVEwcDZTV3B2YVZwSGJHdFBibVJzV1dwd2ExcFlXWFJhUjJ4cldUSTVkR0pUTVhSYVYxSndXVmhTZG1OcE5XOWFXRXAyWVROV2FHTklRWFZaTWpsMFNXbDNhVnBIVm5wWk0wcHdZMGhTY0dJeU5HbFBhVXBvU1VWU1NsSkZUblppVnpCbldsYzFhMk5IT1hCaWJsRnBabEVpTENKdVltWWlPakUyT1RRMU5qTXhNamNzSW1semN5STZJbVJwWkRwd1pXVnlPakl1UlhvMlRGTnlkVk55U0dKR04zVndhMnRVUWtWbVpFMDBjVlZWWjI4eWVIWm5VSE5vUXpjemNrdzNWMFpFUm01bVppNVdlalpOYTJoUVFuQmhOMVo2TmpoS2R6Vk5kWE5uZUV0TllXbEtlR2RWTTNKVFVFWkRaMUoyU0VzMk9GSmxTazEzTGxObGVVcHdXa05KTmtscVJYbE5lbEZwVEVOS01FbHFiMmxhUnpCcFRFTktla2xxYjJsYVIyeHJUMjVrYkZscWNHdGFXRmwwV2tkc2Exa3lPWFJpVXpGMFdsZFNjRmxZVW5aamFUVnZXbGhLZG1FelZtaGpTRUYxV1RJNWRFbHBkMmxhUjFaNldUTktjR05JVW5CaU1qUnBUMmxLYUVsRlVrcFNSVTUyWWxjd1oxcFhOV3RqUnpsd1ltNVJhV1pSSW4wLmtGNzJPaXNFTDVqWVI2UjNPNFZUWlU3NTFnZkw0NGtmc2hXN1EwV1h6VEgyUTFqTnJvMTNna1VPcEdFZjlFaTVVa1BXZy14UlhOUzdxRXV3YlFtdEJ3XG5gYGAifX0sInN1YiI6ImRpZDpwZWVyOjIuRXo2TFNydVNySGJGN3Vwa2tUQkVmZE00cVVVZ28yeHZnUHNoQzczckw3V0ZERm5mZi5WejZNa2hQQnBhN1Z6NjhKdzVNdXNneEtNYWlKeGdVM3JTUEZDZ1J2SEs2OFJlSk13LlNleUpwWkNJNklqRXlNelFpTENKMElqb2laRzBpTENKeklqb2laR2xrT25kbFlqcGtaWFl0Wkdsa1kyOXRiUzF0WldScFlYUnZjaTVvWlhKdmEzVmhjSEF1WTI5dElpd2laR1Z6WTNKcGNIUnBiMjRpT2lKaElFUkpSRU52YlcwZ1pXNWtjRzlwYm5RaWZRIiwibmJmIjoxNjk0NTY4MDI0LCJpc3MiOiJkaWQ6cGVlcjoyLkV6NkxTcnVTckhiRjd1cGtrVEJFZmRNNHFVVWdvMnh2Z1BzaEM3M3JMN1dGREZuZmYuVno2TWtoUEJwYTdWejY4Snc1TXVzZ3hLTWFpSnhnVTNyU1BGQ2dSdkhLNjhSZUpNdy5TZXlKcFpDSTZJakV5TXpRaUxDSjBJam9pWkcwaUxDSnpJam9pWkdsa09uZGxZanBrWlhZdFpHbGtZMjl0YlMxdFpXUnBZWFJ2Y2k1b1pYSnZhM1ZoY0hBdVkyOXRJaXdpWkdWelkzSnBjSFJwYjI0aU9pSmhJRVJKUkVOdmJXMGdaVzVrY0c5cGJuUWlmUSJ9.W5G8ejIWdE9RKmZEQUxDnGxqg2rwCcIz4bExl6XOOPjk5S1nz1KAILZAAJ0Oy6yj7ufQw-DLJ9cRbBrQTqIaBA"
  }
}
'''
```

note: for rendering purposes the code block above uses single quotes instead of backticks

## Purpose

Because the content linked in the credential can include links to other credentials (including other BrainSharePost Credentials), this Credential type can be used as a "node" in a "knowledge graph", but not restricted to 1 user/organization. Knowledge graphs (i.e. "second brains", e.g. Notion) of different entities can overlap and intersect publicly.


### Service Definition

To make a BrainShare public/discoverable, it is useful to include a link to the "root" BrainShareNode in a DID Document Service, as well as the index if "wikileaks" support is intended.

```
{
  "service": [{
    "id":"did:example:123#brain-share-1",
    "type": "BrainShareRoot",
    "root": "zq781628712687",
    "index": "zq12342314324234"
  }]
}
```
