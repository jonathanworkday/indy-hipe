- Name: schema-overlays
- Author: Jonathan Reynolds jonathan.reynolds@workday.com
- Start Date: 2018-09-18
- PR:
- Jira Issue:

# Summary
[summary]: #summary

Schema Overlays are intended to provide extra layers of contextual information to a schema. This extra context can then be used by an agent to transform how information is displayed to the viewer. As described in this document, a Schema Overlay is optionally applied by an agent. Overlays are intended to be ledger based and can therefore be both searched for by and provided by reference to an agent.

# Motivation
[motivation]: #motivation

Schema Overlays solve several use cases:
* Internationalisation: Provide consistent, authoritative translations of schema field names to another language.
* Context translation: Provide consistent, authoritative translations of schema field names to be more relevant terminology for a given use.
* Privacy: Flag a field as being sensitive data such that the user can make a sound decision when providing the data as a proof.
* Hiding fields: Since not all fields will be relevant to a given use case, overlays can suggest that the agent hides or conditionally hides fields from the user.

# Tutorial
[tutorial]: #tutorial

### Problem to be solved
* An agent needs suggestions on how to display data to a user for a given schema
  * Provide extra context
  * Transform what is displayed
* Because the credential definition and proof request are based on a schema definition we want a way to update optional contextual information about the schema without having to issue a new schema definition
* The extra contextual information allows for a schema to be more widely used than if the display definitions were built in to it
* Examples of transformations:
  * Render red and bold
  * Translate to Dutch
  * Hide fields
  * Conditionally hide fields
* What would be transformed?
  * Credential or proof/proof request field names
  * Display instructions are for a single schema on which the credential or proof request are based
  * Overlay display instructions can be applied in conjunction with other overlays

### How does a user know about a given overlay?
* The issuer of the credential definition
* The verifier who is asking the user for a proof request
* The holder could find it by performing a search
  * Popular overlays become standards
  * Combinations of credential definition and proof request could result in a very specific overlay being suggested
    * Assumes proof requests become storable on the ledger

### How does the user get the overlay?
* Overlays live on the ledger
* Overlays are searchable using:
  * overlay id
  * schema id
  * credential definition
  * proof request
    * Assumes proof requests become storable on the ledger

### What does an overlay contain?
* Overlay structure
  * Reference to schema + version
  * For each field to be modified
    * Reference for core lib modification (basic DSL)
      * CATEGORY:DANGEROUS
      * LANGUAGE:NL
  * Possible reference to an alternative schema document
    * Translation overlay could point to a Dutch language version of an English language schema?
* The logic to provide the transformations lives in a core library which agents would pull in
  * The logic is run by the edge agent
  * An agent could choose to pull in a third party library
* Overlays have no code or macros

### Prerequisites
* Ledger Search Service - without the ability to discover overlays through search, use will be very limited.
* Proof requests on ledger - this is a prerequisite for overlays which allow agents search for an overlay based on a proof request in the situation where some proof requests have become de facto standards for a given use case.


# Reference
[reference]: #reference

### Overlay format

Suggested format for Dutch translation overlay:
```
{
  "type":"did:sov:1234abcd;spec/schema/1.0/overlay",
  "id":"12345",
  "schemaId":"67890",
  "schemaVersion":"1.0",
  "name":"dutch translation",
  "attributes": [
    "firstName": {
      "language":{"nl":"voornaam"}
    }
  ]
}
```

Suggested format for flagging SSN as a sensitive field:
```
{
  "type":"did:sov:1234abcd;spec/schema/1.0/overlay",
  "id":"23456",
  "schemaId":"67890",
  "schemaVersion":"1.0",
  "name":"sensitive fields",
  "attributes": [
    "SSN": {
      "category":"DANGEROUS"
    }
  ]
}
```

_TODO:_ expand the following
* Overlay `libindy` code:
  * ledger read and write
  * validation
* DSL definition
* Overlay library with DSL for core overlay types to be pulled in by agents
  * What are the language choices available to us?
* Extensible for new DSL libraries which agents can import
* Ledger search for an overlay
* Update to credential and proof request definitions to allow overlays to be suggested
* Tests
  * libindy
  * Agent DSL library

_TODO:_

Provide guidance for implementers, procedures to inform testing,
interface definitions, formal function prototypes, error codes,
diagrams, and other technical details that might be looked up.

# Drawbacks
[drawbacks]: #drawbacks

* Need to manage proliferation of overlays since they increase the size of the ledger.

# Rationale and alternatives
[alternatives]: #alternatives

This design allows for certain Overlays to emerge over time as trusted de facto standards. Other than the Overlay objects being written to the ledger, this design has very little impact on the existing Indy solution. If an agent chooses to ignore Overlays, they are free to use all other features of Indy without penalty.

# Prior art
[prior-art]: #prior-art

I am not aware of prior art in this space.

# Unresolved questions
[unresolved]: #unresolved-questions

* There has already been discussion in the community to define what an Overlay is. We expect additional use cases brought up during the review process to inform this discussion.
* Design of the Overlay ledger object needs to be defined as part of the review discussion.
* It is unclear at the time of writing how this design is affected by enhancements to the Indy schema. To my knowledge, these enhancements are not yet released formally as a HIPE.
* Naming - is the term overlay the correct one?
