The following is the input JWT Claims Set:

```json
{
    "@context": [
       "https://www.w3.org/ns/credentials/v2", 
       "https://www.w3.org/ns/credentials/examples/v2"
    ],
    "id": "https://example.edu/credentials/1872",
    "type": [
        "VerifiableCredential"
    ],
    "issuer": "did:example:issuer",
    "credentialSubject": {
        "name": "Alice",
        "age": 30,
        "email": "alice@example.com",
        "address": "北京望京",
        "grade": "A",
        "id": "did:example:holder"
    }
}
```



The following is the issued SD-JWT:

```text
eyJhbGciOiJFUzI1NiIsInR5cCI6ImFwcGxpY2F0aW9uL2pzb24rc2Qtand0In0.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvdjIiLCJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvZXhhbXBsZXMvdjIiXSwiaWQiOiJodHRwczovL2V4YW1wbGUuZWR1L2NyZWRlbnRpYWxzLzE4NzIiLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIl0sImlzc3VlciI6ImRpZDpleGFtcGxlOmlzc3VlciIsImNyZWRlbnRpYWxTdWJqZWN0Ijp7Il9zZCI6WyJOMUZUUW1ReGVYVlpOMVIxWjFaWmVXWnpSVEZMVDFrM1Nrd3pWRloxUkZoeVFUWnpjSEE1VTFSUVVRIiwiUVRKRVRraG1iV1pJUjBKbFFXWkVkMlZFTFVrMmJVSlZObTV3VVdadmJHUklRek5DUjAxa1dERklTUSIsIldFTmtkMlY0TmxBM1VsbHVaM2RDZDNvdFJXRlBZVzl5YTNGNWQyZzBjekF5VTNrMFpHOXhUVlZvTkEiLCJVM1JEU3pkalkwaEJOa3B2WDFkaFlYUjJkR3h2TVd0blNqZG5NR1JQUW1sdU4wZFJlbHBqWld4WWJ3IiwiZEhCUlpFeGZWV1pXUm1acmIxcG1lRmhwZVVobGNYZFBSVkZZT0U5S2RsaFhZMnBVWkd4NGFWZFNkdyJdLCJpZCI6ImRpZDpleGFtcGxlOmhvbGRlciJ9LCJjbmYiOnsiandrIjp7ImNydiI6IlAtMjU2Iiwia2lkIjoiMXVUM0pvMmNlOGt0UGY4MW9oOUFOZkV0MzduNjhMekR3MzBJck84dHo3cyIsImt0eSI6IkVDIiwieCI6IlN3YjcxNEVKZWNSeVJmNFNMS0NmVzREcDdPczNVUkpjaHJwcDBBTXlKTEkiLCJ5IjoiZU9PZW5zalQ1WFBBcGRnVDRienRTamxMNTlQZmZmMUNfdklHZ2ZNMnh6RSJ9fSwibm9uY2UiOiJ1d2ZWbWFCZTgzZ3Z5eGlPZkE1VVRnIiwiX3NkX2FsZyI6InNoYS0yNTYifQ.WS02vyc-Ape-J4Jwlr7uoMOPcx0gV_YWEScNMAPhOgu4ES_Qw2A8zEyisqAE1KAel3CgmaaWkiWGkyboYXdSNw~WyJfbDNrTm84djFwUFVmVFpaeFAtMVpBIiwibmFtZSIsIkFsaWNlIl0~WyJMbUR1NklodTU4UEV0TWl5NDhPa0hRIiwiYWdlIiwzMF0~WyJEZnh5N1otNUtBNW1FYzBHVDgtSENBIiwiZW1haWwiLCJhbGljZUBleGFtcGxlLmNvbSJd~WyJrUEpYOEQzR3F4R1pNWVNJQkU2SURRIiwiYWRkcmVzcyIsIuWMl-S6rOacm-S6rCJd~WyJxR1hyaExST2l2amJBNEJ0R3NzU2hBIiwiZ3JhZGUiLCJBIl0~
```




The following payload is used for the SD-JWT:

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "id": "https://example.edu/credentials/1872",
  "type": [
    "VerifiableCredential"
  ],
  "issuer": "did:example:issuer",
  "credentialSubject": {
    "_sd": [
      "N1FTQmQxeXVZN1R1Z1ZZeWZzRTFLT1k3SkwzVFZ1RFhyQTZzcHA5U1RQUQ",
      "QTJETkhmbWZIR0JlQWZEd2VELUk2bUJVNm5wUWZvbGRIQzNCR01kWDFISQ",
      "WENkd2V4NlA3UlluZ3dCd3otRWFPYW9ya3F5d2g0czAyU3k0ZG9xTVVoNA",
      "U3RDSzdjY0hBNkpvX1dhYXR2dGxvMWtnSjdnMGRPQmluN0dRelpjZWxYbw",
      "dHBRZExfVWZWRmZrb1pmeFhpeUhlcXdPRVFYOE9KdlhXY2pUZGx4aVdSdw"
    ],
    "id": "did:example:holder"
  },
  "cnf": {
    "jwk": {
      "crv": "P-256",
      "kid": "1uT3Jo2ce8ktPf81oh9ANfEt37n68LzDw30IrO8tz7s",
      "kty": "EC",
      "x": "Swb714EJecRyRf4SLKCfW4Dp7Os3URJchrpp0AMyJLI",
      "y": "eOOensjT5XPApdgT4bztSjlL59Pfff1C_vIGgfM2xzE"
    }
  },
  "nonce": "uwfVmaBe83gvyxiOfA5UTg",
  "_sd_alg": "sha-256"
}
```



This is an example of an SD-JWT that discloses only address、name:

```text
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "type": "VerifiablePresentation",
  "verifiableCredential": [
    {
      "@context": "https://www.w3.org/ns/credentials/v2",
      "type": "EnvelopedVerifiableCredential",
      "id": "data:application/vc+sd-jwt,eyJhbGciOiJFUzI1NiIsInR5cCI6ImFwcGxpY2F0aW9uL2pzb24rc2Qtand0In0.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvdjIiLCJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvZXhhbXBsZXMvdjIiXSwiaWQiOiJodHRwczovL2V4YW1wbGUuZWR1L2NyZWRlbnRpYWxzLzE4NzIiLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIl0sImlzc3VlciI6ImRpZDpleGFtcGxlOmlzc3VlciIsImNyZWRlbnRpYWxTdWJqZWN0Ijp7Il9zZCI6WyJOMUZUUW1ReGVYVlpOMVIxWjFaWmVXWnpSVEZMVDFrM1Nrd3pWRloxUkZoeVFUWnpjSEE1VTFSUVVRIiwiUVRKRVRraG1iV1pJUjBKbFFXWkVkMlZFTFVrMmJVSlZObTV3VVdadmJHUklRek5DUjAxa1dERklTUSIsIldFTmtkMlY0TmxBM1VsbHVaM2RDZDNvdFJXRlBZVzl5YTNGNWQyZzBjekF5VTNrMFpHOXhUVlZvTkEiLCJVM1JEU3pkalkwaEJOa3B2WDFkaFlYUjJkR3h2TVd0blNqZG5NR1JQUW1sdU4wZFJlbHBqWld4WWJ3IiwiZEhCUlpFeGZWV1pXUm1acmIxcG1lRmhwZVVobGNYZFBSVkZZT0U5S2RsaFhZMnBVWkd4NGFWZFNkdyJdLCJpZCI6ImRpZDpleGFtcGxlOmhvbGRlciJ9LCJjbmYiOnsiandrIjp7ImNydiI6IlAtMjU2Iiwia2lkIjoiMXVUM0pvMmNlOGt0UGY4MW9oOUFOZkV0MzduNjhMekR3MzBJck84dHo3cyIsImt0eSI6IkVDIiwieCI6IlN3YjcxNEVKZWNSeVJmNFNMS0NmVzREcDdPczNVUkpjaHJwcDBBTXlKTEkiLCJ5IjoiZU9PZW5zalQ1WFBBcGRnVDRienRTamxMNTlQZmZmMUNfdklHZ2ZNMnh6RSJ9fSwibm9uY2UiOiJ1d2ZWbWFCZTgzZ3Z5eGlPZkE1VVRnIiwiX3NkX2FsZyI6InNoYS0yNTYifQ.WS02vyc-Ape-J4Jwlr7uoMOPcx0gV_YWEScNMAPhOgu4ES_Qw2A8zEyisqAE1KAel3CgmaaWkiWGkyboYXdSNw~WyJfbDNrTm84djFwUFVmVFpaeFAtMVpBIiwibmFtZSIsIkFsaWNlIl0~WyJrUEpYOEQzR3F4R1pNWVNJQkU2SURRIiwiYWRkcmVzcyIsIuWMl-S6rOacm-S6rCJd~"
    }
  ]
}

```



After the validation, the Verifier will have the following Processed SD-JWT Payload available for further handling:

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "id": "https://example.edu/credentials/1872",
  "type": [
    "VerifiableCredential"
  ],
  "issuer": "did:example:issuer",
  "credentialSubject": {
    "address": "北京望京",
	"name": "Alice"
    "id": "did:example:holder"
  },
  "cnf": {
    "jwk": {
      "crv": "P-256",
      "kid": "1uT3Jo2ce8ktPf81oh9ANfEt37n68LzDw30IrO8tz7s",
      "kty": "EC",
      "x": "Swb714EJecRyRf4SLKCfW4Dp7Os3URJchrpp0AMyJLI",
      "y": "eOOensjT5XPApdgT4bztSjlL59Pfff1C_vIGgfM2xzE"
    }
  },
  "nonce": "uwfVmaBe83gvyxiOfA5UTg",
  "_sd_alg": "sha-256"
}
```



