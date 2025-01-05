---
layout: post
title: "Computational Thinking"
author: "Harper"
date: "2024-12-23"
categories: python
---

I recently completed a course on computational thinking for problem solving. The course covered the pillars of computational thinking, types of searches and complexity as well as how to express and analyze algorithms. It went on the give a brief history of the modern computer to help better understand how computers access information and carry out processes. We learned about von Neuman Architecture Control Flow and how to write Psuedocode. The course used all of these components as the basis of understanding and writing python code. 

I took the time to create secrect gists through github to embed my answers so that they are not readily available to the general public through search. Please feel free to navigate to any given section using the table of contents below. Or, simply scroll through to follow the progression of the course.

- [Applying Pillars of Computational Thinking](#applying-pillars-of-computational-thinking)
- [Creating Algorithmic Flowchart](#creating-algorithmic-flowchart)
- [Binary Search](#binary-search)
- [Greedy vs. Brute Force Algorithms](#greedy-vs-brute-force-algorithms)
- [Describing Algorithms Using a Flowchart](#describing-algorithms-using-a-flowchart)
- [von Neumann Architecture Data and Instructions](#von-neumann-architecture-data-and-instructions)
- [Writing Pseudocode](#writing-pseudocode)
- [Writing Python Code](#writing-python-code)
- [Python code to Search for Words in a Corpus](#python-code-to-search-for-words-in-a-corpus)




## Applying Pillars of Computational Thinking  

### Counting Words and their Synonyms in Corpus  
Apply the four pillars of computational thinking.  

#### Describe Problem

<script src="https://gist.github.com/MelissaHarper/e32bc9255aacca65d76d0950c349bb23.js"></script>

#### Decomposition

<script src="https://gist.github.com/MelissaHarper/7ad2e7713cb253cd5cc5939e88374ddf.js"></script>

#### Pattern Recognition

<script src="https://gist.github.com/MelissaHarper/b3bd2883f59a90596c330c606ce172e9.js"></script>

#### Data Abstraction and Representation

<script src="https://gist.github.com/MelissaHarper/76aace02b076fc5b0dc751c97e6c2794.js"></script>

#### Algorithm

<script src="https://gist.github.com/MelissaHarper/2127bd602dd62a400b8f8aa358165b0f.js"></script> 

## Creating Algorithmic Flowchart

### Finding Minimum Values

#### Directions:

Draw a flowchart for an algorithm (described below) that will find the two smallest values in the collection. You can assume that the collection has at least two values and that all values are distinct.

The algorithm works by making one pass through the collection and keeping track of the two smallest values seen so far:

1. Initialize the two smallest values so far to be the first two elements of the list

2. For each remaining value in the list: if the value is smaller than either of the two smallest so far, replace the larger of the two  

#### Flowchart:

![*Flowchart for finding the two smallest values in a list*](/assets/article_images/2024-12-23-computational_thinking/two_smallest_values.png)  

  
  
## Binary Search

### City Changes Over a Five Year Period

The table below shows five-year changes in crime rate, income, and population for 15 different neighborhoods in the city of Philadelphia, Pennsylvania (
https://nextcity.org/features/view/philadelphia-neighborhoods-gentrification-mapping-growth
).

![*Chart containing data of philidelphia gentrification mapping growth*](/assets/article_images/2024-12-23-computational_thinking/Philadelphia five year crime-income-population changes.JPG)  


#### Question 1:  
A city planner would like to know the change in crime rate for Kensington. Using linear search to find it in the list, she would first look at Bella Vista, then Center City, then Chestnut Hill, etc. and compare each to "Kensington" before finally finding it after six comparisons. If she were instead to use binary search, which entries in the list would she need to compare to "Kensington" before finding it?  

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/299c6f501377bdea3a9090f9989f4a3d.js"></script>



#### Question 2:  

Now the city planner would like to see the change in population for Roxborough. Using binary search, which entries in the list would she need to compare to "Roxborough" before determining it's not there?  

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/791bdf01fdb798c510bf6c21c1ef2b7e.js"></script> 



#### Question 3:  

For which neighborhoods in the listing above would linear search require fewer comparisons than binary search?  

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/3c2336a34f14e711b9c9a5c3cb4038de.js"></script> 



#### Question 4:  

For the 15 neighborhoods in the listing above, what is the average number of comparisons required to find an element using linear search? Binary search?  

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/51a15baa8dbc4525074f0e78a34ed660.js"></script>



#### Question 5:  

Binary search assumes that the elements are already in sorted order; if they’re not sorted, then it would take time to sort them before applying binary search, and sorting is a slow operation.

Whereas the number of operations in the worst case to conduct binary search on a collection of N items is approximately log~2~***N***, the number of operations required to sort N items is ***N***log~2~***N***, i.e., the value of N times its base-2 logarithm.

Assume you have an unsorted collection of N = 15 items, and you know you will need to make at least 10 searches and are concerned about the worst case. How would you determine whether it would be better to conduct 10 linear searches, or if it would be better to first sort the elements (which you only need to do once) and then conduct 10 binary searches?

In making this decision, follow these steps:

  1. First, calculate the number of operations it would require to conduct 10 linear searches in the worst case.

  2. Then, calculate the number of operations it would require to sort the elements once; for simplicity, round log~2~***15*** up to 4.

  3. Last, calculate the number of operations it would require to conduct 10 binary searches in the worst case.

  4. Use the results of these three calculations to determine the answer.

Show the results of your calculations for steps 1-3 and then explain your answer in step 4.  

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/e39ada22c60ca22ba2753da436db5de7.js"></script>



## Greedy vs. Brute Force Algorithms  

### Traveling the Shortest Distance  

You are at home and need to run errands at the post office, grocery store, and bank. You want to visit each of those three places once and get back home with as little traveling as possible.

  The distances between the locations are shown in the diagram below, which is not to scale: 

![*Image showing buildings and numbers representing distances between them*](/assets/article_images/2024-12-23-computational_thinking/Traveling the Shortest Distance.JPG)  


#### Question 1:  

Describe a greedy algorithm that you would use in solving this problem by stating the decision you would make at each step of the algorithm, and how you would make it. 

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/ff4ae0646a52ae1d3eb47782b076f838.js"></script> 


#### Question #2:  

Using your greedy algorithm, in which order would you visit the three locations? What is the total distance traveled? Keep in mind you that you start at home and need to get back home after visiting all three locations.

#### Answer

<script src="https://gist.github.com/MelissaHarper/885a693bc5ac27081871615209813ad2.js"></script> 


#### Question #3:  

In your greedy algorithm, how many comparisons did you need to make in order to find a solution? If you added a fourth location to this problem, how many  comparisons would you need to make? How would you estimate the total number of comparisons this algorithm would require if there were N locations? 

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/ce787b5b95ccb8ef651fd7d86be65984.js"></script> 


#### Question 4:  

How many different possible solutions would you need to consider using a brute force approach?

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/b4f54c51ffb44095a7fd30f384958b00.js"></script>


## Describing Algorithms Using a Flowchart  


### Corpus Search Flowchart

Building on the breakdown of Counting Words and their Synonyms in Corpus we completed earlier, going into more depth and detail, create a flowchart for the algorithm.

In creating the flowchart, you should use the following terms to represent the data:

   - ***Keyword***, which is the word to search for

  - ***Thesaurus***, which is a collection of ***Entries***, each of which consists of a ***Word*** and a corresponding collection called ***Synonyms***, which is a collection of one or more Words

  - ***Corpus***, which is a collection of ***Documents***, each of which is a collection of Words
  
You should use the results of the computational thinking pillar of decomposition to design your flowchart. In particular, your flowchart should have two major parts:

1. Finding the synonyms in the the thesaurus 
2. Counting the number of occurrences of the keyword and its synonyms in the corpus.

For simplicity, you may assume that:

  - The Thesaurus is not empty, i.e., there is at least one Entry in the Thesaurus.

  - The Corpus is not empty, i.e., there is at least one Document in the Corpus.

  - Each Document contains at least one Word.

![*Flowchart for searching corpus for keywords*](/assets/article_images/2024-12-23-computational_thinking/corpus_search_flowchart.png)  


##  von Neumann Architecture Data and Instructions

### Writing Assembly Language

#### Question 1:  

Multiply the variable A times the variable B and storing the result in variable C.  

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/e1781f0b7d325678a7c668e4cf16859e.js"></script>

#### Question 2:  

Fill in the blanks with the necessary instructions for adding X and Y, then subtracting Z, and storing the result in M. 

#### Answer (in ***):  

<script src="https://gist.github.com/MelissaHarper/c346f48e2dad8767770f18035996ca19.js"></script>


#### Question 3:  

Assume that memory holds values for variables A, B, and T. Fill in the blanks with the necessary instructions for calculating A^3^ + B^2^ and store the result in C. Note that your program should not modify A or B, but it is okay to modify T, which we’ll consider a temporary variable; you should not use any variables other than A, B, C, and T. 


#### Answer (in ***):  

<script src="https://gist.github.com/MelissaHarper/f9f00c4b3f8fbd27218da9b86c3068af.js"></script>


## Writing Pseudocode  


### Reading and Writing Pseudocode

#### Question 1:  

Initialize the two smallest values so far to be the first two items of the list

  1. For each remaining value in the list: if the value is smaller than either of the two smallest so far, replace the larger of the two

  2. In this case, the input is a list of items and the outputs are min1 and min2, which are the two smallest items, not necessarily in that order.


#### Answer:  

<script src="https://gist.github.com/MelissaHarper/e9a9e515ea588e11e94f16c5eb0ecb5e.js"></script>


### Pseudocode to Search for Words in a Corpus  


#### Directions:  

Write the pseudocode for your Corpus algorithm, based on the flowchart and using the notation we have seen in previous activities. 

In writing the pseudocode, you should use the following variables:

  - Keyword, which is the word to search for

  - Thesaurus, which is a collection of Entry objects, each of which has a Word attribute and an attribute called Synonyms, which is a collection of Word objects

  - Corpus, which is a collection of Document objects, each of which is a collection of Word objects
  
In your answer, please use underscores ("_") to indent lines and use "<--" to represent an assignment operator 
  
#### Answer:  

<script src="https://gist.github.com/MelissaHarper/301c407160bc903ec4133d02bb27fcbb.js"></script>


## Writing Python Code  


### Lists

#### Directions:  

Write your code so that it does the following:
  - See if the last element of the list is negative. If so, remove it from the list.
  - Then, calculate the average of the first two elements of the list and insert it in between those two elements.
  - Last, swap the values of the first and last elements of the list.

At the end of the program, print out the list using “print(values)”. In this case, it should print [9,
5.5, 7, 8, 4]

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/2ee924e8ab3525549e7f6566e3685fd1.js"></script>


### Loops  


####  Directions 1:  

Write code in Python so that it correctly sets the values of min1 and min2, which should hold the two smallest values in the list provided, though not necessarily in that order. 

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/da3758b42c312bebd88ac6c3b9ed29da.js"></script>

### Functions

#### Directions:  

Implement a function that determines whether or not a card number is valid, according to some simple algorithms. Call the function “verify” that takes a single parameter called “number” and then checks the following rules:

  1. The first digit must be a 4.
  2. The fourth digit must be one greater than the fifth digit; keep in mind that these are separated by a dash since the format is ####-####-####.
  3. The sum of all digits must be evenly divisible by 4.
  4. If you treat the first two digits as a two-digit number, and the seventh and eighth digits as a two-digit number, their sum must be 100.
  
The rules must be checked in this order, and if any of the rules are violated, the function should return the violated rule number.  

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/1f8ffcceed5aaa231bc1316c4488aa36.js"></script>


### Classes and Objects  


#### Directions:  

Implement the “highest_turnout” function so that it does the following:

  - First, find the County that has the highest turnout, i.e. the highest percentage of the population who voted, using the objects’ population and voters attributes   - Then, return a tuple containing the name of the County with the highest turnout and the percentage of the population who voted, in that order; the percentage   should be represented as a number between 0 and 1

#### Answer:  


<script src="https://gist.github.com/MelissaHarper/e38953a5caef80a795a37b11dd9d5d79.js"></script>


## Python code to Search for Words in a Corpus  


### Final Project

#### Directions:  

Implement a python "search" function to search for words in a corpus. The output of your function should be a list of tuples, in which the first element of the tuple is the word that was searched for (either the keyword or one of its synonyms) and the second is its total number of occurrences. 

#### Answer:  

<script src="https://gist.github.com/MelissaHarper/33aee78aa4dfb84e818185ca41230352.js"></script>
