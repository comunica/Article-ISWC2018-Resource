## Features
{:#features}

In this section, we discuss the main features of the Comunica engine,
with the aim of making it a flexible and easy-to-use successor of the TPF client for the Semantic Web research community.

* **SPARQL query evaluation**: SPARQL queries should be evaluatable.
* **Modular**: Adding new functionality or changing certain operations requires minimal to no changes to existing code. Furthermore, the Comunica environment is developer-friendly, including well documented APIs and auto-generation of stub code. New modules are registrable via a configuration file.
* **Succession of the TPF client**: Comunica has at least all of the functionality of the TPF client, which means that it supports client-side SPARQL querying over one or more TPF entrypoints. In this work, we will show that Comunica is not significantly slower than the TPF client for SPARQL query evaluation over TPF entrypoints.
* **Heterogeneous sources**: Next to support for querying over TPF entrypoints, Comunica is also enable combined federated querying over other sources, with at least support for SPARQL endpoints and data dumps in RDF serializations.
* **Environment-agnostic**: Comunica is able to run in different kinds of environments, including Web browsers, local (JavaScript) runtime engines and command-line interfaces.
* **Linked Data-based**: In order to take full advantage of the Linked Data stack, modules are describable, configurable and wireable in RDF.

In [](#features-comparison), we compare the availability of these features in similar works.
ARQ only partially adheres to the _heterogeneous sources_ feature as defined in this section.
While ARQ supports different types of sources to be configured based on a Java interface,
ARQ does not allow federation over these different sources, it only allows federation over SPARQl endpoints.
Furthermore, ARQ also only partially adheres to the _environment-agnostic_ feature.
That is because running ARQ in a browser would require a custom Java applet,
which is not a native Web technology.
The TPF-client and Comunica on the other hand support running in browsers out-of-the-box.

<figure id="features-comparison" class="table" markdown="1">

| Feature               | TPF-client | ARQ | Comunica |
| --------------------- |------------|-----|----------|
| SPARQL                | ✓*         | ✓   | ✓*       |
| Modular               |            |     | ✓        |
| TPF-client+           | ✓          |     | ✓        |
| Heterogeneous sources |            | ~   | ✓        |
| Environment-agnostic  | ✓          | ~   | ✓        |
| Linked Data-based     | ✓          |     | ✓        |

<figcaption markdown="block">
Comparison of the availability of the main features of Comunica in similar works.
(*) At the time of writing, the TPF-client and Comunica support SPARQL 1.0, and a subset of SPARQL 1.1.
</figcaption>
</figure>
