## Introduction
{:#introduction}

{:.comment data-author="RV"}
The introduction still starts strongly from the interfaces perspective.
As per my earlier comment, I still think it should be like:
a query engine consists of many aspects—its algorithms,
its support for certain (existing and new) SPARQL features,
and its support for different kinds of sources.
Cue pointers to different query evaluation paradigms **on the Web**,
such as SPARQL endpoints, Linked Data traversal, TPF.
I'll draft out a couple of paragraph ideas below.

Linked Data on the Web exists in many shapes and forms—and
so do the processors we use to query data from one or multiple sources.
For instance,
engines that query RDF data using the [SPARQL language](cito:citesAsAuthority spec:sparqllang)
employ [_different algorithms_](cito:cites SparqlOptimization, SparqlSelectivityOptimization)
and support [_different language extensions_](cito:cites Erling2009, fSPARQL).
Furthermore,
Linked Data is increasingly published through _different Web interfaces_,
such as
data dumps, [Linked Data documents](cite:cites LinkedDataPrinciples),
[SPARQL endpoints](cite:cites spec:sparqlprot)
and [Triple Pattern Fragments (TPF) interfaces](cite:cites ldf).
This has lead to entirely different query evaluation strategies,
such as [server-side](cite:cites spec:sparqlprot),
client-side (by downloading data dumps and loading them locally),
[link-traversal-based](cite:cites linkeddataqueries),
and [shared client–server query processing](cite:cites ldf).

Different implementations for such querying techniques exist,
where many of the suggested improvements are implemented as _forks_ of existing software.
Few of these ever become part of the main software releases,
as it is often not trivial to harmonize them,
which is why they are almost never evaluated in _combination_ with each other.
For example, different federated querying algorithms exist,
and there are multiple querying algorithms for the different types of heterogeneous interfaces.
However, the combination of both, i.e., federated querying over heterogeneous interfaces, has not been accomplished yet.
Another example is the variety of [optimizations and extensions in the TPF framework](cite:cites brtpf, vtpf, tpfamf, tpfsubstring, tpfoptimization, cyclades, tpfqs)
that are only evaluated in isolation, whereas certain combinations could prove to be beneficial.

As such, in order to handle the increasing _heterogeneity_ of the Web,
and to _combine_ various existing and new querying algorithms,
there is therefore a need for a flexible and modular query engine
that provides a way to experiment with all of these techniques, both separately and in combination.

In this article, we introduce _Comunica_ as a query client that accomplishes this vision.
It is a highly _modular_ meta engine for _federated_ _SPARQL query_ evaluation over _heterogeneous interfaces_,
including TPF interfaces, SPARQL endpoints and data dumps.
Comunica aims to serve as a flexible research platform for experimenting with new Linked Data querying and publication techniques.

Comunica differs from existing query engines on different levels:

1. The **modularity** of the Comunica meta query engine allows for easy _extensions_ and _customization_ of algorithms and functionality. Furthermore, modules can be added and removed using an RDF configuration file, which makes it possible for users to build and finetune their own engine. Finally, these configurations can be published for full repeatability of experiments.
2. Within Comunica, multiple **heterogeneous** interfaces are a first-class citizen. This enables federated querying over heterogeneous sources and makes it for example possible to evaluate queries over any combination of SPARQL endpoints, TPF interfaces, datadumps, or other types of interfaces.
3. Comunica is implemented for the **Web** in JavaScript, which makes it possible to use it in a browser, from the command line, via the [SPARQL protocol](cite:cites spec:sparqlprot), or from any Web or JavaScript application.

Comunica and its default modules are publicly available
on GitHub and the npm package manager under the open-source MIT license.

This article is structure as follows:
In the next section, we discuss the related work, followed by the main features of Comunica in [](#features).
After that, we introduce the architecture of Comunica in [](#architecture), and its implementation in [](#implementation).
Next, we compare the performance of different Comunica configurations with the TPF client in [](#comparison-tpf-client).
Finally, [](#conclusions) concludes and contains opportunities for future work.
