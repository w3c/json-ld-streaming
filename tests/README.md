# Introduction

The JSON-LD Test Suite is a set of tests that can
be used to verify JSON-LD Processor conformance to the set of specifications
that constitute JSON-LD. The goal of the suite is to provide an easy and
comprehensive JSON-LD testing solution for developers creating JSON-LD Processors.

More information and an RDFS definition of the test vocabulary can be found at [vocab](https://w3c.github.io/json-ld-api/tests/vocab).

# Design

Tests driven from a top-level [manifest](manifest.jsonld) and are defined for [stream-toRdf](stream-toRdf-manifest.jsonld):

* [stream-toRdf](stream-toRdf-manifest.jsonld) tests have _input_ and _expected_ documents.
  The _expected_ results can be compared using [RDF Dataset Isomorphism](https://www.w3.org/TR/rdf11-concepts/#dfn-dataset-isomorphism).

  Stream-ToRdf tests may have a `expandContext` option, which is treated
  as an IRI relative to the manifest.

  For *NegativeEvaluationTests*, the result is a string associated with the expected error code.

Test results that include a context input presume that the context is provided locally, and not from the referenced location, thus the results will include the content of the context file, rather than a reference.

# Running tests

The top-level [manifest](manifest.jsonld) references the specific test manifests, which in turn reference each test associated with a particular type of behavior.
Implementations create their own infrastructure for running the test suite. In particular, the following should be considered:

* Some algorithms, may not preserve the order of statements listed in the input document, and provision should be taken for performing unordered array comparison, for arrays other than values of `@list`. (This may be difficult for compacted results, where array value ordering is dependent on the associated term definition).
* Some tests require the use of [JSON Canonicalization Scheme](https://tools.ietf.org/html/draft-rundgren-json-canonicalization-scheme-05) to properly generate RDF Literals from JSON literal values. This algorithm is non-normative, but is assumed to be used to properly compare results using [RDF Dataset Isomorphism](https://www.w3.org/TR/rdf11-concepts/#dfn-dataset-isomorphism). These tests are marked using the `useJCS` option.
* When comparing documents after flattening, framing or generating RDF, blank node identifiers may not be predictable. Implementations using the JSON-LD 1.0 algorithm, where output is always sorted and blank node identifiers are generated sequentially from `_:b0` may continue to use a simple object comparison. Otherwise, implementations should take this into consideration. (One way to do this may be to reduce both results and _expected_ to datsets to extract a bijective mapping of blank node labels between the two datasets as described in [RDF Dataset Isomorphism](https://www.w3.org/TR/rdf11-concepts/#dfn-dataset-isomorphism)).

# Contributing

If you would like to contribute a new test or a fix to an existing test,
please follow these steps:

1. Notify the JSON-LD mailing list, public-json-ld-wg@w3.org,
   that you will be creating a new test or fix and the purpose of the
   change.
2. Clone the git repository: git://github.com/w3c/json-ld-streaming.git
3. Make your changes and submit them via github, or via a 'git format-patch'
   to the [JSON-LD Working Group mailing list](mailto:json-ld-wg@w3.org).

# Distribution
  Distributed under the [W3C Test Suite License](http://www.w3.org/Consortium/Legal/2008/04-testsuite-license). To contribute to a W3C Test Suite, see the [policies and contribution forms](http://www.w3.org/2004/10/27-testcases).

# Disclaimer
  UNDER THE EXCLUSIVE LICENSE, THIS DOCUMENT AND ALL DOCUMENTS, TESTS AND SOFTWARE THAT LINK THIS STATEMENT ARE PROVIDED "AS IS," AND COPYRIGHT HOLDERS MAKE NO REPRESENTATIONS OR WARRANTIES, EXPRESS OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, NON-INFRINGEMENT, OR TITLE; THAT THE CONTENTS OF THE DOCUMENT ARE SUITABLE FOR ANY PURPOSE; NOR THAT THE IMPLEMENTATION OF SUCH CONTENTS WILL NOT INFRINGE ANY THIRD PARTY PATENTS, COPYRIGHTS, TRADEMARKS OR OTHER RIGHTS.
  COPYRIGHT HOLDERS WILL NOT BE LIABLE FOR ANY DIRECT, INDIRECT, SPECIAL OR CONSEQUENTIAL DAMAGES ARISING OUT OF ANY USE OF THE DOCUMENT OR THE PERFORMANCE OR IMPLEMENTATION OF THE CONTENTS THEREOF.
