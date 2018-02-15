## Requirements
{:#requirements}

In this section, we discuss the requirements for the Comunica engine,
with the aim of making it a flexible and easy-to-use successor of the TPF client for the Semantic Web research community.

* **Modular**: Adding new functionality or changing certain operations should require no changes to existing code. Furthermore, the Comunica environment should be developer-friendly, including well documented APIs and auto-generation of stub code. New modules should be registrable via a configuration file.
* **Succession of the TPF client**: Comunica should have at least all of the functionality of the TPF client, which means that it should support client-side SPARQL querying over one or more TPF entrypoints. Comunica should not be significantly slower than the TPF client.
* **Heterogeneous sources**: Next to support for querying over TPF entrypoints, Comunica should also enable combined querying over other sources, with at least support for SPARQL endpoints and data dumps in RDF serializations.
* **Environment-agnostic**: Comunica should be able to run in different kinds of environments, such as Web browsers, local JavaScript runtime engines and command-line interfaces.
* **Linked Data-based**: In order to take full advantage of the Linked Data stack, modules should be describable, configurable and wireable in RDF.
