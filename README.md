Download Link: https://assignmentchef.com/product/solved-cs210-project-5-kd-trees
<br>
<table width="719">

 <tbody>

  <tr>

   <td width="719">This document only contains the description of the project and the project problems. For the programming exercises on concepts needed for the project, please refer to the <a href="https://www.swamiiyer.net/cs210/project5_checklist.pdf">project checklist</a>  <a href="https://www.swamiiyer.net/cs210/project5_checklist.pdf">.</a></td>

  </tr>

 </tbody>

</table>

The purpose of this assignment is to create a symbol table data type whose keys are two-dimensional points. We’ll use a 2d-tree to support efficient range search (find all the points contained in a query rectangle) and <em>k</em>-nearest neighbor search (find <em>k </em>points that are closest to a query point). 2d-trees have numerous applications, ranging from classifying astronomical objects to computer animation to speeding up neural networks to mining data to image retrieval.

<strong>Geometric Primitives </strong>To get started, use the following geometric primitives for points and axis-aligned rectangles in the plane.

Use the immutable data type Point2D for points in the plane. Here is the subset of its API that you may use:

<h1>method                                                                 description</h1>

<table width="720">

 <tbody>

  <tr>

   <td width="238">Point2D(double x, double y)</td>

   <td width="482">construct the point (<em>x,y</em>)</td>

  </tr>

  <tr>

   <td width="238">double x()</td>

   <td width="482"><em>x</em>-coordinate</td>

  </tr>

  <tr>

   <td width="238">double y()</td>

   <td width="482"><em>y</em>-coordinate</td>

  </tr>

  <tr>

   <td width="238">double distanceSquaredTo(Point2D that)</td>

   <td width="482">square of Euclidean distance between this point and <em>that</em></td>

  </tr>

  <tr>

   <td width="238">Comparator&lt;Point2D&gt; distanceToOrder()</td>

   <td width="482">a comparator that compares two points by their distance to this point</td>

  </tr>

  <tr>

   <td width="238">boolean equals(Point2D that)</td>

   <td width="482">does this point equal <em>that</em>?</td>

  </tr>

  <tr>

   <td width="238">String toString()</td>

   <td width="482">a string representation of this point</td>

  </tr>

 </tbody>

</table>

Use the immutable data type RectHV for axis-aligned rectangles. Here is the subset of its API that you may use:

<h1>method                                                                                                      description</h1>

<table width="720">

 <tbody>

  <tr>

   <td width="351">RectHV(double xmin, double ymin, double xmax, double ymax)</td>

   <td width="369">construct the rectangle [<em>x<sub>min</sub>,x<sub>max</sub></em>] × [<em>y<sub>min</sub>,y<sub>max</sub></em>]</td>

  </tr>

  <tr>

   <td width="351">double xmin()</td>

   <td width="369">minimum <em>x</em>-coordinate of rectangle</td>

  </tr>

  <tr>

   <td width="351">double xmax()</td>

   <td width="369">maximum <em>x</em>-coordinate of rectangle</td>

  </tr>

  <tr>

   <td width="351">double ymin()</td>

   <td width="369">minimum <em>y</em>-coordinate of rectangle</td>

  </tr>

  <tr>

   <td width="351">double ymax()</td>

   <td width="369">maximum <em>y</em>-coordinate of rectangle</td>

  </tr>

  <tr>

   <td width="351">boolean contains(Point2D p)</td>

   <td width="369">does this rectangle contain the point <em>p </em>(either inside or on boundary)?</td>

  </tr>

  <tr>

   <td width="351">boolean intersects(RectHV that)</td>

   <td width="369">does this rectangle intersect <em>that </em>rectangle (at one or more points)?</td>

  </tr>

  <tr>

   <td width="351">double distanceSquaredTo(Point2D p)</td>

   <td width="369">square of Euclidean distance from point <em>p </em>to closest point in rectangle</td>

  </tr>

  <tr>

   <td width="351">boolean equals(RectHV that)</td>

   <td width="369">does this rectangle equal <em>that</em>?</td>

  </tr>

  <tr>

   <td width="351">String toString()</td>

   <td width="369">a string representation of this rectangle</td>

  </tr>

 </tbody>

</table>

<strong>Symbol Table API </strong>Here is a Java interface PointST&lt;Value&gt; specifying the API for a symbol table data type whose keys are two-dimensional points represented as Point2D objects:

<h1>method                                                                          description</h1>

boolean isEmpty()                                                                is the symbol table empty?

int size()                                                                              number of points in the symbol table

void put(Point2D p, Value val) associate the value <em>val </em>with point <em>p </em>Value get(Point2D p) value associated with point <em>p </em>boolean contains(Point2D p) does the symbol table contain the point <em>p</em>?

Iterable&lt;Point2D&gt; points()                                                    all points in the symbol table

Iterable&lt;Point2D&gt; range(RectHV rect)                                     all points in the symbol table that are inside the rectangle <em>rect</em>

Point2D nearest(Point2D p) a nearest neighbor to point <em>p</em>; null if the symbol table is empty Iterable&lt;Point2D&gt; nearest(Point2D p, int k) <em>k </em>points that are closest to point <em>p</em>

<strong>Problem 1. </strong>(<em>Brute-force Implementation</em>) Write a mutable data type BrutePointST that implements the above API using a red-black BST (RedBlackBST).

<em>Corner cases. </em>Throw a java.lang.NullPointerException if any argument is null.

<em>Performance requirements. </em>Your implementation should support put(), get() and contains() in time proportional to the logarithm of the number of points in the set in the worst case; it should support points(), range(), and nearest() in time proportional to the number of points in the symbol table.

<table width="720">

 <tbody>

  <tr>

   <td width="720">$ java BrutePointST 0.661633 0.287141 0.65 0.68 0.28 0.29 5 &lt; data/input10K.txt st.empty()? false st.size() = 10000 First 5 values:338015858903416859717265 st.contains((0.661633, 0.287141))? true st.range([0.65, 0.68] x [0.28, 0.29]):(0.663908, 0.285337)(0.661633, 0.287141)(0.671793, 0.288608) st.nearest((0.661633, 0.287141)) = (0.663908, 0.285337) st.nearest((0.661633, 0.287141), 5):(0.663908, 0.285337)(0.658329, 0.290039)(0.671793, 0.288608) (0.65471, 0.276885)(0.668229, 0.276482)</td>

  </tr>

 </tbody>

</table>

<strong>Problem 2. </strong>(<em>2d-tree Implementation</em>) Write a mutable data type KdTreePointST that uses a 2d-tree to implement the above symbol table API. A 2d-tree is a generalization of a BST to two-dimensional keys. The idea is to build a BST with points in the nodes, using the <em>x</em>– and <em>y</em>-coordinates of the points as keys in strictly alternating sequence, starting with the <em>x</em>-coordinates.

<ul>

 <li><em>Search and insert. </em>The algorithms for search and insert are similar to those for BSTs, but at the root we use the <em>x</em>-coordinate (if the point to be inserted has a smaller <em>x</em>-coordinate than the point at the root, go left; otherwise go right); then at the next level, we use the <em>y</em>-coordinate (if the point to be inserted has a smaller <em>y</em>-coordinate than the point in the node, go left; otherwise go right); then at the next level the <em>x</em>-coordinate, and so forth.</li>

 <li><em>Level-order traversal. </em>The points() method should return the points in level-order: first the root, then all children of the root (from left/bottom to right/top), then all grandchildren of the root (from left to right), and so forth. The level-order traversal of the 2d-tree above is (0.7, 0.2), (0.5, 0.4), (0.9, 0.6), (0.2, 0.3), (0.4, 0.7).</li>

</ul>

The prime advantage of a 2d-tree over a BST is that it supports efficient implementation of range search, nearest neighbor, and <em>k</em>-nearest neighbor search. Each node corresponds to an axis-aligned rectangle, which encloses all of the points in its subtree. The root corresponds to the infinitely large square from [(−∞<em>,</em>−∞)<em>,</em>(+∞<em>,</em>+∞)]; the left and right children of the root correspond to the two rectangles split by the <em>x</em>-coordinate of the point at the root; and so forth.

<ul>

 <li><em>Range search. </em>To find all points contained in a given query rectangle, start at the root and recursively search for points in both subtrees using the following pruning rule: if the query rectangle does not intersect the rectangle corresponding to a node, there is no need to explore that node (or its subtrees). That is, you should search a subtree only if it might contain a point contained in the query rectangle.</li>

 <li><em>Nearest neighbor search. </em>To find a closest point to a given query point, start at the root and recursively search in both subtrees using the following pruning rule: if the closest point discovered so far is closer than the distance between the query point and the rectangle corresponding to a node, there is no need to explore that node (or its subtrees). That is, you should search a node only if it might contain a point that is closer than the best one found so far. The effectiveness of the pruning rule depends on quickly finding a nearby point. To do this, organize your recursive method so that when there are two possible subtrees to go down, you choose first the subtree that is on the same side of the splitting line as the query point; the closest point found while exploring the first subtree may enable pruning of the second subtree.</li>

 <li><em>k-nearest neighbor search. </em>Use the technique from kd-tree nearest neighbor search described above.</li>

</ul>

<em>Corner cases. </em>Throw a java.lang.NullPointerException if any argument is null.

<table width="720">

 <tbody>

  <tr>

   <td width="720">java KdTreePointST 0.661633 0.287141 0.65 0.68 0.28 0.29 5 &lt; data/input10K.txt st.empty()? false st.size() = 10000 First 5 values:02143</td>

  </tr>

  <tr>

   <td width="720">62 st.contains((0.661633, 0.287141))? true st.range([0.65, 0.68] x [0.28, 0.29]):(0.671793, 0.288608)(0.663908, 0.285337)(0.661633, 0.287141) st.nearest((0.661633, 0.287141)) = (0.663908, 0.285337) st.nearest((0.661633, 0.287141), 5):(0.668229, 0.276482) (0.65471, 0.276885)(0.671793, 0.288608)(0.658329, 0.290039)(0.663908, 0.285337)</td>

  </tr>

 </tbody>

</table>

<strong>Acknowledgements </strong>This project is an adaptation of the Kd-Trees assignment developed at Princeton University by Kevin Wayne, with boid simulation by Josh Hug.