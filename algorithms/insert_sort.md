# Insert Sort

Get one element of the **list** and compare it to the **left** and to the **right**.
We start with the **second** element *(index=1)*, because the **first** element *(index=0)* is considered **sorted** at the start.

## Time Complexity:

- Best case *(already sorted)*: $$O(n)$$

- Average and Worst case: $$O(n^2)$$

- Space Complexity *(in-place)*: $$O(1)$$


**List:** `[5, 2, 4, 1]`

1. So we get `2`.

    So we **loop** through all **sorted** elements so far.

    These elements are all the **previous** elements.

    When we **loop** through them, we check to see where to **insert** our element `2`.

    **list**: `[2, 5, 4, 1]`.

2. Then sort element `4` in `[2, 5, 4, 1]`.

    **list**: `[2, 4, 5, 1]`.

3. Then sort element `1` in `[2, 4, 5, 1]`.

    **list**: `[1, 2, 4, 5]`.

**Sorted!**

>*Note: This looks like ***bubble*** sort, but the difference is that you ***insert*** the element in the sorted part of the array.*

*See https://www.geeksforgeeks.org/insertion-sort-algorithm/*

## Problem to solve

This is like **insert** sort, but here we only **insert** the `last_num` into this **mostly** sorted array.

- `n` - the number of elements in `arr`, basically `arr.size()`.

- `arr` - list of numbers.

The goal here is to **insert** `last_num` into its place in the *array*. As it is *mostly* sorted. The *last* number in the *array* is the number that needs to be sorted. 

This **loop** is done in *reverse*, so you may try to think in a *different* way.

## Solution

```java
public static void insertionSort1(int n, List<Integer> arr) {
    // get last_num to insert
    int last_num = arr.get(0);

    // start from pre-last number
    for (int i = n - 2; i >= 0; i--) {

        // get current element
        int current_num = arr.get(i);

        // check if we have found where to insert
        if (current_num < last_num) {
            arr.set(i + 1, last_num);

            // you have done your job sorting. break.
            break;
                
        } else {

            // you are still searching for a place to insert it
            arr.set(i + 1, current_num);
            if (i == 0) {

                // that is probably the smallest element of all
                arr.set(0, last_num);
            }
        }
    }
}
```
