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

* Overlay definition
* Overlay `libindy` code:
  * ledger read and write
  * validation
* Overlay library with DSL for core overlay types to be pulled in by agents
  * What are the language choices available to us?
* Extensible for new DSL libraries which agents can import
* Ledger search for an overlay
* Update to credential and proof request definitions to allow overlays to be suggested


_TODO:_

Provide guidance for implementers, procedures to inform testing,
interface definitions, formal function prototypes, error codes,
diagrams, and other technical details that might be looked up.
Strive to guarantee that:

- Interactions with other features are clear.
- Implementation trajectory is well defined.
- Corner cases are dissected by example.

# Drawbacks
[drawbacks]: #drawbacks

* Need to manage proliferation of overlays since they increase the ledger size.

# Rationale and alternatives
[alternatives]: #alternatives

_TODO:_

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not
choosing them?
- What is the impact of not doing this?

# Prior art
[prior-art]: #prior-art

_TODO:_

Discuss prior art, both the good and the bad, in relation to this proposal.
A few examples of what this can include are:

- Does this feature exist in other SSI ecosystems and what experience have
their community had?
- For other teams: What lessons can we learn from other attempts?
- Papers: Are there any published papers or great posts that discuss this?
If you have some relevant papers to refer to, this can serve as a more detailed
theoretical background.

This section is intended to encourage you as an author to think about the
lessons from other implementers, provide readers of your proposal with a
fuller picture. If there is no prior art, that is fine - your ideas are
interesting to us whether they are brand new or if they are an adaptation
from other communities.

Note that while precedent set by other communities is some motivation, it
does not on its own motivate an enhancement proposal here. Please also take
into consideration that Indy sometimes intentionally diverges from common
identity features.

# Unresolved questions
[unresolved]: #unresolved-questions

_TODO:_

- What parts of the design do you expect to resolve through the
enhancement proposal process before this gets merged?
- What parts of the design do you expect to resolve through the
implementation of this feature before stabilization?
- What related issues do you consider out of scope for this
proposal that could be addressed in the future independently of the
solution that comes out of this doc?
