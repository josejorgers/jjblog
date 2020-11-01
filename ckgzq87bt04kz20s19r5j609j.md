## Improving linked lists

You know the situation is tricky when you need more than just arrays and dictionaries. That was my case a couple of months ago. I needed an indexed data structure that supported insertion and removal operations as fast as possible.

With an array, I can get the i-th element really fast (this will be called the _index_ operation. I provide an index ```i``` and get the ```i-th``` element of the list), no matter what the value of i is. But when it comes to inserting or removing an element from the middle of the array things change a little bit.

On the other hand, linked lists offer a slower index operation. We need to traverse a long segment of the list to get the element we are looking for. But we can make faster insertions and deletions.

In this article, I will write about a solution I came up with. This is a "new" data structure that somehow combines arrays and linked lists to get a middle point that suited my requirements. I don't know whether this is a truly novel data structure. Furthermore, I don't think it is an awesome idea. I just used some clever and relatively popular techniques to achieve my goal.

The next section is about the differences between arrays and linked lists, their respective strengths, and their weaknesses. If you are just interested in the data structure specifics, you can skip it. Then I'll talk about the core idea, and later, I'll give some implementation details.

>IMPORTANT!
I assume you have some basic knowledge about algorithmic complexity and big-O notation.

## Arrays vs. Linked Lists

Arrays are contiguous memory slots to store sequences of data. We arrange some elements in such a way that we keep a reference to every single one of them. For example, if we have an array that contains numbers from 1 to 10, we have a reference to every memory slot that contains each of those ten numbers. That allows us to get any element by the index it has in the array really fast.

```python

# Example of array 
array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

array[3]
# >> 4

array[0]
# >> 1
```

We have an implicit mapping from index to element. No matter the value of the index, the operation will always be equally fast. That means that doing ```array[0]``` is as fast as doing ```array[9]``` because we keep all the references at the same time. That's the most useful feature of arrays, we have access to any position in constant time, no matter the size of the array, nor the position itself.

This great feature is also the reason we can't insert or delete elements with the same ease. We need to keep elements in contiguous positions and maintain the references to them. When deleting an element, we are creating holes in the middle of the array, and that is not allowed. So we need to fix the references manually and fill the holes. The same happens with insertions, in this case, we need to create the hole first and then, fill it with the new element.

Thus, to insert/delete an element, we need to rearrange the array. If we have an array with ```N``` elements, we'll need to rearrange ```O(N)``` of those elements when doing an insertion/deletion. I needed to do it faster.

Linked lists don't maintain all those references in the same way. They are composed of nodes that store an element but also the reference to the next node. This way we only maintain a direct reference to the first node of the list (called the head), this node has a reference to the second one, the second one has a reference to the third one, and so on. In the case of doubly-linked lists, every node has a reference to both the next and the previous node, and we also keep a direct reference to the tail of the list (the last node).

Thus, looking for an element in a certain position implies to traverse ```O(N)``` nodes in the worst case. The good thing is that we can insert or delete an element without worrying about the holes because we only need to change a few references. But I needed to have access to positions in the lists in a faster way.

I needed to make faster insertion/deletion (better than ```O(N)```), but yet be able to retrieve the ```i-th``` element of the list in less than ```O(N)``` operations as well. Is important to note that when I say insertion I mean an operation that receives an element and __a position__, and then inserts the element in that position. The delete operation is defined in the same way.

In the next section, I'll introduce a data structure that lets us fulfill such requirements.

## An Indexed Linked List

As we saw in the previous section, the "problem" with arrays is that having all those references make the insertion/deletion slow. But the few references of linked lists are a problem as well, because we need to make a lot of operations to get the element at a given position. Thus, it seems reasonable to think about finding a middle point in the number of references.

First of all, I need to say that no data structure lets us make all the index, insert, and delete operations in constant time. So, a "perfect world" is out of the discussion. Let's move on.

I was thinking about having a doubly-linked list with more direct references. Instead of just keeping a reference to the first and the last node, I will keep also references to inner nodes, but how many references would I need? If I maintained just a few of them, the index operation might be slow, but if I maintained too many references, then insertions and deletions would take more time.

After thinking for a while, I came up with the right number of references: ```sqrt(N)``` (here ```sqrt``` stands for the square root operation). The idea is to keep a list of references, the length of that list will be ```sqrt(N)```. The other important property is that every referenced node will be at a distance of ```sqrt(N)``` from the next node in the reference list.

For example, suppose we have a doubly-linked list with the numbers ```1, 2, 3, 4, 5```. Then our reference list will contain two references because of ```2 < sqrt(5) < 3```. Also, every node referenced in the list will be at a distance of ```sqrt(5)``` of the other node referenced in the list. So a possible list will be one that contains the second and the fourth node. Another possibility would be a list with the third and the fifth nodes. In both cases, the distance between nodes is less than ```sqrt(5)```.

> When I say ```sqrt(N)``` I mean the largest integer ```n``` such that ```n <= sqrt(N)```

By keeping such a reference list we can make all the index, insert, and delete operations in ```O(sqrt(N))```. Note that we got a slower index operation to achieve a faster insertion/deletion.

>By the way, I borrowed some ideas from a technique called __sqrt decomposition__. You can check it out [here](https://cp-algorithms.com/data_structures/sqrt_decomposition.html).

Now I'm going to explain the insert, delete, and index operations.

## Inserting an element

As I said, I maintain a list of selected nodes. The length of this list is always ```sqrt(N)``` and the distance between two consecutive selected nodes in the actual list will be also ```sqrt(N)```.

Every time we insert a new node in the list we need to check whether the new length of the list has a new integer ```sqrt(N)```. In that case, we need to update the selected nodes list. It would be like:

```python
import math

class IndexedLinkedListNode:
    def __init__(self, content, prev=None, next=None):
        self.content = content
        self.prev = prev
        self.next = next
   
    def __eq__(self, other):
        return self.content == other.content


class IndexedLinkedList:

    def __init__(self, elements=[]):
        self.selected_nodes = []
        self.length = 0
        self.head = None
        self.tail = None
    
        for e in elements:
             self.insert(e)

    def insert(self, element):
        '''
        Inserts an element at the end of the list
        '''
        new_node = IndexedLinkedListNode(element)

        sqlen = math.floor(math.sqrt(self.length))

        if self.length == 0:
            self.selected_nodes.append(new_node)
            self.head = new_node
            self.tail = new_node

        else:
            self.tail.next = new_node
            new_node.prev = self.tail
            self.tail = new_node
            self.length += 1
            if math.floor(math.sqrt(self.length)) > sqlen:
                 self.update_list_cause_insertion(new_node)

    def update_list_cause_insertion(self, new_node):
          '''
          Here we make every selected node become its next node in the list and add the 
          new node to the list
          '''
            
          self.selected_nodes = list(map(lambda node : node.next, self.selected_nodes))
          self.selected_nodes.append(new_node)
   
    def insert_at(self, element, index):
        '''
        Here we insert the new node before the node that is in the specified position.
        You can implement yourself a very similar method to insert the new element 
        AFTER the specified position.
        '''
       
       new_node = IndexedLinkedListNode(element)
       target = self[index]
       sqlen = math.floor(math.sqrt(self.length))
       self.length += 1

       if index == 0:
           self.head.prev = new_node
           new_node.next = self.head
           self.head = new_node
           
       else:
            target.prev.next = new_node
            new_node.prev = target.prev
            new_node.next = target
            target.prev = new_node
      
       if math.floor(math.sqrt(self.length)) > sqlen:
           self.update_list_cause_insertion(self.tail)
```

It can be proved that we always obtain a selected nodes list of length ```sqrt(N)``` and with at most ```sqrt(N)``` distance between any pair of consecutive nodes. I'm going to skip the demonstration to keep the article as simple as possible.

## Deletion

This is a very similar method. Is like doing the inverse operation. We just need to take care of some corner cases like deleting the only node in the list and deleting the first or the last node.


```python
...

    def remove_at(self, index):
        target = self[index]
        sqlen = math.floor(math.sqrt(self.length))
        selected_pos = None
        try:
            selected_pos = self.selected_nodes.index(target)
        except:
             pass
        
        if self.length <= 1:
            self.head = None
            self.tail = None
            self.selected_nodes = []
            self.length = max(0, self.length - 1)
            return

        if target.prev:
            target.prev.next = target.next
        else:
            self.head = target.next
  
        if target.next:
             target.next.prev = target.prev
        else:
             self.tail = target.prev
   
        self.length -= 1
         
        if selected_pos:
            for selected in self.selected_nodes[selected_pos:]:
                 selected = selected.next
        if math.floor(math.sqrt(self.length)) < sqlen:
             self.update_list_cause_removal()

    def update_list_cause_removal(self):
          '''
          Here we make every selected node become its prev node in the list and remove 
          the last selected node
          '''
          self.selected_nodes = self.selected_nodes[:-1]
          self.selected_nodes = list(map(lambda node : node.prev, self.selected_nodes))
...
```

Until here we have achieved the requirements for insertion and deletion methods. But that is assuming that the index operation (```self[index]```) could be done also in the required time.

## Index

Let's see how to implement the index operation. This is the last piece of the puzzle. It is a trickier method that would require more explanation to understand the insights. As I said, I would like to keep this article as simple as possible so I'm not going to explain too many details about this operation.

```python
...

    def __getitem__(self, index):
    
        if len(self.selected_nodes) == 0 or index < 0 or index >= self.length:
            return None
        first_index = math.floor(math.sqrt(self.length)) - 1 # index of the first selected node
        first_node = self.selected_nodes[0]
        if index <= first_index:
             return self.prevn(first_node, first_index - index)
        aux_idx = index - first_index
        target_k = self.integer_positive_root(aux_idx)
        target_index = target_k**2 + target_k + first_index
        index -= target_index
        return self.nextn(self.selected_nodes[target_k], index)

    def integer_positive_root(self, index):
        '''
        Solves the equation K^2 + K - index
        ''' 

       return math.floor((-1 + math.sqrt(1 + 4*index))/2)
    
    ## Utility functions to make the prev and next operations N times
    
    def prevn(self, node, n):
        for _ in range(n):
            node = node.prev
        return node
     
    def nextn(self, node, n): 
         for _ in range(n):
            node = node.next
         return node
```

That's it. We have completed our Indexed Linked List implementation!

## Conclusions

In this article, we defined a new data structure that allows us to do insertions, deletions, and indexing in ```O(sqrt(N))``` where ```N``` is the length of the list.

I didn't want to go into the implementation details and proofs of correctness and time complexities because I wanted the article to be accessible for a wider audience. I know some parts of the code can be obscure right now. But I consider that talking about a problem and sharing the ideas to solve it is very helpful for everybody.

If you want me to make a more detailed article with all the formulas and demonstrations just let me know. You can react to this article and leave a comment, or you can follow me or @-me on Twitter to talk about this or any other CS-related topic.



