---
layout: default
title: Jupyter test
categories: removed
---
Note (manually written): Jupyter to markdown conversion successful. However, the workflow isn't very smooth, and I'm not yet sure how to fix the styling problems.
```java
import java.util.ArrayList; // import package

ArrayList<String> math = new ArrayList<String>(); // new arraylist

List<String> basic = new ArrayList<String>(); // 2 lists to be added to arraylist
List<String> advanced = new ArrayList<String>();

basic.add("addition"); // adding these items to basic list
basic.add("subtraction");
basic.add("multiplication");
basic.add("division");

advanced.add("square root"); // adding to advanced list
advanced.add("exponent");
advanced.add("logarithm");

math.addAll(basic);
math.addAll(advanced); // adding 2 lists to math

System.out.println("Original: " + math); // math has all items from both
```
