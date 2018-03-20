## Features
{:#features}

In this section, we discuss the main requirements and features of the Comunica framework,
with the aim of being a flexible and easy-to-use successor of the TPF client for the Semantic Web research community.
Furthermore, we discuss each feature based on the availability in related work.
The main feature requirements of Comunica are the following:

* **SPARQL query evaluation**: The engine should be able to understand, process and output results for SPARQL queries.
* **Modular**: Different independent modules should contain the implementation of specific tasks, and they should be combinable in a flexible framework.
* **Heterogeneous interfaces**: Different types of datasource interfaces should be supported, and it should be possible to add new types independently.
* **Web-based**: The engine should be able to run in Web browsers using native Web technologies.

In [](#features-comparison), we summarize the availability of these features in similar works.

<figure id="features-comparison" class="table" markdown="1">

| Feature                  | TPF-client | ARQ | RDFLib | rdflib.js | rdfstore-js | Comunica |
| ------------------------ |------------|-----|--------|-----------|-------------|----------|
| Modular                  |            |     |        |           |             | ✓        |
| SPARQL                   | ✓*         | ✓   | ✓      | ✓*        | ✓*          | ✓*       |
| Heterogeneous interfaces |            |     |        |           |             | ✓        |
| Web-based                | ✓          |     |        | ✓         | ✓           | ✓        |

<figcaption markdown="block">
Comparison of the availability of the main features of Comunica in similar works.
(*) Only a subset of SPARQL 1.1 is supported.
</figcaption>
</figure>

### Modular

Adding new functionality or changing certain operations in Comunica requires minimal to no changes to existing code.
Furthermore, the Comunica environment is developer-friendly, including well documented APIs and auto-generation of stub code.
In order to take full advantage of the Linked Data stack, modules in Comunica are describable, configurable and wireable in RDF.
Comunica's modular architecture will be explained further in [](##architecture).
By registering or excluding modules from a configuration file, the user is free to choose how heavy or lightweight the query engine will be.
ARQ, RDFLib, rdflib.js and rdfstore-js only support customization by implementing a custom query engine programmatically to handle operators.
They do not allow plugging in or out certain modules.

### SPARQL query evaluation

At the time of writing, the TPF-client, rdflib.js, rdfstore-js, and Comunica support SPARQL 1.0, and a subset of SPARQL 1.1.
ARQ and RDFLib are fully SPARQL 1.1 compliant.

### Heterogeneous interfaces

Due to the existence of different types of Linked Data Fragments for exposing Linked Datasets,
Comunica aims to support _heterogeneous_ interfaces types.
Next to support for querying over TPF interfaces, as is supported by the TPF client,
Comunica also enables combined federated querying over other sources,
such as SPARQL endpoints and data dumps in RDF serializations.
ARQ, RDFLib, rdflib.js and rdfstore-js only support federation over SPARQL endpoints using the SERVICE keyword.
The TPF client only supports federation over TPF interfaces.

### Web-based

Comunica is built using native Web technologies, such as JavaScript and RDF configuration files.
Comunica is able to run in different kinds of environments, including Web browsers, local (JavaScript) runtime engines and command-line interfaces,
just like the TPF-client.
ARQ, RDFLib, rdflib.js and rdfstore-js are able to run in their language's runtime and via a command-line interface, but not from within Web browsers.
ARQ would be able to run in browsers using a custom Java applet, which is not a native Web technology.
