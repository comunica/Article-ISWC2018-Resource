## Conclusions
{:#conclusions}

In this work, we introduced Comunica as a highly _modular_ SPARQL engine for _federated_ _SPARQL query_ evaluation over _heterogeneous interfaces_.
Comunica is thereby first system that truly accomplishes the Linked Data Fragments vision of a client that is able to query over heterogeneous interfaces.
Not only can Comunica be used as a client-side SPARQL engine, it can also be customized to become a more lightweight engine and perform more specific tasks,
such as for example only evaluating BGPs over Turtle files,
evaluating the efficiency of different join operators,
or even serve as a complete server-side SPARQL query endpoint that aggregates different datasources.
In future work, we will look into supporting supporting alternative (non-semantic) query languages as well, such as [GraphQL](cite:cites graphql).

Since Comunica is built on Linked Data and Web technologies,
it is the ideal research platform for investigating new Linked Data publication interfaces,
and for experimenting with different querying algorithms.
Because Comunica is extensively documented and has a ready-to-use API,
it can not only be used by the research community,
but for example also by developers of RDF-consuming applications.

We invite future and past LDF researchers to implement their interface handlers and algorithms in Comunica,
so that the effects of _combining_ these features can be investigated, instead of only experimenting with them in isolation.
In the future, we will continue maintaining and developing Comunica and intend to support future researcher on this platform.
