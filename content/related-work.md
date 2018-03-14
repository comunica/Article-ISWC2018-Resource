## Related Work
{:#related-work}

In this section, we discuss the Linked Data Fragments framework,
followed by the state-of-the-art on modular SPARQL engines.
Finally, we discuss several software design patterns that are essential in the architecture of Comunica.

### Linked Data Fragments

A [Linked Data Fragment](cite:cites ldf) (LDF) has been defined as a type of response to a Web interface for an RDF-based knowledge graph.
The simplest type of LDF is a data dump---it is the response of a single HTTP requests for a complete RDF dataset.
If only a subset of a dataset is needed, then downloading a full data dump is not always the most efficient way.
Alternatively, SPARQL endpoints could be used to query specific parts of a dataset.
Therefore, the response of a SPARQL endpoint can also be considered an LDF.

Choosing an interface to publish a dataset involves a trade-off between server and client load for query evaluation.
On the one hand, a raw dataset dump can be published by a simple static file server,
but requires significant effort from clients to query them.
Before a dump can be queried, it needs to be downloaded first, and then loaded into a local SPARQL engine.

{:.comment data-author="MVS"}
rephrase next sentence

On the other hand, datasets can be published through a SPARQL endpoint,
which requires minimal effort from clients,
but now the server has to do all the work for evaluating the query.
The [Triple Pattern Fragments](cite:cites ldf) interface was introduced as a trade-off

{:.comment data-author="MVS"}
rephrase next part

between the flexibility and large server-load of SPARQL endpoints,
and the non-flexible, but low-cost publication of datadumps.

{:.comment data-author="MVS"}
rephrase sentence below

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

Comunica is _loosely-coupled_ to any RDF indexing structure,
but for the sake of completeness, we also mention _tightly-coupled_ SPARQL engines.

{:.comment data-author="MVS"}
I rewrote this part a bit. The discussion of RDF indexing (native/relational/NoSQL) is convoluted with the distinction between tight/loose coupling. Thar said, if this is all your going to say about tight-coupled SPARQL engines, I wouldn't mention them, and only talk about SPARQL frameworks.   

Such indexing structure can either be a native RDF store, e.g., [AllegroGraph](cite:cites allegrograph) and [Blazegraph](cite:cites blazegraph),
or a non-RDF store, e.g., [Virtuoso](cite:cites virtuoso) uses a Object-Relational Database Management System
and evaluates SPARQL queries by translating them into SQL.


The [Triple Pattern Fragments client](cite:cites ldf) is a client-side SPARQL engine 

{:.comment data-author="MVS"}
that not really accurate, it was designed to query LDF APIs, but didn't go beyond TPF yet.

that is decoupled from an RDF store.
It is able to retrieve data over HTTP from TPF entrypoints using triple pattern queries.
It can however not directly interact with other types of datasources.
Furthermore, the TPF client is built for the Web, as it can run in any JavaScript environment, such as the browser.

[Jena](cite:cites jena), [RDFLib](cite:cites rdflib), [rdflib.js](cite:cites rdflibjs) and [rdfstore-js](cite:cites rdfstorejs)
are frameworks for handling RDF data and include 

{:.comment data-author="MVS"}
What's a SPARQL engine API?

a SPARQL engine API.
Jena is a Java framework, RDFLib is a python package, and rdflib.js and rdfstore-js are JavaScript modules.
Jena, or more specifically the ARQ API, and RDFLib are fully SPARQL 1.1 compliant.
rdflib.js and rdfstore-js both support a subset of SPARQL 1.1.
Furthermore, rdflib.js and rdfstore-js only support the Node.js environment, they can not be used from within a browser.
These SPARQL engines are not coupled to a specific datasource,
instead, in-memory models or other sources can be used, such as Jena TDB in the case of ARQ.
These SPARQL engine APIs are similar to the Comunica engine that we propose,
but the main difference is that they are not modular,
they do not support heterogeneous interfaces,
and they do not run natively in Web browsers.

### Software design patterns

In the following, we discuss three software design patterns that are relevant to the modular design of the Comunica engine.

{:.comment data-author="MVS"}
- I'd add a figure of each pattern, like this: https://realtimeapi.io/wp-content/uploads/2017/09/pubsub-1.png
- the relation with Comunica should be made more clear for all of them


#### Publish–subscribe pattern

The [_publish-subscribe_](cite:cites publishsubscribepattern) design pattern involves the passing of _messages_ between _publishers_ and _subscribers_.
Instead of publishers being programmed to send messages directly to subscribers, they are programmed to _publish_ messages to certain _categories_.
Subscribers can _subscribe_ to these categories which will cause them to receive these published messages, without requiring prior knowledge of the publishers.
This pattern is useful for decoupling software components from each other,
and only requiring prior knowledge of message categories.

#### Actor Model

The [_actor_ model](cite:cites actormodel) was designed as a way to achieve highly parallel systems consisting of many independent _agents_
communicating using messages, similar to the publish-subscribe pattern.
An actor is a computational unit that performs a specific task, acts on messages, and can send messages to other messages.
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
