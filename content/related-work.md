## Related Work
{:#related-work}

TODO: actor/publish-subscribe/mediator patterns?

### Linked Data Fragments

A [Linked Data Fragment](cite:cites ldf) (LDF) has been defined as a type of response to a Web interface for an RDF-based knowledge graph.
A data dump is the simplest form of LDF, it is the response of a single HTTP requests for a complete RDF dataset.
If only a subset of a dataset is needed, then downloading a full data dump is not always the most efficient way.
Alternatively, SPARQL endpoints could be used to query specific parts of a dataset.
A SPARQL endpoint response can therefore also been seen as an LDF.

Choosing an interface to publish a dataset involves a trade-off between server and client load for query evaluation.
On the one hand a raw dataset dump can be published on a simple static file server,
but it requires significant effort from clients to query within this dataset,
by for example first downloading the dump, loading it into a local SPARQL engine, and querying locally.
On the other hand, datasets can be published through a SPARQL endpoint,
which requires minimal effort from clients,
but now the server has to do all the work for evaluating the query.
The [Triple Pattern Fragments](cite:cites ldf) interface was introduced as a trade-off
between the flexibility and large server-load of SPARQL endpoints,
and the non-flexible, but low-cost publication of datadumps.

Within the LDF vision, there is no silver bullet,
i.e., there is no single interface that works well in all situations, as each one involves trade-offs.
As such, data publishers must choose the type of interface that matches their intended use case, target audience and infrastructure.
This however complicates client-side engines that are supposed to retrieve data from the resulting heterogeneity of interfaces.
As shown by the TPF approach, interfaces can be _self-descriptive_ and expose one or more [_features_](cite:cites webapifeatures),
i.e., they describe their functionality using a [common vocabulary](cite:cites hydra,describingapiresponses).
This allows clients without prior knowledge of the exact inputs and outputs of an interface
to discover its _controls_ and use it to retrieve data.

Up until now, no approaches exist that are able to query over interfaces in a heterogeneous manner,
which is exactly a problem we aim to solve with Comunica.

### SPARQL engines

In this section, we discuss SPARQL engines with and without an embedded store.
Comunica is an engine that is independent of an RDF store,
but for the sake of completeness, we also mention works _with_ a coupled store.

Most of the existing SPARQL engines are coupled with an internal store.
This can either be native a RDF store, such as for [AllegroGraph](cite:cites allegrograph) or [Blazegraph](cite:cites blazegraph),
or a non-RDF store can be used, such as for [Virtuoso](cite:cites virtuoso) that is a Object-Relational Database Management System
and evaluates SPARQL queries by translating them into SQL.

The [Triple Pattern Fragments client](cite:cites ldf) is a client-side SPARQL engine that is decoupled from an RDF store.
It is able to retrieve data over HTTP from TPF entrypoints using triple patterns.
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
3. ARQ supports federation over SPARQL endpoints, while Comunica supports federated query evaluation over heterogeneous interfaces, including not only SPARQL endpoints, but also TPF entrypoints and more.