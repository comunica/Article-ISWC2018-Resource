## Comparison with TPF client
{:#comparison-tpf-client}

The primary goal of Comunica is to replace the TPF client as a more *flexible* and *modular* alternative,
with at least the same *functionality* and similar *performance*.
The fact that Comunica supports multiple heterogeneous interfaces and sources as shown in the previous section
validates this flexibility and modularity, as the TPF client only supports querying over TPF entrypoints.
The current implementation of Comunica has all of the functionality of the TPF client, and supersedes it.
Finally, Comunica achieves similar performance compared to the TPF client, as will be shown hereafter.

### Performance

Next to a functional completeness, it is also desired that Comunica achieves similar *performance* compared to the TPF client.
The higher modularity of Comunica is however expected to cause performance overhead,
due to the additional bus and mediator communication, which does not exist in the TPF client.
Hereafter, we compare the performance of the TPF client and Comunica
and discover that Comunica in most cases outperforms the TPF client.
As the main goal of Comunica is modularity, and not performance, we do not compare with similar frameworks such as ARQ and RDFLib.

For the setup of this evaluation we made use of a single machine (Intel Core i5-3230M CPU at 2.60 GHz with 8 GB of RAM), running both Server and Client. The main goal of this evaluation was to determine the performance impact of the new implementation, while keeping all other variables constant. We used the following <a about="#evaluation-workflow" content="Comunica evaluation workflow" href="#evaluation-workflow" property="rdfs:label" rel="cc:license" resource="https://creativecommons.org/licenses/by/4.0/">workflow</a>:

<ol id="evaluation-workflow" property="schema:hasPart" resource="#evaluation-workflow" typeof="opmw:WorkflowTemplate" markdown="1">
<li id="workflow-data" about="#workflow-data" typeof="opmw:WorkflowTemplateProcess" rel="opmw:isStepOfTemplate" resource="#evaluation-workflow" property="rdfs:label" markdown="1">
  Generate a [WatDiv](cite:cites watdiv) dataset with scale factor=100.
</li>
<li id="workflow-queries" about="#workflow-queries" typeof="opmw:WorkflowTemplateProcess" rel="opmw:isStepOfTemplate" resource="#evaluation-workflow" property="rdfs:label" markdown="1">
  Generate the corresponding default WatDiv [queries](https://github.com/comunica/test-comunica/tree/master/sparql/watdiv-10M){:.mandatory} with query-count=5.
</li>
<li id="workflow-tpf-server" about="#workflow-tpf-server" typeof="opmw:WorkflowTemplateProcess" rel="opmw:isStepOfTemplate" resource="#evaluation-workflow" property="rdfs:label" markdown="1">
  Install [the server software configuration](https://linkedsoftwaredependencies.org/raw/ldf-availability-experiment-config.jsonld){:.mandatory}, implementing the [TPF specification](https://www.hydra-cg.com/spec/latest/triple-pattern-fragments/){:.mandatory}, with its [dependencies](https://linkedsoftwaredependencies.org/raw/ldf-availability-experiment-setup.ttl){:mandatory}.
</li>
<li id="workflow-tpf-client" about="#workflow-tpf-client" typeof="opmw:WorkflowTemplateProcess" rel="opmw:isStepOfTemplate" resource="#evaluation-workflow" property="rdfs:label" markdown="1">
  Install [the client software configuration](https://github.com/LinkedDataFragments/Client.js){:.mandatory}, implementing the [SPARQL 1.1 protocol](https://www.w3.org/TR/sparql11-protocol){:mandatory}, with its [dependencies](https://linkedsoftwaredependencies.org/raw/ldf-availability-experiment-client.ttl){:.mandatory}.
</li>
<li id="workflow-tpf-run" about="#workflow-tpf-run" typeof="opmw:WorkflowTemplateProcess" rel="opmw:isStepOfTemplate" resource="#evaluation-workflow" property="rdfs:label" markdown="1">
  Execute the generated WatDiv queries 3 times on the TPF client, after doing a warmup run, and record the execution times [results](https://github.com/comunica/test-comunica/blob/master/results/watdiv-ldf.csv).
</li>
<li id="workflow-comunica" about="#workflow-comunica" typeof="opmw:WorkflowTemplateProcess" rel="opmw:isStepOfTemplate" resource="#evaluation-workflow" property="rdfs:label" markdown="1">
  Install [the Comunica software configuration](https://github.com/comunica/comunica/blob/master/packages/actor-init-sparql/config/config-default.json){:.mandatory}, implementing the [SPARQL 1.1 protocol](https://www.w3.org/TR/sparql11-protocol){:mandatory}, with its [dependencies](){:.mandatory}.
  (TODO: Add dependencies)
</li>
<li id="workflow-comunica-run" about="#workflow-comunica-run" typeof="opmw:WorkflowTemplateProcess" rel="opmw:isStepOfTemplate" resource="#evaluation-workflow" property="rdfs:label" markdown="1">
  Execute the generated WatDiv queries 3 times on the Comunica client, after doing a warmup run, and record the execution times [results](https://github.com/comunica/test-comunica/blob/master/results/watdiv-comunica.csv).
</li>
</ol>

<figure id="performance-average">
<center>
<img src="img/avg.svg" alt="[performance-average]" class="plot">
<img src="img/avg_c23.svg" alt="[performance-average]" class="plot">
</center>
<figcaption markdown="block">
Average query evaluation times for the TPF client and Comunica for all queries.
C2 and C3 are shown separately because of their higher evaluation times.
</figcaption>
</figure>

The results from [](#performance-average) show that Comunica is able to achieve similar,
and in most cases even better performance than the TPF client.
Concretely, Comunica is faster for 16 queries, and slower for 4 queries.
Overall, Comunica has slightly better performance than the TPF client.
However, the difference in evaluation times is in most cases very small,
and are caused by implementation details, as the implemented algorithms are equivalent.
Contrary to our expectations, the performance overhead of Comunica's modularity is negligible.
Comunica therefore improves upon the TPF client in terms of *modularity*, *functionality* and *performance*.
