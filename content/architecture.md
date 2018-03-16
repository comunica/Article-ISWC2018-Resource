## Architecture
{:#architecture}

In this section, we discuss the design and architecture of the Comunica engine.
In summary, Comunica is collection of small modules that, when wired together,
are able to perform a certain task, such as evaluating SPARQL queries.
We first discuss the main design patterns that are used within Comunica.
After that, we talk about the wiring of modules based on dependency-injection.
Finally, we give an overview of all modules.

### Actor-Mediator-Bus Pattern

The modules in comunica work together based on the [_actor_](cite:cites actormodel),
[_mediator_](cite:cites mediatorpattern) and [_publish-subscribe_](cite:cites publishsubscribepattern) patterns.
Any number of _actor_ modules can be created,
where each actor interacts with _mediators_, that in its turn invoke other actors that are registered to a certain _bus_.
_Actors_, _buses_ and _mediators_ form the three main categories of modules in Comunica.

[](#actor-mediator-bus) shows an example logic flow between actors through a mediator and a bus.
The relation between these components, their phases and the chaining of them will be explained hereafter.

<figure id="actor-mediator-bus">
<img src="img/actor-mediator-bus.svg" alt="[actor-mediator-bus pattern]">
<figcaption markdown="block">
Example logic flow where Actor 0 requires an action to be performed.
This is done by sending the action to the Mediator, which sends a test action to Actors 1, 2 and 3 via the Bus.
The test replies are then collected by the Mediator, and it chooses the best actor for the action, in this case Actor 3.
Finally, the Mediator sends the original action to Actor 3, and returns its response to Actor 0.
</figcaption>
</figure>

#### Relation between Actors and Buses

Actors are the main computational units in Comunica, and buses and mediators form the _glue_ that ties them together and makes them interactable.
Actors are responsible for being able to accept certain messages via the bus they are subscribed to and replying with an answer.
Separate buses exist for different message types.
[](#relation-actor-bus) shows an example of how actors can be registered to buses.

<figure id="relation-actor-bus">
<img src="img/relation-actor-bus.svg" alt="[relation between actors and buses]">
<figcaption markdown="block">
An example of two different buses each having two subscribed actors.
The left bus has different actors for parsing triples in a certain RDF serialization to triple objects.
The right bus has actors that join query bindings streams together in a certain way.
</figcaption>
</figure>

#### Mediators handle Actor Run and Test Phases

Each mediator is connected to a single bus, and its goal is to determine and invoke the *best* actor for a certain task.
The definition of '*best*' depends on the mediator, and different implementations can lead to different choices in different scenarios.
A mediator works in two phases: the _test_ phase and the _run_ phase.
The test phase is used to check which actors on the bus are _able to act_ on a certain type of message,
and if so, what are the conditions under which this action can be performed.
This phase must always come before the run phase, and is used to select which actor is best suited to perform a certain task under certain conditions.
If such an actor is determined, the run phase of a single actor is initiated.
This run phase takes this same type of message, and requires to _effectively act_ on this message,
and return the result of this action.
[](#run-test-phases) shows an example of a mediator invoking a run and test phase.

<figure id="run-test-phases">
<img src="img/run-test-phases.svg" alt="[mediators handle actor run and test phases]">
<figcaption markdown="block">
Example sequence diagram of a mediator that chooses the fastest actor
on a parse bus with two subscribed actors.
The first parser is very fast but requires a lot of memory,
while the second parser is slower, but requires less memory.
The mediator first calls the _tests_ the actors for the action, and then _runs_ the action using the _best_ actor.
</figcaption>
</figure>

#### Chaining of Actors

More complex actors can require the functionality of other types of actors.
Instead of letting actors call each other immediately, actors must call mediators instead,
as is shown in the interaction in [](#actor-mediator-bus).
This makes actors loosely coupled, as they only require a task to be solved by an external actor,
it does not matter _how_ the task was solved.
The '_how_' is chosen by the mediator.
The exact type of mediator can be passed to the actor during intialization,
so that different mediators can be configured depending on a configuration.

For example, a SPARQL query evaluation actor can require the functionality of actors that join streams of bindings.

### Dynamic Wiring

In order to make actors, mediators and buses loosely coupled and flexible to combine,
we use the concept of [_dependency injection_](cite:cites DependencyInjection)
to wire these modules together at runtime based on a configuration file.
We do this using the [Components.js](cite:cites componentsjs) JavaScript dependency injection framework,
This framework is based on semantic module descriptions and configuration files
using the [Object-Oriented Components ontology](cite:cites describingsoftwareconfigurations).

Using the Components.js framework, we semantically describe all actors, mediators and buses in [JSON-LD](cite:cites jsonld).
[](#config-actor) shows an example of the semantic description of an RDF parser.
The Comunica engine can be _initialized_ using Components.js configuration files,
that describe the wiring between actors, mediators and buses.
For example, [](#config-parser) shows a configuration file of an engine that is able to parse N3 and JSON-LD-based documents.
This example shows that Comunica is, next to its main purpose of being a query engine,
also able to be used for more specific tasks, such as building a custom RDF parser.

<figure id="config-actor" class="listing">
````/code/config-actor.json````
<figcaption markdown="block">
Semantic description of an actor that is able to parse N3-based RDF serializations.
This actor has a single parameter that allows media types to be registered that this parser is able to handle.
In this case, the actor has four default media types that can be overridden via the config file.
</figcaption>
</figure>

<figure id="config-parser" class="listing">
````/code/config-parser.json````
<figcaption markdown="block">
Comunica configuration of `ActorInitRdfParse` for parsing an RDF document in an unknown serialization.
This actor is linked to a mediator with a bus containing two RDF parsers for specific serializations.
</figcaption>
</figure>

While the primary goal of these configuration files is to initialize a Comunica engine,
these files can also be used as a semantic description of the engine.
This description can for example be referenced when linking benchmark results to the used engines in a machine-readable format.

### Modules

At the time of writing, comunica consists of 79 different modules.
This consists of 13 buses, 3 mediator types, 57 actors and 6 other modules.
In this section, we will only discuss the most important actors and their interactions.

The main bus in Comunica is the _query operation_ bus, which consists of 19 different actors
that implement the typical SPARQL operations such as quad patterns, basic graph patterns (BGPs), unions, projects, ...
These actors interact with each other using quad and bindings streams,
and act on a query plan in [SPARQL algebra](cite:cites spec:sparqllang).
By default, no query plan rewriting is done, and BGPs are resolved using the original [TPF algorithm](cite:cites ldf).

In order to enable heterogeneous sources to be queried in a federated way,
we allow a list of sources, annotated by type, to be passed when a query is initiated.
These sources are passed down through the chain of query operation actors,
until the quad pattern level is reached.
At this level, different actors exist for handling a single source of a certain type,
such as TPF entrypoints, SPARQL endpoints, local or remote datadumps.
In the case of multiple sources, one actor exists that implements the federation algorithm of the [TPF client](cite:cites ldf),
but instead of federating over different TPF entrypoints, it federates over different single-source quad pattern actors.

At the end of the pipeline, different actors are available for serializing the results of a query in different ways.
For instance, there are actors for serializing the results according to
the SPARQL [JSON](cite:cites spec:sparqljson) and [XML](cite:cites spec:sparqlxml) result specifications,
but actors with more visual and developer-friendly formats are available as well.