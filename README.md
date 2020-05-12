# Binary Search Tree
_Implementation of Binary Search Tree in Javascript_

A data structure that consists of nodes in a parent/child relationship.

* Every parent node has at most two children.
* Every node to the left of a parent node is always less than the parent.
* Every node to the right of a parent node is always greater than the parent.

## Operations

### Insertion
1. Create a new node.
2. Check if there is a root, if not assign new node to be the root.
3. If there is a root check if the value of the new node is greater than or less than the value of the root.
4. Recursively do the following steps:
    * If value is greater check if there is a node to the right. If there is move to that node compare again. If there is not add the new node as the right property
    * If it is less check if there is node to the left. If there is move to that node compare again. If there is not add the new node as the left property.

### Search
1. If there is no root node stop searching.
2. If there is a root node, compare value we are searching for to the value of the root node. If it is the value we are searching for we are done.
3. If not check to see if the value is greater than or less than the value of the root.
4. Recursively do the following steps:
    * If value is greater than check if there is a node to the right and compare. If there is no node to the right we are done.
    * If value is less than check if there is a node to the left and compare. If there is no node to the left we are done.

### Deletion
1. If node being deleted has no children simply detach it from it's parent node.
2. If node being deleted has a single child, connect the node's parent to its only child.
3. If node has 2 children
    * Find minimum in right subtree
    * Copy the value in the targetted value
    * Delete duplicate from the right sub-tree


# Code:
```javascript class:"lineNo"
// CLASS NODE
class Node {
    constructor (value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

// BINARY SEARCH TREE CLASS
class BinarySearchTree {
    constructor () {
        this.root = null;
    }

    // INSERT A NODE
    insert (value) {
        let newNode = new Node(value);

        function insertNode (current, node) {
            if (current.value === node.value) return null;

            if (node.value < current.value) {
                (current.left === null) ? current.left = node : insertNode(current.left, node);
            } else {
                (current.right === null) ? current.right = node : insertNode(current.right, node);
            }
        }

        if (this.root === null) {
            this.root = newNode;
        } else {
            insertNode(this.root, newNode);
        }
    }

    // SEARCH FOR A NODE 
    search (value) {
        
        function find (current, value) {
            if (current === null) return null;

            if (current.value === value) {
                return current;
            } else if (value < current.value && current.left !== null) {
                return find(current.left, value);
            } else if (value > current.value && current.right !== null) {
                return find(current.right, value);
            } else {
                return false; //=> value not found
            }
            
        }

        return find(this.root, value);

    }

    // DELETE A NODE
    delete (value) {
        function findMin(node) {
            while (node.left !== null) {
                node = node.left;
            }

            return node;
        }

        function removeNode (node, value) {
            if (node === null) {
                return null;
            } else if (value < node.value) {
                node.left = removeNode(node.left, value);
            } else if (value > node.value) {
                node.right = removeNode(node.right, value);
            } else {
                if (node.left === null && node.right === null) {
                    return null;
                } else if (node.left === null) {
                    return node.right;
                } else if (node.right === null) {
                    return node.left;
                } else {
                    let temp = findMin(node.right);
                    node.value = temp.value;
                    node.right = removeNode(node.right, temp.value);
                }
            }

            return node;
        }
        
        return removeNode(this.root, value);
    }

    // TRAVERSE - BREADTH FIRST SEARCH
    traverseBFS () {
        let queue = [];
        let data = [];
        let node;

        queue.push(this.root);

        while (queue.length !== 0) {
            node = queue.shift();
            data.push(node.value);

            if (node.left !== null) queue.push(node.left);
            if (node.right !== null) queue.push(node.right);
        }

        return data;
    }

    // TRAVERSE - DEPTH FIRST SEARCH
    traverseDFS () {
        let data = [];

        function traverse (node) {
            data.push(node.value);
            if (node.left !== null) traverse(node.left);
            if (node.right !== null) traverse(node.right);
        }

        traverse(this.root);
        return data;
    }
}


// CREATE A BINARYSEARCHTREE
let tree = new BinarySearchTree();

tree.insert(12);
tree.insert(5);
tree.insert(15);
tree.insert(3);
tree.insert(7);
tree.insert(13);
tree.insert(17);
tree.insert(1);
tree.insert(9);
tree.insert(14);
tree.insert(20);
tree.insert(8);
tree.insert(11);
tree.insert(18);

tree.search(8);

tree.delete(15);

tree.traverseBFS();

tree.traverseDFS();
```

