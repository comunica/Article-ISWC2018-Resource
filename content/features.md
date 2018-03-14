## Features
{:#features}

In this section, we discuss the main features of the Comunica engine,

{:.comment data-author="MVS"}
It's a resources paper, so it's already made

with the aim of making it a flexible and easy-to-use successor of the TPF client for the Semantic Web research community.
Furthermore, we discuss each feature based on the availability in related work.

The following main features of Comunica will be discussed and summarized hereafter:

{:.comment data-author="MVS"}
- Explain these briefly, for instance by using first sentence of each subsection
- the general explanation could use more depth

* **SPARQL query evaluation**
* **Modular**
* **Heterogeneous interfaces**
* **Environment-agnostic**

In [](#features-comparison), we summarize the availability of these features in similar works.

<figure id="features-comparison" class="table" markdown="1">

| Feature                  | TPF-client | ARQ | RDFLib | rdflib.js | rdfstore-js | Comunica |
| ------------------------ |------------|-----|--------|-----------|-------------|----------|
| SPARQL                   | ✓*         | ✓   | ✓      | ✓*        | ✓*          | ✓*       |
| Modular                  |            |     |        |           |             | ✓        |
| Heterogeneous interfaces |            |     |        |           |             | ✓        |
| Environment-agnostic     | ✓          |     |        |           |             | ✓        |

<figcaption markdown="block">
Comparison of the availability of the main features of Comunica in similar works.
(*) Only a subset of SPARQL 1.1 is supported.
</figcaption>
</figure>

### SPARQL query evaluation

The most important feature of a SPARQL engine is its ability to evaluate SPARQL queries.
At the time of writing, the TPF-client, rdflib.js, rdfstore-js, and Comunica support SPARQL 1.0, and a subset of SPARQL 1.1.
ARQ and RDFLib are fully SPARQL 1.1 compliant.

### Modular

Adding new functionality or changing certain operations in Comunica requires minimal to no changes to existing code.
Furthermore, the Comunica environment is developer-friendly, including well documented APIs and auto-generation of stub code.
In order to take full advantage of the Linked Data stack, modules in Comunica are describable, configurable and wireable in RDF.
By registering or excluding modules from a configuration file, the user is free to choose how heavy or lightweight the query engine will be.
ARQ, RDFLib, rdflib.js and rdfstore-js only support customization by implementing a custom query engine programmatically to handle operators.
They do not allow plugging in or out certain modules.

### Heterogeneous interfaces

Next to support for querying over TPF entrypoints, Comunica also enables combined federated querying over other sources,
such as SPARQL endpoints and data dumps in RDF serializations.
ARQ, RDFLib, rdflib.js and rdfstore-js only support federation over SPARQL endpoints using the SERVICE keyword.
The TPF client only supports federation over TPF entrypoints.

### Environment-agnostic

Comunica is able to run in different kinds of environments, including Web browsers, local (JavaScript) runtime engines and command-line interfaces,
just like the TPF-client.
ARQ, RDFLib, rdflib.js and rdfstore-js are able to run in their language's runtime and via a command-line interface, but not from within Web browsers.
ARQ would be able to run in browsers using a custom Java applet, which is not a native Web technology.
