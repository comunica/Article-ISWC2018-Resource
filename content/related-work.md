## Related Work
{:#related-work}

In this section, we discuss the Linked Data Fragments framework,
followed by the state-of-the-art on modular SPARQL engines.
Finally, we discuss several software design patterns that are essential in the architecture of Comunica.

{:.comment data-author="RV"}
No no, start the first section with
the SPARQL languages, query engines, and algorithms,
and end that with federation,
and traversal-based query evaluation.
Then a section about TPF and others.
The point of RelWork should be:
so many degrees of freedom in a SPARQL query processor,
and most of them were never even combined.

### Linked Data Fragments

<ins class="comment" data-author="RV">
In order to formally capture the heterogeneity of different Web interfaces to publish RDF data,
</ins>
A [Linked Data Fragment](cite:cites ldf) (LDF) has been defined as a type of response to a Web interface for an RDF-based knowledge graph.
The simplest type of LDF is a _data dump_---it is the response of a single HTTP requests for a complete RDF dataset.
If only a subset of a dataset is needed, then downloading a full data dump is not always the most efficient way.
Alternatively, _SPARQL endpoints_ could be used to query specific parts of a dataset.
Therefore, the response of a SPARQL endpoint can also be considered an LDF.
<span class="comment" data-author="RV">…by definition. I'd rephrase that.</span>

{:.comment data-author="RV"}
Refs missing.

Choosing an interface to publish a dataset involves a trade-off between server and client load for query evaluation.
On the one hand, a raw dataset dump can be published by a simple static file server,
but requires significant effort from clients to query them.
Before a dump can be queried, it needs to be downloaded first, and then loaded into a local SPARQL engine.
Datasets can alternatively be published though a SPARQL endpoint,
which only requires the client to send a query to the server over HTTP after which the server will reply with results.
SPARQL endpoints however require the server to do all the work for evaluating queries.
The [Triple Pattern Fragments](cite:cites ldf) interface was introduced as a trade-off
between data dumps and SPARQL endpoints, where both client and server share the effort of evaluating queries.

{:.comment data-author="RV"}
We're again focusing too much about history here.
Look at the Web as it currently is:
RDF in different interfaces,
each of which has a good reason of existence (trade-offs).
So the question is no longer: which is the best interface,
but rather, how do we deal with that heterogeneity?

When choosing a type of LDF to publish a dataset, there is no silver bullet,
i.e., there is no single interface that works well in all situations, as each one involves trade-offs.
<span class="comment" data-author="RV">Okay good, you're getting into that story now 😉 Maybe earlier then? Or maybe we don't need the previous paragraph at all?</span>
As such, data publishers must choose the type of interface that matches their intended use case, target audience and infrastructure.
This however complicates client-side engines that are supposed to retrieve data from the resulting heterogeneity of interfaces.
As shown by the TPF approach, interfaces can be _self-descriptive_ and expose one or more [_features_](cite:cites webapifeatures),
i.e., they describe their functionality using a [common vocabulary](cite:cites hydra,describingapiresponses).
This allows clients without prior knowledge of the exact inputs and outputs of an interface
to discover its _controls_ and use it to retrieve data.

Up until now, no approaches exist that are able to query over interfaces in a heterogeneous manner,
which is a problem we aim to solve with Comunica.

### SPARQL frameworks

Comunica is a SPARQL framework that is _loosely-coupled_ to any RDF indexing structure,
as opposed to _tightly-coupled_ SPARQL engines such as
[AllegroGraph](cite:cites allegrograph), [Blazegraph](cite:cites blazegraph), and [Virtuoso](cite:cites virtuoso).
For the remainder of this section, we only focus on other loosely-coupled SPARQL frameworks.

{:.comment data-author="RV"}
Mmm no, explain these as engines that are intended to run on a server,
closely to the data.
And I wouldn't just go over them in one sentence.

The [Triple Pattern Fragments client](cite:cites ldf) is a client-side SPARQL engine 

{:.comment data-author="MVS"}
that not really accurate, it was designed to query LDF APIs, but didn't go beyond TPF yet.

{:.comment data-author="RT"}
True, but I think we should explain it as it is, not as it was intended.

{:.comment data-author="RV"}
That's what Miel is saying, right?
I think he was just opposing <q>client-side SPARQL engine</q>,
but I am fine with it
(it is a SPARQL engine on top of a specific data source,
just like Virtuoso is [but Jena less so]).

that is decoupled from an RDF store.
It is able to retrieve data over HTTP from TPF interfaces using triple pattern queries.
It can however not directly interact with other types of datasources.
Furthermore, the TPF client is built for the Web, as it can run in any JavaScript environment, such as the browser.

[Jena](cite:cites jena), [RDFLib](cite:cites rdflib), [rdflib.js](cite:cites rdflibjs) and [rdfstore-js](cite:cites rdfstorejs)
are frameworks for handling RDF data and include an API for evaluating SPARQL queries.
Jena is a Java framework, RDFLib is a python package, and rdflib.js and rdfstore-js are JavaScript modules.
Jena, or more specifically the ARQ API, and RDFLib are fully SPARQL 1.1 compliant.
rdflib.js and rdfstore-js both support a subset of SPARQL 1.1.
<del class="comment" data-author="RV">
Furthermore, rdflib.js and rdfstore-js only support the Node.js environment, they can not be used from within a browser.
</del>
<span class="comment" data-author="RV">Incorrect; they were designed for the browser first.</span>
These SPARQL engines are not coupled to a specific datasource,
instead, in-memory models or other sources can be used, such as Jena TDB in the case of ARQ.
These SPARQL engine APIs are similar to the Comunica engine that we propose,
but the main difference is that they are not modular,
<span class="comment" data-author="RV">Rather, we propose a wholly different level of modularity. Try swapping out a query algorithm in them.</span>
they do not support heterogeneous interfaces,
<del class="comment" data-author="RV">
and they do not run natively in Web browsers.
</del>
These differences will be explained in more detail in [](#features).

### Software design patterns

{:.comment data-author="RV"}
This section can go (or be substantially shortened) if space is short.

In the following, we discuss three software design patterns that are relevant to the modular design of the Comunica engine.

{:.comment data-author="MVS"}
- I'd add a figure of each pattern, like this: https://realtimeapi.io/wp-content/uploads/2017/09/pubsub-1.png

{:.comment data-author="RV"}
Probably no space though?

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
