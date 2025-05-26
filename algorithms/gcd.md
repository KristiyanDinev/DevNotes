```java
static class Solution {
    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    public String gcdOfStrings(String str1, String str2) {

        // Check if both strings have the same substring.
        // str1 = ABABAB; str2 = ABAB; substring = AB;

        if (!(str1 + str2).equals((str2 + str1))) {
            return "";
        }

        // Now we know that they are made of one substring
            // Calculate the GCD of the lengths
        int gcdLength = gcd(str1.length(), str2.length());

        // Return the prefix up to the gcd length
        return str1.substring(0, gcdLength);
    }
}

```