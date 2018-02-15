## Architecture
{:#architecture}

In this section, we discuss the design and architecture of the Comunica engine.
In summary, Comunica is collection of small modules that, when wired together,
are able to perform a certain task, such as evaluating SPARQL queries.
We first discuss the main design patterns that are used within Comunica.
After that, we talk about the wiring of modules based on dependency-injection.
Finally, we give an overview of all modules

### Actor-Bus-Mediator Pattern

The modules in comunica work together based on the [_actor_](cite:cites actormodel),
[_publish-subscribe_](cite:cites publishsubscribepattern) and [_mediator_](cite:cites mediatorpattern) patterns.
Any number of _actor_ modules can be created,
where each actor interact with _mediators_, that in its turn invoke other actors that are registered to a certain _bus_.
_Actors_, _buses_ and _mediators_ form the three main categories of modules in Comunica.

Actors are the main computational units in Comunica, and buses and mediators form the _glue_ that ties them together and makes them interactable.
They are responsible for being able to accept certain messages via the bus they are subscribed to and replying with an answer.
Separate buses exist for different message types.
For example, a bus can exist with multiple registered actors for parsing triples in a certain RDF serialization to triple objects,
or another bus can contain actors that join query bindings together in a certain way.

Each actor handles messages in two phases: the _test_ phase and the _run_ phase.
The test phase is used to check if the actor is _able to act_ on a certain type of message,
and if so, what are the conditions under which this action can be performed.
This phase must always come before the run phase, and is used to select which actor is best suited to perform a certain task under certain conditions.
If such an actor is determined, the run phase of a single actor is initiated.
This run phase takes this same type of message, and requires to _effectively act_ on this message,
and return the result of this action.
For example, in the case of RDF parsers, the test phase could be used to check the time and memory requirements of a certain parser.
One parser could be very fast but require a lot of memory,
while another parser could be slower, but require less memory.
Under certain conditions, the optimal parser could be selected based on these parameters.

More complex actors can require the functionality of other types of actors.
For example, a SPARQL query evaluation actor can require the functionality of actors that join streams of bindings.
In order to avoid tight-coupling between such actors, a certain type of _mediator_ is called with a message instead.
Each mediator is linked with a single bus.
When a mediator receives a message, it is responsible for replying this this message by using the actors in some way from the registered bus.
For example, a mediator could exist over the RDF parsers bus and be programmed to always pick the parser that will perform a certain task the fastest.
When this mediator receives a message, it has to send the message to all actors in the bus to invoke their test phases.
Based on their responses, the mediator could determine the actor with the lowest time requirements.
After that, the mediator invokes the run phase of this actor, and returns its response.
More complex mediators could exist that take combine responses of certain responses.

TODO: add example figure showing a small workflox (test+run), clarify that run skips the bus

### Dynamic Wiring

TODO: stuff with componentsjs

### Modules

TODO: overview of the available actors, buses and mediators
