






---
# **More about the website**
---

> In this website, I will share my notes of leetcode questions also some basic knowledge of data structure and algorithms. Besides, I will refresh my machine learning and computer vision. I will share my notes of machine learning course on Coursera. Also, some intersting projects using openCV.  I have two interesting projects in progress during summer and I will share it among this website. I love reading and I always try to refresh my knowledge by reading. I will also share my reading notes in this website. Apart from professional study, I also have a lovely family. My lovely girlfriend enjoys cooking a lot. I will share the photos and methods of these delicacy. I love them deeply. Hope you not only can get something useful on this website, but also feel pleasant and relax once you open this website. Enjoy it!

---
# **Contents**

---
> [!NOTE]
> There are about seven parts of the content. More contents will be updated.
>
>  - [**Leetcode**]()
>  - [**Projects**]()
>  - [**Machine Learning**]()
>  - [**Computer Vision**]()
>  - [**Reading Notes**]()
>  - [**My family**]()
>  - [**Contact Me**]()
>  - [**Continuing**...]()

---

---
# Leetcode

#### Amazon

##### [Leetcode 1 Two Sum easy](https://leetcode.com/problems/two-sum/)

##### [Leetcode 102 Binary Tree Level order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)


#### According to tags
##### BFS
#####  DFS
##### Array
##### Linked List
##### Binary Search Tree

(1) Definition

- If the left subtree of any node is not empty, the value of all nodes on the left subtree is less than the value of its root node;

- If the right subtree of any node is not empty, the value of all nodes on the right subtree is greater than the value of its root node;

- The left subtree and the right subtree of any node are also binary search tree.

  

![image-20220610140045347](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610140045347.png)

(2) Insertion

![Insertion in a BST – Iterative and Recursive Solution | Techie Delight](https://www.techiedelight.com/wp-content/uploads/Insert-into-BST.png)

(3) Delete

Binary search tree delete node 3 cases:

- If there is no child node, delete it directly.

![image-20220610114004311](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610114004311.png)

- If there is only one child node, the child node replaces the current node, and then deletes the current node.

  ![image-20220610114030105](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610114030105.png)

- If there are two child nodes, replace the current node with the smallest node from the right subtree, because the smallest node on the right is also larger than the value on the left.

  ![÷](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610114047006.png)

(4) Max-value & Min-value

The largest value is on the right child node, as long as the recursive traversal is the right child until be empty, the current node is the largest node.

- C++ code for finding Max-value

```c++
    Node *search_max_value(Node *node) {
        if (node == nullptr || node -> data == 0) {
            return nullptr;
        }
        
        // If the right child node is empty, find the largest node.
        if (node -> right == nullptr) {
            return node;
        }
        
        // Recursively find the maximum in the right subtree.
        return search_max_value(node -> right);
    }
```

- The smallest value is on the left subtree, as long as the recursion traverses the left child until be empty, the current node is the minimum node.

C++ code for finding Min-value

```c++
    Node *search_min_value(Node *node) {
        if (node == nullptr || node -> data == 0) {
            return nullptr;
        }

        if (node -> left == nullptr) {
            return node;
        }

        return search_min_value(node -> left);
    }
```

C++ code for creating, inserting and deleting BST.

[!NOTE] Notes for deletion.

![image-20220610161242627](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610161242627.png)

The current node 69 does not have a left child or right child.  So it will execute this line of code. And the result is that this current node will be deleted.

```c++
node = (node -> left != nullptr) ? node -> left : node -> right;
```

![image-20220610163428414](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610163428414.png)

The current node only has a left child, so it will also executes this line of code. And the result will be that the parent node's left pointer will point to the current node's left child node. And the memory of the current node will be free.  I noticed that the code in the original book only change the pointer but not free the memory. In my point of view, this may cause a memory leak. So I change the code to release the memory.

![image-20220610165830877](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610165830877.png)

The current node has both a left child and a right child. Firstly, find the smallest data in the right subtree and subsititudes the cureent node's data with this smallest data. After that, recursively adjusting the node in the right subtree.

```c++
#include <iostream>

using namespace std;

class Node {
public:
    int data;
    Node *left;
    Node * right;
};

class BinarySearchTree {
private:
    Node *root;

public:
    BinarySearchTree() {
        root = nullptr;
    }

    Node *create_new_node(int new_data) {
        Node *new_node = new Node();
        new_node -> data = new_data;
        new_node -> left = nullptr;
        new_node -> right = nullptr;
        return new_node;
    }

    void insert(Node *node, int new_data) {
        if (root == nullptr) {
            root = new Node();
            root -> data = new_data;
            root -> left = nullptr;
            root -> right = nullptr;
            return;
        }

        int compare_value = new_data - node -> data;

        // The new data is less than the root node.
        // Need to find a place in the left subtree to insert the new node.
        // Recursive left subtree to find the insertion position.
        if (compare_value < 0) {
            if (node -> left == nullptr) {
                node -> left = create_new_node(new_data);
            } else {
                insert(node -> left, new_data);
            }
        }
        // The new data is greater than the root node.
        // Need to find a place in the right subtree to insert the new data.
        // Recursive right subtree to find the insertion position.
        else if (compare_value > 0) {
            if (node -> right == nullptr) {
                node -> right = create_new_node(new_data);
            } else {
                insert(node -> right, new_data);
            }
        }
    }

    Node *search_min_value(Node *node) {
        if (node == nullptr || node -> data == 0) {
            return nullptr;
        }

        if (node -> left == nullptr) {
            return node;
        }

        return search_min_value(node -> left);
    }

    Node *remove_node(Node* node, int new_data) {
        if (node == nullptr) {
            return node;
        }

        // Recursively to find the position of the node needed to be deleted.
        int compare_value = new_data - node -> data;
        if (compare_value > 0) {
            node -> right = remove_node(node -> right, new_data);
        }
        else if (compare_value < 0) {
            node -> left = remove_node(node -> left, new_data);
        }

        // If the position is found and this node has both a left child and a right child.
        // Firstly, find the smallest node data in the right subtree of this node to replace the current node data.
        //
        else if (node -> left != nullptr && node -> right != nullptr) {
            node -> data = search_min_value(node -> right) -> data;
            node -> right = remove_node(node -> right, node -> data);
        }
        else {
            Node *tmp_node = node;
            node = (node -> left != nullptr) ? node -> left : node -> right;
            free(tmp_node);
        }

        return node;
    }

    Node *get_root() {
        return root;
    }

    // Recursively free the node of the whole BST using Past-order Traversal.
    void free_memory(Node *node) {
        if (node == nullptr) {
            return;
        }

        free_memory(node -> left);
        free_memory(node -> right);

        delete node;
    }

    ~BinarySearchTree() {
        free_memory(root);
    }
};
```

(5) Traversal

![image-20220610231235996](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610231235996.png)

- In-order Traversal

  Left subtree -> Root node -> Right subtree

  ![image-20220610231412916](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610231412916.png)

  C++ code for In-order Traversal (BFS)
```c++
void in_order(Node *node) {
  if (node == nullptr) {
    return;
  }
  in_order(node -> left);
  cout << node -> data << ",";
  in_order(node -> right);
}
```
- Pre-order  Traversal

  Root node -> Left subtree -> Right subtree

  ![image-20220610231816747](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610231816747.png)

  C++ code for Pre-order Traversal (BFS)
```c++
void pre_order(Node *node) {
  if (node == nullptr) {
    return;
  }
  cout << node -> data << ",";
  pre_order(node -> left);
  pre_order(node -> right);
}
```
- Post-order Tracersal

  Left subtree -> Right subtree -> Root node

  ![image-20220610232002986](/Users/weizhang/Library/Application Support/typora-user-images/image-20220610232002986.png)

  C++ code for Past-order Traversal (BFS)
```c++
void post_order(Node *node) {
  if (node == nullptr) {
    return;
  }
  post_order(node -> left);
  post_order(node -> right);
  cout << node -> data << ",";
}
```

(6)  Related Leetcode problem
- [Leetcode 102]()


(7) Reference

All images come from the internet.

<a id="1">[1]</a> Hu, Y. (2022). *Easy Learning Data Structures & Algorithms C++*. Independently published. 

---
# **Projects**
---
#### [Adventure Game]()
#### [Tuit]()
#### [Online Preview Editor]()
#### [Raspberry pi rat car]()
---
# **Machine Learning**
---

---
# **Computer Vision**
---

---
# **Reading notes**
---

---
# **My Family**
---

---
# **Contact Me**
---
<p align="center">
  <a href="https://github.com/rd2coding/Road2Coding" target="_blank"><img src="https://img.shields.io/badge/Github-r2coding-red.svg"></a>
  <a href="https://gitee.com/rd2coding/Road2Coding" target="_blank"><img src="https://img.shields.io/badge/Gitee-r2coding-blue.svg"></a>
  <a href="https://space.bilibili.com/384068749" target="_blank"><img src="https://img.shields.io/badge/bilibili-哔哩哔哩-critical"></a>
  <a href="https://mp.weixin.qq.com/s/ePhaYezFblgt0NgbvtWqww" target="_blank">
    <img src="https://img.shields.io/badge/微信联系作者-WeChat-green.svg" alt="微信联系">
  </a>
</p>

---
# **Continuing...**
---

