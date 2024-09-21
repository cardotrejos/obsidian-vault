```javascript
function ValueInTree(tree, val) {
  // base case: empty tree
  if (!tree) {
    return false;
  }

  // check the current node's value
  if (tree[0] === val) {
    return true;
  }

  // recursively search the left subtree
  if (ValueInTree(tree[1], val)) {
    return true;
  }

  // recursively search the right subtree
  if (ValueInTree(tree[2], val)) {
    return true;
  }

  // value not found
  return false;
}

// Example usage:
const arr1 = [3, [8, [5, null, null], null], [7, null, [1, null, null]]];
console.log(ValueInTree(arr1, 5)); // true

const arr2 = [3, [8, [5, null, null], null], [7, null, [1, null, null]]]; 
console.log(ValueInTree(arr2, 9)); // false

const arr3 = [3, [8, [5, null, null], null], [7, null, null]];
console.log(ValueInTree(arr3, 7)); // true

const arr4 = [];
console.log(ValueInTree(arr4, 5)); // false
```
