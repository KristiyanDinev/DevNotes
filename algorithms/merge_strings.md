# Merge Strings Algorithm

## Intuition
<!-- Describe your first thoughts on how to solve this problem. -->

## Approach
<!-- Describe your approach to solving the problem. -->

## Complexity
- Time complexity:
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity:
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

## Code
```java []
class Solution {
    public String mergeAlternately(String word1, String word2) {
        String merged = "";

        int word1Length = word1.length();
        int word2Length = word2.length();
        for (int i = 0; i < Math.max(word1Length, word2Length); i++) {
            if (i < word1Length) {
                merged += String.valueOf(word1.charAt(i));
                if (i < word2Length) {
                    merged += String.valueOf(word2.charAt(i));
                }
            } else {
                merged += String.valueOf(word2.charAt(i));
                if (i < word1Length) {
                    merged += String.valueOf(word1.charAt(i));
                }
            }
        }
        return merged;
    }
}
```