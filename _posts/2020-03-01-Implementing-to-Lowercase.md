---
layout: post
title: Implementing toLowercase()
---

Many languages are case sensitive, so there should be difference between uppercase and lowercase letters. The ASCII value difference between uppercase and lower case letters in alphabet is 32.

Uppercase A has ASCII value 65 in decimal .So for Z ,the value is 90 in decimal.

Lowercase a has ASCII value 97 in decimal .So for z ,the value is 122 in decimal.

Taken from [https://www.quora.com/What-is-difference-in-the-ASCII-values-of-uppercase-and-lowercase-letters](https://www.quora.com/What-is-difference-in-the-ASCII-values-of-uppercase-and-lowercase-letters)

```java

class Solution {
    public String toLowerCase(String str) {
        char[] chars = str.toCharArray();
        for(int i=0; i<=chars.length-1;i++){
            chars[i] = (char) ((chars[i] >= 65 && chars[i] <= 90) ? chars[i] + 32 : chars[i]); 
        }
        return String.valueOf(chars);
    }
}
```

Time complexity: O(n) <br>
Space complexity: O(1) <br>

Created to solve: [https://leetcode.com/problems/to-lower-case/](https://leetcode.com/problems/to-lower-case/)

<hr/>

01 March 2020