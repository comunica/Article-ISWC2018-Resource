## Related Work
{:#related-work}

In this section, we discuss the Linked Data Fragments vision,
followed by the state-of-the-art on modular SPARQL engines.
Finally, we discuss several software design patterns that are essential in the architecture of Comunica.

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

[Jena](cite:cites jena), [RDFLib](cite:cites rdflib), [rdflib.js](cite:cites rdflibjs) and [rdfstore-js](cite:cites rdfstorejs)
are frameworks for handling RDF data and include a SPARQL engine API.
Jena is a Java framework, RDFLib is a python package, and rdflib.js and rdfstore-js are JavaScript modules.
Jena, or more specifically the ARQ API, and RDFLib are fully SPARQL 1.1 compliant.
rdflib.js and rdfstore-js both support a subset of SPARQL 1.1.
Furthermore, rdflib.js and rdfstore-js only support the Node.js environment, they can not be used from within a browser.
These SPARQL engines are not coupled to a specific datasource,
instead, in-memory models or other sources can be used, such as Jena TDB in the case of ARQ.
These SPARQL engine APIs are similar to the Comunica engine that we propose,
but they main difference is that they are not modular,
they do not support heterogeneous interfaces,
and they do not run natively in Web browsers.

### Software design patterns

In this section, we discuss three software design patterns that are relevant to the modular design of the Comunica engine.

#### Publish–subscribe pattern

The [_publish-subscribe_](cite:cites publishsubscribepattern) design pattern involves the passing of _messages_ between _publishers_ and _subscribers_.
Instead of publishers being programmed to send messages directly to subscribers, they are programmed to _publish_ messages to certain _categories_.
Subscribers can _subscribe_ to these categories which will cause them to receive these published messages, without requiring prior knowledge of the publishers.
This pattern is useful for decoupling software components from each other,
and only requiring prior knowledge of message categories.

#### Actor Model

The [_actor_ model](cite:cites actormodel) was designed as a way to achieve highly parallel systems consisting of many independent _agents_
communicating using messages --such as the publish-subscribe pattern-- where each actor performs a specific task.
An actor is a computational unit that acts on messages, and can send messages to other messages.
The main advantages of the actor model are that actors can be independently made to implement certain specific tasks based on messages,
and that these can be handled asynchronously.
These characteristics are highly beneficial to the modularity that we want to achieve with Comunica.

#### Mediator pattern

The [_mediator_](cite:cites mediatorpattern) pattern is able to _reduce coupling_ between software components that interact with each other,
and to easily _change the interaction_ if needed.
This can be achieved by encapsulating the interaction between software components in a mediator component.
Instead of the components having to interact with each other directly,
they now interact through the mediator.
These components therefore do not require prior knowledge of each other,
and different implementations of these mediators can lead to different interaction results.
In Comunica, we use this pattern to handle actions when multiple actors are able to solve the same task,
by for example choosing the _best_ actor for a task, or by combining the solutions of all actors.