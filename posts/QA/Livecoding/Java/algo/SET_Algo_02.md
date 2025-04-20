#### Задача 1 Инвертировать бинарное дерево

```java
public class TreeNode {

    int val;
    TreeNode left;
    TreeNode right;

    TreeNode (int x) {
        val = x;
    }
}

/*
Нужно переставить узлы бинарного дерева, как показано в примере. То есть необходимо
инвертировать бинарное дерево.
        4
      7   2
    9  6  3  1

        4
      2   7
    1  3  6  9
*/
public TreeNode invertTree(TeeNode root) {

}
```

###### Решение

```java
// вглубь, рекурсия
public TreeNode invertTree(TeeNode root) {
    if (root = null) {
        return null;
    }
    TreeNode temp = root.left;
    root.left = invertTree(root.right);
    root.right = invertTree(temp);

    return root;
}

// в ширину, используя очередь

    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            TreeNode current = queue.poll();

            // Меняем местами левое и правое поддеревья
            TreeNode temp = current.left;
            current.left = current.right;
            current.right = temp;

            // Добавляем детей в очередь, если они не null
            if (current.left != null) {
                queue.add(current.left);
            }
            if (current.right != null) {
                queue.add(current.right);
            }
        }

        return root;
    }

```

