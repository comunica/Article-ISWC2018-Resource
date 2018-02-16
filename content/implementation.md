## Implementation
{:#implementation}

Comunica is implemented in TypeScript/JavaScript as a collection of Node modules.
Comunica is available under an open license on [GitHub](https://github.com/comunica/comunica){:.mandatory}
and on the [NPM package manager](https://www.npmjs.com/org/comunica){:.mandatory}.
The 72 Comunica modules are tested thoroughly, with more than 1000 unit tests reaching a test coverage of 100%.
In order to be compatible with existing JavaScript RDF libraries,
Comunica is fully compatible with the JavaScript API specification by the [RDFJS community group](https://www.w3.org/community/rdfjs/){:.mandatory}.
Finally, we provide detailed documentation for the usage and development with [Comunica](https://comunica.readthedocs.io){:.mandatory}.

We provide a default configuration file with all available actors for evaluating (federated) SPARQL queries over heterogeneous sources.
This allows SPARQL queries to be evaluated using a command line tool,
a webservice implementing the [SPARQL protocol](cite:cites spec:sparqlprot),
from within a JavaScript application,
or within the browser.
Just like the TPF client, we fully support [SPARQL 1.0](cite:cites spec:sparqllang1) and a subset of [SPARQL 1.1](cite:cites spec:sparqllang) at the time of writing.
In future work, we intend to implement additional actors for supporting SPARQL 1.1 completely.

TODO: preliminary evaluation
