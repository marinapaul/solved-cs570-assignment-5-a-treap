Download Link: https://assignmentchef.com/product/solved-cs570-assignment-5-a-treap
<br>
Assignment

A <em>treap </em>is a binary search tree (BST) which additionally maintains heap priorities. An example is given in Figure 1. A node consists of

<ul>

 <li>A key <em>k </em>(given by the letter in the example),</li>

 <li>A random heap priority <em>p </em>(given by the number in the example). The heap priority <em>p </em>is assigned at random upon insertion of a node. It should be unique in the treap.</li>

 <li>A pointer to the left child and to the right child node,</li>

</ul>

Note that if you insert just the keys into an empty BST following the priority (from max to min) you get back a BST of the exact same form as the treap. For example, if you insert the keys h, j, c, a, e into an empty BST, then you get back a tree just like that in Figure 1. Since the priorities are chosen at random, the resulting tree is one particular permutation (in this case [h,j,c,a,e]) chosen at random. Thus treaps may be understood as <em>random BSTs</em>.

Regarding the performance of its operations, on average (the run time depends on the randomly chosen heap priorities), add, delete, and find operations take <em>O</em>(log(<em>n</em>)) time where <em>n </em>is the number of nodes in the treap.

Figure 1: Example of a Treap

In this homework, you will implement a treap. In the following, we discuss the components of the data structure and its operations in detail.

<h2>2.1           The Node Class</h2>

Create a private static inner class Node&lt;E&gt; of the Treap class (described in the next subsection) with the following attributes and constructors:

<ul>

 <li>Data fields:</li>

</ul>

<table width="0">

 <tbody>

  <tr>

   <td width="496"><strong>public </strong>E data // key for the search <strong>public int </strong>priority // random heap priority <strong>public </strong>Node&lt;E&gt; left <strong>public </strong>Node&lt;E&gt; right</td>

  </tr>

 </tbody>

</table>

2

4

<ul>

 <li>Constructors:</li>

</ul>

<strong>public </strong>Node(E data, <strong>int </strong>priority)

Creates a new node with the given data and priority. The pointers to child nodes are null. Throw exceptions if data is null.

<ul>

 <li>Methods:</li>

</ul>

<table width="0">

 <tbody>

  <tr>

   <td width="496">Node&lt;E&gt; rotateRight()Node&lt;E&gt; rotateLeft()</td>

  </tr>

 </tbody>

</table>

1

rotateRight() performs a right rotation according to Figure 2, returning a reference to the root of the result. The root node in the figure corresponds to this node. Update the data and priority attributes as well as the left and right pointers of the involved nodes accordingly.

rotateLeft() performs a left rotation according to Figure 2 returning a reference to the root of the result. The root node in the figure corresponds to this node. Update the attributes of the nodes accordingly.

Why rotation? Rotations preserve the ordering of the BST, but allows one to restore the heap invariant. Indeed, after adding a node to a treap or removing a node from a treap, the node may violate the heap property considering the priorities. In this case it is necessary to perform one or more of these rotations to restore the heap property. Further details shall be supplied below.

(a)                                                                             (b)

Figure 2: Right rotation (a<em>→</em>b) and left rotation (b<em>→</em>a)

<h2>2.2           The Treap Class</h2>

<ul>

 <li>Data fields:</li>

</ul>

<table width="0">

 <tbody>

  <tr>

   <td width="496"><strong>private </strong>Random priorityGenerator; <strong>private </strong>Node&lt;E&gt; root;</td>

  </tr>

 </tbody>

</table>

2

E must be Comparable.

<ul>

 <li>Constructors:</li>

</ul>

<table width="0">

 <tbody>

  <tr>

   <td width="496"><strong>public </strong>Treap() <strong>public </strong>Treap(<strong>long </strong>seed)</td>

  </tr>

 </tbody>

</table>

2

Treap() creates an empty treap. Initialize priorityGenerator using <strong>new </strong>Random(). See <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Random.html">http://docs.oracle.com/javase/8/docs/api/java/util/Random.html</a> for more information regarding Java’s pseudo-random number generator.

Treap(<strong>long </strong>seed) creates an empty treap and initializes priorityGenerator using <strong>new </strong>Random(seed).

<ul>

 <li>Methods:</li>

</ul>

<table width="0">

 <tbody>

  <tr>

   <td width="496"><strong>boolean </strong>add(E key) <strong>boolean </strong>add(E key, <strong>int </strong>priority) <strong>boolean </strong>delete(E key)<strong>private boolean </strong>find(Node&lt;E&gt; root, E key) <strong>public boolean </strong>find(E key) <strong>public </strong>String toString()</td>

  </tr>

 </tbody>

</table>

2

4

6

We next describe each of these methods.

<strong>2.2.1        Add operation</strong>

To insert the given element into the tree, create a new node containing key as its data and a random priority generated by priorityGenerator. The method returns <strong>true</strong>, if a node with the key was successfully added to the treap. If there is already a node containing the given key, the method returns <strong>false </strong>and does not modify the treap.

<ul>

 <li>Insert the new node as a leaf of the tree at the appropriate position according to the ordering on E, just like in any BST.</li>

 <li>If the priority of the parent node is less than the priority of the new node, bubble up the new node in the tree towards the root such that the treap is a heap according to the priorities of each node (the heap is a max-heap, i.e., the root contains the highest priority). To bubble up the node, you must implement the rotation operations mentioned above.</li>

</ul>

Here is an example in which the node (i,93) is added to the treap.

Hint: For the add method you should proceed as in the addition for BSTs. Except that

I would suggest

<ul>

 <li>adapting it to an iterative version (rather than using recursion).</li>

 <li>storing each node in the path from the root until the spot where the new node will be inserted, in a stack</li>

 <li>after performing the insertion, use a helper function reheap (with appropriate parameters that should include the stack) to restore the heap invariant. Note that if our nodes had pointer to their parents, then we would not need a stack.</li>

 <li>have add(E key) call the add(E key, <strong>int </strong>priority) method once it has generated the random priority. Thus all the “work” is performed by the latter method.</li>

</ul>

<strong>2.2.2 Delete operation </strong><strong>boolean </strong>delete(E key) deletes the node with the given key from the treap and returns <strong>true</strong>. If the key was not found, the method does not modify the treap and returns <strong>false</strong>. In order to remove a node trickle it down using rotation until it becomes a leaf and then remove it. When trickling down sometimes you will have to rotate left and sometimes right. That will depend on whether there is no left subtree of the node to delete, or there is no right subtree of the node to erase; if the node to erase has both then you have to look at the priorities of the children and consider the highest one to determine whether you have to rotate to the left or the right.

Here is an example of deletion of the node (z,47):

<strong>2.2.3 Find operation </strong><strong>private boolean </strong>find(Node&lt;E&gt; root, E key): Finds a node with the given key in the treap rooted at root and returns true if it finds it and false otherwise.

<strong>boolean </strong>find(E key): Finds a node with the given key in the treap and returns true if it finds it and false otherwise.

<strong>2.2.4 </strong><strong>toString operation</strong>

<strong>public </strong>String toString(): Carries out a preorder traversal of the tree and returns a representation of the nodes as a string. Each node with key <em>k </em>and priority <em>p</em>, left child <em>l</em>, and right child <em>r </em>is represented as the string [<em>k,p</em>] (<em>l</em>) (<em>r</em>). If the left child does not exist, the string representation is [<em>k,p</em>] <em>null </em>(<em>r</em>). Analogously, if there is no right child, the string representation of the tree is [<em>k,p</em>] (<em>l</em>) <em>null</em>. Variables <em>l</em>, <em>k</em>, and <em>p </em>must be replaced by its corresponding string representation, as defined by the toString() method of the corresponding object.

Hint: You can reuse the exact same method in the binary tree class we saw in class. You will have to add a toString method to the Node class so that it prints a pair consisting of the key and its priority.

<h1>3           An Example Test</h1>

For testing purposes you might consider creating a Treap by inserting this list of pairs (key,priority) using the method <strong>boolean </strong>add(E key, <strong>int </strong>priority):

(4,19);(2,31);(6,70);(1,84);(3,12);(5,83);(7,26)

The code for building this Treap is:

<table width="0">

 <tbody>

  <tr>

   <td width="529">testTree = <strong>new </strong>Treap&lt;Integer&gt;(); testTree.add(4,19); testTree.add(2,31); testTree.add(6,70); testTree.add(1,84); testTree.add(3,12); testTree.add(5,83); testTree.add(7,26);</td>

  </tr>

 </tbody>

</table>

2

4

6

8

The resulting Treap should look like this:

The output using toString() of the above would be

<table width="0">

 <tbody>

  <tr>

   <td width="529">(key=1, priority=84) <strong>null</strong>(key=5, priority=83)(key=2, priority=31)</td>

  </tr>

 </tbody>

</table>

2

4

<table width="0">

 <tbody>

  <tr>

   <td width="529"><strong>null</strong>(key=4, priority=19)(key=3, priority=12) <strong>null null</strong><strong>null</strong>(key=6, priority=70) <strong>null</strong>(key=7, priority=26) <strong>null null</strong></td>

  </tr>

 </tbody>

</table>


