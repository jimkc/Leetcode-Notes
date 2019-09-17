## Question
Given a binary tree<br>

<pre>
struct TreeLinkNode {
  TreeLinkNode *left;
  TreeLinkNode *right;
  TreeLinkNode *next;
}
</pre>

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.<br>

Initially, all next pointers are set to NULL.<br>

Note:<br>

You may only use constant extra space.<br>
Recursive approach is fine, implicit stack space does not count as extra space for this problem.

**Example :**   
<pre>
Given the following binary tree,

    1
   /  \
  2    3
 / \    \
4   5    7
After calling your function, the tree should look like:

     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \    \
4-> 5 -> 7 -> NULL
</pre>

## Thinking
Use queue and level to do level order traverss(Bfs)

## Coding
Time: O(n);<br>
Space: O(n)
```python3
class Solution:
# @param root, a tree link node
# @return nothing
    def connect(self, root):
        if not root:
            return
        queue, level = collections.deque([root]), collections.deque()
        while queue:
            node = queue.popleft()
            if node.left:
                level.append(node.left)
            if node.right:
                level.append(node.right)
            node.next = queue[0] if queue else None
            if not queue and level:
                queue, level = level, queue
```

O(1) Space:
```python3
class Solution:
# @param root, a tree link node
# @return nothing
def connect(self, root):
    queue, level, curr = root, None, None
    while queue:
        if queue.left:
            if not level:
                level = curr = queue.left
            else:
                curr.next = queue.left
                curr = curr.next
        if queue.right:
            if not level:
                level = curr = queue.right
            else:
                curr.next = queue.right
                curr = curr.next
        queue = queue.next
        if not queue and level:
            queue, level, curr = level, None, None
```
