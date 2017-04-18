.. _apigoals:


API Goals
!!!!!!!!!

The OmicMD api platform will allow the interoperable exchange of genomic information across multiple organizations and on multiple platforms. It overcomes the barriers of incompatible infrastructure between organizations and institutions to enable DNA data providers and consumers to better share genomic data and work together on a global scale, advancing genome research and clinical application.

1. The API must allow flexibility in server implementation, including:

* choice of persistent backend (e.g. files, SQL, NoSQL)

* choice of implementation language (e.g. Java, Python, Go)

* choice of authorization model (e.g. all public, all private, fine-grain ACLs)

* choice of import mechanism (e.g. self-service vs. centrally managed)

* choice of scale (from a single researcher working with dozens of sequences and homogeneous tools, to government-funded studies with over a million sequences and multiple tool chains)

2. The API must allow full-fidelity representation of data that was prepared using today’s common methods and stored using today’s common file formats.

* Note that real-world data files sometimes use invalid or ambiguous syntax, making it hard to understand the semantics of the contained data. If a server can't figure out what those semantics are, it can throw an error on import. But whenever the semantics are clear, including when they're specified in valid data files, the API must allow preserving them.
  
3. The API should allow adding more structure to data beyond today’s common practices (e.g. formal provenance, versioning).

4. The API should allow data owners to organize their data in ways that make sense to them, which implies there often isn’t One True Taxonomy. For example, a ReadGroupSet is defined as “a set of ReadGroups that are intended to be analyzed together” -- different researchers or clinicians might choose different sets for different purposes.

5. At the same time, the API should encourage reusable organization, anticipating a future that supports cross-researcher and cross-repository data federation.


Unresolved Issues
@@@@@@@@@@@@@@@@@

* What are the performance goals of the API in various configurations?
