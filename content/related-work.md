## Related Work
{:#related-work}

In this section, we illustrate the many degrees of freedom when it comes to SPARQL querying,
and show that they are hard to combine, which is precisely the problem we aim to solve with Comunica.
We first discuss the SPARQL query language, its engines and algorithms.
After that, we discuss alternative Linked Data publishing interfaces, and their link to querying.
Finally, we discuss several software design patterns that are essential in the architecture of Comunica.

### The Different Facets of SPARQL

[SPARQL](cite:cites spec:sparqllang) is the W3C-recommended RDF query language.
This language is implemented in different SPARQL query engines
that are mostly intended to run on a server close to the data,
as they are tied to specific internal storage techniques using various [query algorithms](cite:cites linkeddataqueries).
Their internal storage can either be a native RDF store, e.g., [AllegroGraph](cite:cites allegrograph) and [Blazegraph](cite:cites blazegraph),
or a non-RDF store, e.g., [Virtuoso](cite:cites virtuoso) uses a Object-Relational Database Management System
and evaluates SPARQL queries by translating them into SQL.

In order to evaluate SPARQL queries over datasets that are not necessarily attached to a specific type of datasource,
different SPARQL query frameworks were developed, such as
[Jena (ARQ)](cite:cites jena), [RDFLib](cite:cites rdflib), [rdflib.js](cite:cites rdflibjs) and [rdfstore-js](cite:cites rdfstorejs).
Jena is a Java framework, RDFLib is a python package, and rdflib.js and rdfstore-js are JavaScript modules.
Jena, or more specifically the ARQ API, and RDFLib are fully [SPARQL 1.1](cite:cites spec:sparqllang) compliant.
rdflib.js and rdfstore-js both support a subset of SPARQL 1.1.
These SPARQL engines are not coupled to a specific datasource,
instead, in-memory models or other sources can be used, such as Jena TDB in the case of ARQ.
Most of the query algorithms in these frameworks are tightly coupled to each other,
which makes swapping out query algorithms for specific query operators hard or sometimes even impossible.
Furthermore, these frameworks do not support federated querying over heterogeneous interfaces out-of-the-box.
This issue of modularity and heterogeneity are two of the main problems we aim to solve within Comunica.
The differences between Comunica and existing frameworks will be explained in more detail in [](#features).

The [Triple Pattern Fragments client](cite:cites ldf) (also known as Client.js or `ldf-client`) is a client-side SPARQL engine
that is able to retrieve data over HTTP from [Triple Pattern Fragments (TPF) interfaces](cite:cites ldf) using triple pattern queries.
[Different algorithms](cite:cites tpfoptimization, cyclades, tpfqs) for this client and
[TPF interface extensions](cite:cites brtpf, vtpf, tpfamf, tpfsubstring) have been proposed that reduce effort of server or client in some way.
All of these efforts are however implemented and evaluated in isolation,
which causes us to potentially loose useful insights regarding certain combinations.
Furthermore, the implementations are tied to TPF interface, which makes it impossible to use them for other types of datasources and interfaces.
With Comunica, we aim to solve this by modularizing query operation implementations into specific modules,
so that they can be plugged in and combined in different ways, on top of different datasources and interfaces.

Due to the ever-increasing number of available Linked Datasets,
_federated query processing_ has been an active area of research.
However, most of the existing frameworks work in the context of SPARQL endpoints,
where [multiple endpoints are combined in a client-server architecture](cite:cites sparqlfederation).
The TPF client is different in that sense, as it federates over TPF entrypoints instead of SPARQL endpoints.
However, no frameworks exist yet that enable federation over heterogeneous interfaces,
such as the federation over any combination of SPARQL endpoints and TPF entrypoints.
With Comunica, we also aim to fill this gap.

{:.comment data-author="RT"}
Not too sure about the following, as we don't do it in comunica at the moment.

Next to RDF index-centric approaches like SPARQL endpoints and TPF entrypoints,
alternative methodologies such as [link traversal-based query evaluation](cite:cites linktraversal) exist.
Traversal-based querying is based around the concept of following links within Linked Data documents,
which is useful for live exploration within Linked Data,
but it leads to slower query evaluation and completeness can not be guaranteed.

### Linked Data Fragments

In order to formally capture the heterogeneity of different Web interfaces to publish RDF data,
a [Linked Data Fragment](cite:cites ldf) (LDF) has been defined as a type of response to a Web interface for an RDF-based knowledge graph.
The simplest type of LDF is a _data dump_---it is the response of a single HTTP requests for a complete RDF dataset.
If only a subset of a dataset is needed, then downloading a full data dump is not always the most efficient way.
Alternatively, [_SPARQL endpoints_](cite:cites spec:sparqlprot) can be used to query _specific parts_ of a dataset,
as SPARQL query responses are also a type of LDF.
Furthermore, the [Triple Pattern Fragments](cite:cites ldf) interface was introduced as a trade-off
between data dumps and SPARQL endpoints, where both client and server share the effort of evaluating queries.

When choosing a type of LDF to publish a dataset, there is no silver bullet,
i.e., there is no single interface that works well in all situations, as each one involves trade-offs.
As such, data publishers must choose the type of interface that matches their intended use case, target audience and infrastructure.
This however complicates client-side engines that are supposed to retrieve data from the resulting heterogeneity of interfaces.
As shown by the TPF approach, interfaces can be _self-descriptive_ and expose one or more [_features_](cite:cites webapifeatures),
i.e., they describe their functionality using a [common vocabulary](cite:cites hydra,describingapiresponses).
This allows clients without prior knowledge of the exact inputs and outputs of an interface
to discover its _controls_ and use it to retrieve data.

Up until now, no approaches exist that are able to query over interfaces in a heterogeneous manner,
which is a problem we aim to solve with Comunica.

### Software Design Patterns

{:.comment data-author="RV"}
This section can go (or be substantially shortened) if space is short.

In the following, we discuss three software design patterns that are relevant to the modular design of the Comunica engine.

#### Publish–subscribe pattern

The [_publish-subscribe_](cite:cites publishsubscribepattern) design pattern involves the passing of _messages_ between _publishers_ and _subscribers_.
Instead of publishers being programmed to send messages directly to subscribers, they are programmed to _publish_ messages to certain _categories_.
Subscribers can _subscribe_ to these categories which will cause them to receive these published messages, without requiring prior knowledge of the publishers.
This pattern is useful for decoupling software components from each other,
and only requiring prior knowledge of message categories.
We use this pattern in Comunica for allowing different implementations of certain tasks to subscribe to task-specific buses.

#### Actor Model

The [_actor_ model](cite:cites actormodel) was designed as a way to achieve highly parallel systems consisting of many independent _agents_
communicating using messages, similar to the publish-subscribe pattern.
An actor is a computational unit that performs a specific task, acts on messages, and can send messages to other messages.
The main advantages of the actor model are that actors can be independently made to implement certain specific tasks based on messages,
and that these can be handled asynchronously.
These characteristics are highly beneficial to the modularity that we want to achieve with Comunica.
That is why we use this pattern in combination with the publish-subscribe pattern to let each implementation of a certain task correspond to a separate actor.

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
