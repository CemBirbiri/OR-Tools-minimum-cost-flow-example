# minimum-cost-flow-example-with-OR-Tools
The minimum-cost flow problem (MCFP) is an optimization and decision problem to find the cheapest possible way of sending a certain amount of flow through a flow network. A typical application of this problem involves finding the best delivery route from a factory to a warehouse where the road network has some capacity and cost associated.

The min cost flow problem also has special nodes, called `supply nodes` or `demand nodes`, which are similar to the `source` and `sink` in the max flow problem. Material is transported from supply nodes to demand nodes.

- At a `supply node`, a positive amount — the supply — is added to the flow. A supply could represent production at that node, for example.
- At a `demand node`, a negative amount — the demand — is taken away from the flow. A demand could represent consumption at that node, for example.

Here, I will show an example of a **Minimising the total weekly costs** problem and solutions implemented with [OR-Tools](https://developers.google.com/optimization/flow/mincostflow) and [Networkx library](https://networkx.org/)


<img width="784" alt="Screenshot 2023-06-28 at 20 10 52" src="https://github.com/CemBirbiri/minimum-cost-flow-problem-with-OR-Tools/assets/46814542/b9758ab8-871e-4e00-8b91-bba249ef1556">

1- Using OR-Tools

We need to install to OR-Tool library first. To use it in Google Colab, we need to install an old version(version = 7.5.7466) of OR-Tools.

```
#Install the OR-Tools
!pip install ortools==7.5.7466
```

Then we define the sources, distributers and unit costs
```
#Sources
start_nodes = [ 1, 1, 2, 2, 3, 3, 3, 4, 4, 4]
#Distributors centers:
end_nodes   = [ 3, 4, 3, 4, 5, 6, 7, 5, 6, 7]
#Costs
unit_costs  = [ 44, 50, 54, 52, 40, 59, 76, 73, 47, 79]
```

Our problem does not have a given capacity limit for the links, we will therefore use a very large number instead. Unfortunately OR-Tools does not give us the option to define an arc without providing a capacity.

```
capacity = 9999999
```

Let's define the supply values for each node.

```
supplies = [600, 500, 0, 0, -300, -300, -500]
```

In OR-Tools, the SimpleMinCostFlow() functions solves the problem directly.

```
import ortools
import ortools.graph.pywrapgraph

#Define the model
model = ortools.graph.pywrapgraph.SimpleMinCostFlow()

# Add each arc.
for i in range(len(start_nodes)):
    model.AddArcWithCapacityAndUnitCost(start_nodes[i], end_nodes[i],capacity, unit_costs[i])

for i in range(len(supplies)):
    model.SetNodeSupply(i+1, supplies[i])
```
Then solve the model and print the flow.

```
model.Solve()

print('Minimum cost:', model.OptimalCost())
print('')
for i in range(model.NumArcs()):
    print(f'{model.Tail(i)}->{model.Head(i)} \t Flow: {model.Flow(i)}')
```

The output of the above code is:

```
Minimum cost: 116900

1->3 	 Flow: 600
1->4 	 Flow: 0
2->3 	 Flow: 200
2->4 	 Flow: 300
3->5 	 Flow: 300
3->6 	 Flow: 0
3->7 	 Flow: 500
4->5 	 Flow: 0
4->6 	 Flow: 300
4->7 	 Flow: 0
```
We can see the sources and distributer centers as well as minimum cost flow between each of them. The minimum cost of the whole flow is 116900.
