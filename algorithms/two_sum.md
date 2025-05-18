# Two Sum (LeetCode problem)

## Intuition
Just replacing the limited **for** loop that has 1 variable and track with it a single variable with fixed amount of tries of the second variable. Basically if you have a single **i** , which you can use to get that number, but what about the second number? We need a variable for it as well. For as you see in the testcases that the numbers in the 
**int[] nums** are not tested only next to each other, but they can jump from number to number. We can implement a solution by using the **while** keyword, so we can introduce the option for us to use 2 variables or however many variables we want and are not limited to one **i**. I mean you can use the **for** loop with the **i**, but also you got to manage the rest of the variables based on that **i**. So I think both are a good solution, but I guess if you have a more complex system, then you will need some more cleaner and maintainable code for it.

## Approach
Instead of having a for loop with a single **i** that increases.
We can have 2 variables and handle them in a **while** loop.
These 2 variables will be the indexes of the 2 numbers that we need to compare.
Of course that we have to add the ifs and other checks for valid indexes.

## Complexity
*Time complexity - the amount of time it takes the algorithm to handle all the inputs.*
- Time complexity: $$O(n)$$

*Space complexity - the amount of memory it take the algorithm to succeed based on the inputs. For example the more inputs, the more memory it will require.*
- Space complexity: $$O(1)$$

*$$O(1)$$ because it uses fixed amount of space regarding the input.*

## Code
```java 
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] sum = {-1, -1};
        int lastNumIndex = nums.length - 1;

        // the indexes should not be equal
        int firstNumIndex = 0;  // the first number index
        int secondNumIndex = 1; // the second number index
        while (sum[0] == -1) {
            if (nums[firstNumIndex] + nums[secondNumIndex] == target) {
                sum[0] = firstNumIndex;
                sum[1] = secondNumIndex;
                break;
            }

            if (secondNumIndex == lastNumIndex) {
                secondNumIndex = ++firstNumIndex + 1;

            } else {
                secondNumIndex++;
            }
        }
        
        return sum;
    }
}
```

I think this code may also work well for other programming languages like: `JavaScript`, `C/C++`, `C#` and maybe a bit `Python`.
