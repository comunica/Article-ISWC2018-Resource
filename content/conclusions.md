## Conclusions
{:#conclusions}

In this work, we introduced Comunica as a highly modular meta engine for federated SPARQL query evaluation over heterogeneous interfaces.
Comunica is thereby first system that accomplishes the Linked Data Fragments vision of a client that is able to query over heterogeneous interfaces.
<span class="comment" data-author="RV">So we will need to soften the previous sentence if we don't demonstrate.</span>
Not only can Comunica be used as a client-side SPARQL engine, it can also be customized to become a more lightweight engine and perform more specific tasks,
such as for example only evaluating BGPs over Turtle files,
evaluating the efficiency of different join operators,
or even serve as a complete server-side SPARQL query endpoint that aggregates different datasources.
In future work, we will look into supporting supporting alternative (non-semantic) query languages as well, such as [GraphQL](cite:cites graphql).

Since Comunica is built on Linked Data and Web technologies,
it is the ideal research platform for investigating new Linked Data publication interfaces,
and for experimenting with different query algorithms.
Because Comunica is extensively documented and has a ready-to-use API,
it can not only be used by the research community,
but for example also by developers of RDF-consuming applications.

<span class="comment" data-author="RV">Yes, but phrase your sentences with researcher/people as subjects, i.e., involve the reader, draw them in. Also, emphasize again that they just need to build new independent modules, not fork or anything. And that the DI just wires it together.</span>

We invite future and past LDF researchers to implement their interface handlers and algorithms in Comunica,
<span class="comment" data-author="RV">Maybe say that we will also do that, to not just put the work on others</span>
so that the effects of _combining_ these features can be investigated, instead of only experimenting with them in isolation.
In the future, we will continue maintaining and developing Comunica and intend to support future researchers on this platform.
<span class="comment" data-author="RV">There's a possibility of ending more strongly: now there's a query engine experimentation platform, this will spawn tons of new research, and expose promising areas that remained covered so far.</span>
