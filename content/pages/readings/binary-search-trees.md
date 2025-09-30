---
content_type: page
description: This section  contains various implementations of different Binary Search
  Trees (BSTs).
draft: false
learning_resource_types:
- Readings
ocw_type: CourseSection
parent_title: Readings
parent_type: CourseSection
parent_uid: aa632f83-51fe-4fa5-8aaf-1513de806253
title: Binary Search Trees
uid: 3e392c68-f217-c957-d2c8-70f39c63c084
video_metadata:
  youtube_id: null
---
This page contains various implementations of different Binary Search Trees (BSTs).

## Simple BST (no balancing)

- {{% resource_link 7f7ba1c1-c85b-1ec4-bd58-965bfe791489 "bst.py (PY)" %}}
    - Features: insert, find, delete\_min, ASCII art
- {{% resource_link c73ee324-2ff0-adf8-38ad-898c415fd267 "bstsize.py (PY)" %}}
    - Imports and extends bst.py
    - Augmentation to compute subtree sizes
- {{% resource_link 866988d0-4cb0-99a0-77e8-e93f554c5b85 "bstsize_r.py" %}}
    - Recursive version from recitation for computing subtree sizes
    - Features: insert, find, rank, delete

## AVL tree

- {{% resource_link e1bf9149-5a4b-705a-5a77-94d01029d802 "avl.py (PY)" %}}
    - Imports and extends bst.py
    - Features: insert, find, delete\_min, ASCII art

## Testing

Both bst.py and avl.py (as well as bstsize.py) can be tested interactively from a UNIX shell as follows:

- python bst.py 10 — do 10 random insertions, printing BST at each step
- python avl.py 10 — do 10 random insertions, printing AVL tree at each step

Alternatively, you can use them from a Python shell as follows:

```plaintext
>>> import bst
>>> t = bst.BST()
>>> print t

>>> for i in range(4):
...   t.insert(i);
...
>>> print t
0
/\
 1
 /\
  2
  /\
   3
   /\
>>> t.delete_min()
>>> print t
1
/\
 2
 /\
  3
  /\
```

```plaintext
>>> import avl
>>> t = avl.AVL()
>>> print t

>>> for i in range(4):
...   t.insert(i);
...
>>> print t
  1
 / \
0  2
/\ /\
    3
    /\
>>> t.delete_min()
>>> print t
  2
 / \
1  3
/\ /\
```