# Force-Directed-Graph

I simply implement the force-directed graph layout algorithm using Javascript and SVG objects.Then I made some improvements on it.

## Algorithm 

The force-directed layout algorithm is easy-understanding.We treat each node as point interconnected with spring and repulsive force.So according to Coulomb's law and Huke's law,these points will move and finally stop when their force banlanced.In this state,these points would not be too far or too near from each other,thus making a beautiful layout.The pseudocode of this algorithm presented below:

     L = ... // spring rest length
	 K_r = ... // repulsive force constant
	 K_s = ... // spring constant
	 delta_t= ... // time step
	 N = nodes.length
 	// initialize net forces
 	for i = 0 to N-1
 	nodes[i].force_x= 0
 	nodes[i].force_y= 0

 	// repulsion between all pairs
 	for i1 = 0 to N-2
 	node1 = nodes[i1]
  	for i2 = i1+1 to N-1
  	node2 = nodes[i2]
 	dx = node2.x - node1.x
 	dy = node2.y - node1.y
 	if dx != 0 or dy != 0
  	distanceSquared= dx *dx + dy*dy
  	distance = sqrt( distanceSquared)
  	force = K_r / distanceSquared
 	fx = force * dx / distance
  	fy = force * dy / distance
  	node1.force_x = node1.force_x - fx
  	node1.force_y = node1.force_y - fy
  	node2.force_x = node2.force_x + fx
  	node2.force_y = node2.force_y + fy
 	// spring force between adjacent pairs
 	for i1 = 0 to N-1
  	node1 = nodes[i1]
  	for j = 0 to node1.neighbors.length-1
 	i2 = node1.neighbors[j]
 	node2 = nodes[i2]
  	if i1 < i2
  	dx = node2.x - node1.x
  	dy = node2.y - node1.y
  	if dx != 0 or dy != 0
  	distance = sqrt( dx*dx + dy*dy )
 	force = K_s*( distance - L )
  	fx = force*dx / distance
 	fy = force*dy / distance
  	node1.force_x = node1.force_x + fx
  	node1.force_y = node1.force_y + fy
  	node2.force_x = node2.force_x - fx
  	node2.force_y = node2.force_y - fy

 	// update positions
 	for i = 0 to N-1
  	node = nodes[i]
 	dx = delta_t*node.force_x
  	dy = delta_t*node.force_y
  	displacementSquared= dx*dx + dy*dy
  	if ( displacementSquared> MAX_DISPLACEMENT_SQUARED )
  	s = sqrt( MAX_DISPLACEMENT_SQUARED / displacementSquared)
  	dx = dx *s
  	dy = dy*s
  	node.x= node.x + dx
  	node.y = node.y + dy

## Improvements
We all know that the bottleleck of this algorithm lies in the calculation of repulsion between all vertices pairs and the general algorithm complexity is O(n^2) for every iteration.There are ways to improve its performance to O(n*logn) using quard-tree or K-d tree data structure etc.There are also some simple ways to optimize it.Here are my improvements:

- Using the simulated annealing algorithm.

In order to make the nodes converge quickly,we can define a "global temperature"
that controls the step width of node movements and the algorithm's termination. The step width is proportional to the temperature, so if the temperature is hot, the nodes move faster. This temperature is the same for all nodes, and cools down at each iteration. Once the nodes stop moving, the system terminates.

- Ignoring the repulsion from the remote vertices

In order to decrease the calculation of the repulsion,we can ignore the repulsion force from remote vertices.As long as the nodes would not converge too close,we can try this.We need to determine whether the distance is larger than the threshold or not before calculating the repulsion.And we can adjust the other parameters to make the final graph more nicely.

- Some minor improvements such as presenting the constant calculation code out of the loop to avoid unnecessary cost on the performance.


## Display

Run the demo.xhtml in your browser.

