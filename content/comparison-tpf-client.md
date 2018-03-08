## Comparison with TPF client
{:#comparison-tpf-client}

The primary goal of Comunica is to replace the TPF client as a more flexible and modular alternative.
The current implementation of Comunica has all of the functionality of the TPF client, and supersedes it.
The fact that Comunica supports multiple heterogeneous interfaces and sources as shown in the previous section
validates this flexibility, as the TPF client only supports querying over TPF entrypoints.

Next to a functional completeness, it is also desired that Comunica achieves similar performance compared to the TPF client.
The higher modularity of Comunica is however expected to cause performance overhead.
Hereafter, we compare the performance of the TPF client and Comunica
to discover this overhead.
As the main goal of Comunica is modularity, and not performance, we do not compare with similar frameworks such as ARQ and RDFLib.

### Performance

TODO: discuss setup, exact versions (LSD-way) and configuration

<figure id="performance-average">
<center>
<img src="img/avg.svg" alt="[performance-average]" class="plot">
<br />
<img src="img/avg_c23.svg" alt="[performance-average]" class="plot">
</center>
<figcaption markdown="block">
Average query evaluation times for the TPF client and Comunica for all queries.
C2 and C3 are shown separately because of their longer evaluation times.
</figcaption>
</figure>

TODO: discuss results
