## Introduction
{:#introduction}

{:.comment data-author="MVS"}
I think the build up should be like: Linked Data publishing -> many interfaces -> LDF -> embrace Web and heterogeneity -> modular client in JS. Now, it is written from the old limited client perspective, as we experienced it, but we shouldn't sell it that way.

{:.comment data-author="RV"}
And I think it should be like:
a query engine consists of many aspects—its algorithms,
its support for certain (existing and new) SPARQL features,
and its support for different kinds of sources.
Cue pointers to different query evaluation paradigms **on the Web**,
such as SPARQL endpoints, Linked Data traversal, TPF.
Explain that many of the suggested improvements are implemented as forks,
few of which ever make it to master,
and almost none of which are ever evaluated in combination.
Case in point: nobody has done federation over heterogeneous interfaces.
It's hard to make such combinations, so it rarely happens—imagine
all the data points we're missing!
Yippee ki-yay Semantic Web community,
that's exactly what we're bringing to the table:
a modular query engine as a research experimentation platform that does all of this.

<del class="comment" data-author="RV">
The number of datasets that are available in the [Linked Open Data](cite:cites linkeddata) cloud is [continuously increasing](cite:cites linkeddataadoption).
As of February 2018, the [LODStats project](cite:cites lodstats) reports 2,973 available datasets containing more than 149 billion triples.
</del>
This Linked Data is being published in different ways, such as
data dumps and [SPARQL query endpoints](cite:cites spec:sparqlprot).

These different techniques for publishing Linked Data are also known as [Linked Data Fragments](cite:cites ldf) (LDFs).
The LDF framework introduces an axis for comparing the load distribution between server and client load of Linked Data publication techniques when evaluating queries.
[Triple Pattern Fragments](cite:cites ldf) (TPF) was a first low-cost server interface on this axis
that was introduced as an attempt to solve the [major availability issues](cite:cites sparqlreadyforaction) when publishing Linked Data through SPARQL endpoints,
by moving part of [SPARQL query evaluation](cite:cites spec:sparqllang) to the client side.
This alternative Linked Data publication method has been receiving increasing attention within the research community,
including [server interface extensions](cite:cites brtpf, vtpf, tpfamf, tpfsubstring) to [client-side optimizations and extensions](cite:cites tpfoptimization, cyclades, tpfqs).
Most of these works include an adaptation of the TPF client (also known as Client.js or `ldf-client`),
which is the default implementation of
the TPF algorithm for federated evaluation of SPARQL queries using TPF entrypoints.

{:.comment data-author="RV"}
To much history; focus on the current state of the Web.
No clear winner, never will be.

These client adaptations clients are however not fully compatible with each other.
They are implemented as different diverged _forks_ of the original client,
and it is not trivial to harmonize them.
This is because the TPF client is too dedicated to TPF entrypoints,
i.e., it was not designed with extensions or adaptations in mind.
Therefore, the TPF client is not able to achieve the complete LDF vision of a client that embraces the heterogeneity of interfaces,
and there is a need for a more flexible client in which modules can be plugged to support different types of interfaces or query algorithms.

{:.comment data-author="RV"}
Forks argument is good, but it's not just for interfaces.
Sell this much more broadly.
At the moment, it reads like you're solving our and Olaf's problem,
but we're solving something much broader.

In this article, we introduce _Comunica_ as a query client that truly accomplishes this LDF vision.
It is a highly _modular_ meta engine for _federated_ _SPARQL query_ evaluation over _heterogeneous interfaces_,
including TPF entrypoints, SPARQL endpoints and data dumps.
Comunica thereby positions itself as the successor of the TPF client,
with the aim to serve as a flexible research platform for experimenting with new Linked Data querying and publication techniques.

{:.comment data-author="RV"}
Same here, not just LDF.

Comunica differs from existing query engines on different levels:

1. The **modularity** of the Comunica meta query engine allows for easy _extensions_ and _customization_ of algorithms and functionality. Furthermore, modules can be added and removed using an RDF configuration file, which makes it possible for users to build and finetune their own engine.
<span class="comment" data-author="RV">Yeah, and those configurations can be published too. Full repeatability of an exact configuration. More than repeatability: just take an existing config, add in your algorithm, and you can compare.</span>
2. Within Comunica, multiple **heterogeneous** interfaces are a first-class citizen. This enables federated querying over heterogeneous sources and makes it for example possible to evaluate queries over any combination of SPARQL endpoints, TPF entrypoints, datadumps, or other types of interfaces.
3. Comunica is implemented for the **Web** in JavaScript, which makes it possible to use it in a browser, from the command line, via the [SPARQL protocol](cite:cites spec:sparqlprot), or from any Web or JavaScript application.

Comunica and its default modules are publicly available
on GitHub and the npm package manager under the open-source MIT license.

This article is structure as follows:
In the next section, we discuss the related work, followed by the architecture of Comunica in [](#architecture), and its implementation in [](#implementation).
After that, we discuss its differences with related frameworks in [](#features) and the TPF client in [](#comparison-tpf-client).
Finally, [](#conclusions) concludes and contains opportunities for future work.
