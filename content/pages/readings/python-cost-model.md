---
content_type: page
description: This section reviews various Python primitive operations to determine
  bounds and/or estimates on their running times.
draft: false
learning_resource_types:
- Readings
ocw_type: CourseSection
parent_title: Readings
parent_type: CourseSection
parent_uid: aa632f83-51fe-4fa5-8aaf-1513de806253
title: Python Cost Model
uid: 2e91de4c-23a9-f323-866d-265121e71f07
video_metadata:
  youtube_id: null
---
Python is a high-level programming language, with many powerful primitives. Analyzing the running time of a Python program requires an understanding of the cost of the various Python primitives.

For example, in Python, you can write:

L = L1 + L2

where L, L1, and L2 are lists; the given statement computes L as the concatenation of the two input lists L1 and L2. The running time of this statement will depend on the lengths of lists L1 and L2. (The running time is more-or-less proportional to the sum of those two lengths.)

Our goal in this section is to review various Python primitive operations, and to determine bounds and/or estimates on their running times. Our approach will involve both a review of the relevant Python implementation code, and also some experimentation (analysis of actual running times and interpolating a nice curve through the resulting data points).

## Python Running Time Experiments and Discussion

The running times for various-sized inputs were measured, and then a least-squares fit was used to find the coefficient for the high-order term in the running time. (Which term is high-order was determined by some experimentation; it could have been automated…)

The least-squares fit was designed to minimize the sum of squares of relative error, using scipy.optimize.leastsq.

(Earlier version of this program worked with more than the high-order term; they also found coefficients for lower-order terms. But the interpolation routines tended to be poor at distinguishing *n* and *n* lg *n*. Also, it was judged to be more interesting to work with relative error than with absolute error. Finally, it seemed that looking at only the high-order term, and studying only the relative error, seemed simplest.)

The machine used was an IBM Thinkpad T43p with a 1.86GHz Pentium M processor and 1.5GB RAM.

- {{% resource_link e989d4e7-b6bc-a1b7-47a5-1e1a14d07437 "Timing code (PY)" %}}
- {{% resource_link 5b1e2569-373c-58a4-e6bd-0dd1112a9ba7 "Sample output (TXT)" %}}

\[This output may have results somewhat different than in the charts below, due to random run-time variations…\]

### **Cost of Python Integer Operations**

x,y, and z are n-bit numbers, w is an 2n-bit number, s is an n-digit string

{{< tableopen >}}{{< tbodyopen >}}{{< tropen >}}{{< tdopen >}}
**Convert string to integer**
{{< tdclose >}}{{< tdopen >}}
int(s)
{{< tdclose >}}{{< tdopen >}}
84 \* (n/1000)^2 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 8000  
{{< tdclose >}}{{< tdopen >}}
6% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Convert integer to string**
{{< tdclose >}}{{< tdopen >}}
str(x)
{{< tdclose >}}{{< tdopen >}}
75 \* (n/1000)^2 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 8000  
{{< tdclose >}}{{< tdopen >}}
3% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Convert integer to hex**
{{< tdclose >}}{{< tdopen >}}
"%x"%x
{{< tdclose >}}{{< tdopen >}}
2.7 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
19% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Addition (or Subtraction)**
{{< tdclose >}}{{< tdopen >}}
x+y
{{< tdclose >}}{{< tdopen >}}
0.75 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 512000  
{{< tdclose >}}{{< tdopen >}}
8% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Multiplication**
{{< tdclose >}}{{< tdopen >}}
x\\\*y
{{< tdclose >}}{{< tdopen >}}
13.73 \* (n/1000)^1.585 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
10% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Division (or Remainder)**
{{< tdclose >}}{{< tdopen >}}
w/x
{{< tdclose >}}{{< tdopen >}}
47 \* (n/1000)^2 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 32000  
{{< tdclose >}}{{< tdopen >}}
6% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Modular Exponentiation**
{{< tdclose >}}{{< tdopen >}}
pow(x,y,z)
{{< tdclose >}}{{< tdopen >}}
60000 \* (n/1000)^3 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 4000  
{{< tdclose >}}{{< tdopen >}}
8% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**n-th power of two**
{{< tdclose >}}{{< tdopen >}}
2\*\*n
{{< tdclose >}}{{< tdopen >}}
0.06 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 512000  
{{< tdclose >}}{{< tdopen >}}
10% rms error
{{< tdclose >}}{{< trclose >}}{{< tbodyclose >}}{{< tableclose >}}

It is perhaps curious that multiplication is implemented using Karatsuba's algorithm, giving an Θ(*n*{{< sup "lg 3" >}}) running time, while division uses an Θ(*n*{{< sup "2" >}}) algorithm.

### **Cost of Python String Operations**

s and t are length-n strings, u is length (n/2)

{{< tableopen >}}{{< tbodyopen >}}{{< tropen >}}{{< tdopen >}}
**Extract a byte from a string**
{{< tdclose >}}{{< tdopen >}}
s\[i\]
{{< tdclose >}}{{< tdopen >}}
0.13 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 512000  
{{< tdclose >}}{{< tdopen >}}
29% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Concatenate**
{{< tdclose >}}{{< tdopen >}}
s+t
{{< tdclose >}}{{< tdopen >}}
1 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 256000  
{{< tdclose >}}{{< tdopen >}}
19% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Extract string of length n/2**
{{< tdclose >}}{{< tdopen >}}
s\[0:n2\]
{{< tdclose >}}{{< tdopen >}}
0.3 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 256000  
{{< tdclose >}}{{< tdopen >}}
28% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Translate a string**
{{< tdclose >}}{{< tdopen >}}
s.translate(s,T)
{{< tdclose >}}{{< tdopen >}}
3.2 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 256000  
{{< tdclose >}}{{< tdopen >}}
11% rms error
{{< tdclose >}}{{< trclose >}}{{< tbodyclose >}}{{< tableclose >}}

### **Cost of Python List Operations**

L and M are length-n lists, P has length n/2

{{< tableopen >}}{{< tbodyopen >}}{{< tropen >}}{{< tdopen >}}
**Create an empty list**
{{< tdclose >}}{{< tdopen >}}
list()
{{< tdclose >}}{{< tdopen >}}
0.40 microseconds
{{< tdclose >}}{{< tdopen >}}
(n=1)
{{< tdclose >}}{{< tdopen >}}
.5% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Access**
{{< tdclose >}}{{< tdopen >}}
L\[i\]
{{< tdclose >}}{{< tdopen >}}
0.10 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 640000  
{{< tdclose >}}{{< tdopen >}}
3% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Append**
{{< tdclose >}}{{< tdopen >}}
L.append(0)
{{< tdclose >}}{{< tdopen >}}
0.24 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 640000  
{{< tdclose >}}{{< tdopen >}}
3% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Pop**
{{< tdclose >}}{{< tdopen >}}
L.pop()
{{< tdclose >}}{{< tdopen >}}
0.25 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000 
{{< tdclose >}}{{< tdopen >}}
0.5% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Concatenate**
{{< tdclose >}}{{< tdopen >}}
L+M
{{< tdclose >}}{{< tdopen >}}
22 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
3% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Slice extraction**
{{< tdclose >}}{{< tdopen >}}
L\[0:n2\]
{{< tdclose >}}{{< tdopen >}}
5.4 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
4% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Copy**
{{< tdclose >}}{{< tdopen >}}
L\[:\]
{{< tdclose >}}{{< tdopen >}}
11.5 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
10% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Slice assignment**
{{< tdclose >}}{{< tdopen >}}
L\[0:n2\] = P
{{< tdclose >}}{{< tdopen >}}
11 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
4% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Delete first**
{{< tdclose >}}{{< tdopen >}}
del L\[0\]
{{< tdclose >}}{{< tdopen >}}
1.7 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
4% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Reverse**
{{< tdclose >}}{{< tdopen >}}
L.reverse()
{{< tdclose >}}{{< tdopen >}}
1.3 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
10% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Sort**
{{< tdclose >}}{{< tdopen >}}
L.sort()
{{< tdclose >}}{{< tdopen >}}
0.0039 \* n lg(n) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
12% rms error
{{< tdclose >}}{{< trclose >}}{{< tbodyclose >}}{{< tableclose >}}

The **first** time one appends to a list, there is additional cost as the list is copied over and extra space, about 1/8 of the list size, is added to the end. Whenever the extra space is used up, the list is re-allocated into a new array with about 1.125 the length of the previous version.

### **Cost of Python Dictionary Operations**

D is a dictionary with n items

{{< tableopen >}}{{< tbodyopen >}}{{< tropen >}}{{< tdopen >}}
**Create an empty dictionary**
{{< tdclose >}}{{< tdopen >}}
dict()
{{< tdclose >}}{{< tdopen >}}
0.36 microseconds
{{< tdclose >}}{{< tdopen >}}
(n=1)
{{< tdclose >}}{{< tdopen >}}
0% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Access**
{{< tdclose >}}{{< tdopen >}}
D\[i\]
{{< tdclose >}}{{< tdopen >}}
0.12 microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
3% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**Copy**
{{< tdclose >}}{{< tdopen >}}
D.copy()
{{< tdclose >}}{{< tdopen >}}
57 \* (n/1000) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
27% rms error
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
**List items**
{{< tdclose >}}{{< tdopen >}}
D.items()
{{< tdclose >}}{{< tdopen >}}
0.0096 \* n lg(n) microseconds
{{< tdclose >}}{{< tdopen >}}
n \<= 64000  
{{< tdclose >}}{{< tdopen >}}
14% rms error
{{< tdclose >}}{{< trclose >}}{{< tbodyclose >}}{{< tableclose >}}

What should the right high-order term be for copy and list items? It seems these should be linear, but the data for both looks somewhat super-linear. We've modelled copy here as linear and list items as n lg(n), but these formulae need further work and exploration.