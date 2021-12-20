---
version: 0.1
title: Community Parameters Convention for Standard Profile Assets (ASAs)
status: Draft
---

# Community Parameters Convention for Standard Profile Assets (ASAs)

## Summary

Standard paramaters convention for creating and displaying public user profiles on Algorand blockchain applications.

## Abstract

The goal is to create an ASA convention for any applications where profiles can be associated with account addresses. The user profile is an ASA that stores its metadata as JSON objects in transactions note.

## Specification

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**MAY**", and "**OPTIONAL**" in this document are to be interpreted as described in [RFC-2119](https://www.ietf.org/rfc/rfc2119.txt).

A PROFILE ASA has an associated JSON Metadata file, formatted as specified below, that is stored on-chain in the most recent asset configuration transaction.

### ASA Parameters Conventions

The ASA parameters should follow the following conventions:

* *Asset Name* (`an`): no restriction
* *Unit Name* (`un`): **MUST** be `PROFILE`
* *Total* (`t`): **MUST** be 1
* *Decimals* (`dc`): **MUST** be 0
* *Asset URL* (`au`): no restriction
* *Asset Metadata Hash* (`am`): no restriction
    * **OPTIONAL**
* *Freeze Address* (`f`): 
    * **SHOULD** be empty
* *Clawback Address* (`c`): 
    * **SHOULD** be empty

There are no requirements regarding the manager account of the ASA, or the reserve account. However, if immutability is required the manager address **MUST** be removed.

### JSON Metadata File Schema

The JSON Metadata schema is as follows:

```json
{
    "title": "Profile Metadata",
    "type": "object",
    "properties": {
        "name": {
            "description": "Name or username of the user",
            "type": "string",
        },
        "picture": {
            "description": "A URI pointing to the profile picture.",
            "type": "string"
        },
        "bio": {
            "description": "Text describing the user. Also known as its biography.",
            "type": "string"
        },
        "links": {
            "description": "Links to the user's external pages.",
            "type": "array",
            "items": {
                "description": "Link to an external page.",
                "type": "object",
                "properties": {
                    "name": {
                        "description": "Name of the website linked.",
                        "type": "string",
                    },
                    "url": {
                        "description": "URL pointing at an external website.",
                        "type": "string",
                    }
                }
            }
            
        },
    }
}
```
The `picture` field: 

* **MUST** be an image media type
* **SHOULD** link to a file small enough to fetch quickly in a gallery view.
* **MUST** follow [RFC-3986](https://www.ietf.org/rfc/rfc3986.txt) and **MUST NOT** contain any whitespace character.
* **SHOULD** use one of the following URI schemes (for compatibility and security): *https* and *ipfs*:
    * When the file is stored on IPFS, the `ipfs://...` URI **SHOULD** be used. IPFS Gateway URI (such as `https://ipfs.io/ipfs/...`) **SHOULD NOT** be used.
* **SHOULD NOT** use the following URI scheme: *http* (due to security concerns)


#### Examples

##### Basic Example

An example of a profile JSON Metadata file for a song follows. The properties array proposes some **SUGGESTED** formatting for token-specific display properties and metadata.

```json

{   
    "name": "John Doe",
    "picture": "ipfs://bafybeic4xuwsedtfjohcrc2kbr6ixt4u4gbq6oyn6vretq2soefulgmkqm",
    "bio": "NFT collector on the Algorand blockchain.",
    "links": [
        {
            "name":"Twitter",
            "value":"https://twitter.com/johndoe-sample"
        },
        {
            "name":"Website",
            "value":"https://johndoe.com"
        },
    ]
}
```

#### Mutability

##### Rendering

Clients SHOULD render a profile's latest metadata. Clients MAY render an ASA's previous metadata for changelogs or other historical features.

##### Updating Profile metadata

Managers MAY update a profile metadata. To do so, they MUST send a new `acfg` transaction with the entire metadata represented as JSON in the transaction's `note` field.

##### Making Profile metadata immutable

Managers MAY make a profile immutable. To do so, they MUST remove the ASA's manager address with an `acfg` transaction.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).