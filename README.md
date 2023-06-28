# minimum-cost-flow-example-with-OR-Tools
The minimum-cost flow problem (MCFP) is an optimization and decision problem to find the cheapest possible way of sending a certain amount of flow through a flow network. A typical application of this problem involves finding the best delivery route from a factory to a warehouse where the road network has some capacity and cost associated.

The min cost flow problem also has special nodes, called `supply nodes` or `demand nodes`, which are similar to the `source` and `sink` in the max flow problem. Material is transported from supply nodes to demand nodes.

- At a supply node, a positive amount — the supply — is added to the flow. A supply could represent production at that node, for example.
- At a demand node, a negative amount — the demand — is taken away from the flow. A demand could represent consumption at that node, for example.
