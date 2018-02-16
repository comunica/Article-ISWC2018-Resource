## Related Work
{:#related-work}

TODO: actor/publish-subscribe/mediator patterns?

### Triple Pattern Fragments

TODO?

### SPARQL engines

Most of the existing SPARQL engines are coupled with an internal store.
This store can either be native RDF store, such as [AllegroGraph](cite:cites allegrograph) or [Blazegraph](cite:cites blazegraph),
or a non-RDF store can be used in the background, such as [Virtuoso](cite:cites virtuoso) that is a Object-Relational Database Management System
and evaluates SPARQL queries by translating them into SQL.

The [Triple Pattern Fragments client](cite:cites ldf) is a client-side SPARQL engine that is decoupled from an RDF store.
It is able to retrieve data from TPF entrypoints using triple patterns.
But it can not directly interact with other types of datasources.
Furthermore, the TPF client is built for the Web, as it can run in any JavaScript environment, such as the browser.

[Jena](cite:cites jena) is an open source Java framework for handling Linked Data.
It contains the ARQ API, which is a SPARQL 1.1 compliant query engine.
ARQ is not coupled to a specific datasource,
instead, in-memory models or other sources such as Jena TDB can be used.
Jena Fuseki is an implementation of the SPARQL protocol using this framework.
ARQ is similar to the Comunica engine that we propose, but it is different on 3 different levels:

1. ARQ is a Java engine, while Comunica is built for the Web, i.e., it is implemented in JavaScript.
2. Comunica is modular and supports declarative module injection, while ARQ only supports customization by implementing a custom query engine programmatically to handle operators.
3. ARQ supports federation over SPARQL endpoints, while Comunica supports federated query evaluation over heterogeneous sources, including not only SPARQL endpoints, but also TPF entrypoints and more.