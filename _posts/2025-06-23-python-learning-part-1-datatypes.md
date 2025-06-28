---
title: Python Learning, Part 1. Datatypes
date: 2025-06-23 15:41 +0700
categories: [Programming, Python, Learning]
tags: [programming, python, learning, datatypes, learning journal]
---

### Appreciation Words

This learning was possible because I got informed by this [GitHub repo](https://github.com/fmottamendes/Hacking-related-books/tree/master) and in it there's so many learning resources about cyber security, ethical hacking, network, programming and many more. And I stumbled upon e-book titled **"Full Speed Python"** by [**João Ventura**](https://joaoventura.net/), where his book teaches python but with such a simple way and hands-on practice too. What I also like about his book is that, it presented in such a simple way and really straight to the point. I really recommending reading his [book](https://joaoventura.net/books/).

Once again thank you so much for Sir João Ventura for this book and the learning material, you're awesome.

## Welcome!

This is my first attempt to write a learning journal about what I just learn when learning Python programming, new things that I just knows about python, debugging that I just learn, and many more. This one will be just short writing, so if you look for tutorial, I suggest to go to [GeeksforGeeks python tutorial](https://www.geeksforgeeks.org/python/python-programming-language-tutorial/), [W3Schools](https://www.w3schools.com/python/), or better yet google the heck out of it. 

So let's get started with what I just learn and maybe you could learn one or two things from my learning progress too.

## Some Stuff That I Learned

We just gonna go straight to the chapter 3 of the book. Because chapter 2 is just explaining how to installing python on various OS and because I already installing python on my device I just skipping it. Otherwise, if you guys still don't have python installed, follow along from chapter 2.

### Chapter 3: Datatypes
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/basic-datatypes.png){: width="800"}
_Chapter 3: Datatypes_

In this chapter we mainly focusing on declaring variable, learning basic of datatypes i.e. numbers and strings. So the things that I discovered when doing the exercises on this chapter is as follows:

- The basic of python variable is just the same with other programming language and it feels so simple too. Previously I was using **Kotlin** and I feels like python is more straight forward than it.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/declaring-variables.png){: width="650"}
_Variables in Python_

- This one really unique for me lol, I just knows in python you could multiply strings and the result would be the string multiplied. The more I know...
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/multiplying-strings.png){: width="650"}
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/string-multiplied-output.png){: width="650"}
_Multiplying String_

- This one is also interesting to me and I will always keep in mind. But before I explain, take a look of this picture.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/python-reading-code-from-up-to-bottom.png){: width="650"}
_Python Way of Reading Code_
The picture above shows that I have multiple line of codes from the previous attempt that I made, and when I try outputting the script, the process would be like this.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/python-output-with-multiple-codes.png){: width="650"}
_Python Way of Reading Code_
Python processing the mathematical operation on the upper part of codes and after that it shows the output which is `7` and then unto next process which is multiplying the string. And lastly it would stop at input process like picture above and for the next process would explained on the next point.

- Next, let's take a look of the string list below.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/processing-list-strings-and-concatenation.png){: width="650"}
_Python Processing String & Concatenation_
From that picture I just know that despite being on the list, the strings in it can be also be combined with the numerical operation such as multiply and plus. But at the same time, I know it could also be combined with the string that I inputted. Let's try I input ``Albert`` and let's see what the output would be like.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/string-list-process-output.png){: width="650"}
_Output of String List_
And look at that, the input being shown alongside the multiplied string from the list, in this case it's the string on the number 2 position. A little side note for me and maybe you, computer start counting from ``0``, so if you want to call something from the beginning, you have to put ``0`` not ``1`` because computer reads it as the second. And in that code of mine, I just called number ``2``, which is on the third position(**john**) based on computer logic.
The code also showing the string being multiplied three times and lastly being combined with the input by calling it's variable.

- We move to number two exercise with number. Take a look of the task given below.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/number-2-datatypes-numbers-exercise.png){: width="650"}
_Number 2 Exercise With Numbers_
On that, we being asked to do average of the sequences of some numbers. So I got curious and I thought "I want try to use loop to perform that operation", let me show you how my curiosity when on shown below:
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/false-averaging-script.png){: width="650"}
_Wrong Loop For Averaging_
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/false-averaging-script-output.png){: width="650"}
_Output of Wrong Loop For Averaging_
As you can see from my stupidly written code, the line ``number /= manual_sum`` doesn't affecting anything and just vanished into the void doing nothing. And so I asked ChatGPT about it and it giving me a hint to use ``len(sequence_x)`` so that I could fraction the sum by the numbers of the elements inside it. And so I follow along the suggestion and fixing it like this below.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/still-false-averaging-script.png){: width="650"}
_Another Failed Script_
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/still-false-averaging-script-output.png){: width="650"}
_Another Failed Output_
Welp, just me being silly again, this time when I asked ChatGPT again as why my code outputting like that, it suggest that I still dividing the ``manual_sum`` within each loop. It would go like:
First loop ``number = 2``
``manual_sum += 2 → manual_sum = 2``
``manual_sum /= 2 → manual_sum = 1.0``
Second loop ``number = 4``
``manual_sum += 4 → manual_sum = 5.0``
``manual_sum /= 2 → manual_sum = 2.5``
So from that point on ChatGPT Suggesting that I need to **first summing all of the numbers and only after that I can divide the full total of it**. At first I was confused by this, but I remember maybe ChatGPT meant that it need to be place outside of the loop. And so here's my attempt at processing the average after the loop.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/correct-averaging-script.png){: width="650"}
_Correct Script_
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/correct-averaging-output.png){: width="650"}
_Correct Output_
And tadaa~~ that what ChatGPT meant by processed outside the loop. So in Python, be mindful of the positioning because it really is that important. Python really sensitive with that kind of stuff. ChatGPT also criticize me of why using loop for that operation, saying that it's an overkill to use it, I could just go use something like.
```python
sequence_1 = (2, 4)
average = sum(sequence_1) / len(sequence_1)
print(average)
```
And yeah, it's correct that I'm overkill but I just curious and I also got to learn loop along the way too. Unto next learning that do.
- Unto the next sequence and that is ``(12, 14/6, 15)``, and after learned how to use loop, I thought to use it again.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/wrong-variable-being-called.png){: width="650"}
_Calling Wrong Variable_
But, would you look at that, I got wrong output again all because one simple mistake.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/output-for-calling-wrong-variable.png){: width="650"}
_Output for Calling Wrong Variable_
All of that because I wrongly calling variable ``number`` in the second sequence loop and remember that I still got previous script on the same file, so it calling wrong variable making it false. So from this I learned that I need to be careful about what variable being called on the progress and **don't forget naming your variable with some meaningful and describing name** to prevent this thing happening. So, just change variable ``number`` to ``integer`` and the result would be.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/output-for-calling-right-variable.png){: width="650"}
_Output for Calling Right Variable_
And the correct output is ``9.777777777777779``. Unto my next learning.

- Next is task number 3, now we're being tasked to find the volume of a sphere shown below.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/number-3-datatypes-numbers-exercise.png){: width="650"}
_Task Number 3, Finding Sphere Volume_
I kinda lost in thought about how to solve this question but I try my best and start with what I called **rough sketch** of what the script would look like and it goes like this:
```python
pi = 3.1415
radius = 5
## 4/3 *= pi
## pi ** 3
## print (pi)
```
I said to ChatGPT that I also wanted to loop for that task too but it kinda against my idea. So I asked why is that and it says that there's more cleaner way to do it like just straight up:
```python
pi = 3.1415
radius = 5
volume = (4/3) * pi * (radius ** 3)
print(volume)
```
Because that is more clear to read, fast to execute, and minimalist but also expressive. Which is look like my rough sketch but me being still curious, my ChatGPT giving me an ideal scenario for me to use loop for this kind of operation, so it goes like this.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/sphere-volume-with-list.png){: width="650"}
_Searching Sphere Volume With List_  
ChatGPT suggesting me to change the task to using list, instead of just one integer value, and so, I accepted it and get to working on it, and it would look like this.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/sphere-loop-output.png){: width="650"}
_Output Loop For Searching Volume of Radius List_  
But, what is this? The output is just showing the last value from the list. And with that I remember that position is really matters again. So the correct code would be this.

```python
pi = 3.1415
radius = [1, 3, 5, 7]

for radii in radius:
    volume = (4/3) * pi * (radii ** 3)
    print(f"Radius: {radii}, Volume: {volume}")
```
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/correct-output-of-sphere-list.png){: width="650"}
_Correct Output Loop For Searching Volume of Radius List_  
So, I asked ChatGPT again, what is the logic for the positioning in Python, why is it so sensitive about it. ChatGPT response was Python work by reading code line by line(left to right), top to bottom, but because previously my ``print(volume)`` code is outside the loop, that is the same way of telling Python to print the output once and that happen after the loop. That is why it just processing and hold the last calculation within the list.

- Next exercise is using modulo operator `%` and to check whether the number is even or odd.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/even-or-odd-task.png){: width="650"}
_Odd or Even Task_
And my first attempt to solve that task is like this.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/first-attempt-even-or-odd-script.png){: width="650"}
_First Attempt Even or Odd Code_
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/first-output-even-or-odd-script.png){: width="650"}
_Output of First Attempt Even or Odd Code_    
Why the error again? Yep, me being stoopid and not **positioning the ``print`` part INSIDE the ``if`` condition**. So let's try again. And you see that _odd_ looking ``odds %= odd``? Yeah, that is me being a dumbo again, doing some gibberish and ended up using uninitialized variable and ChatGPT suggesting don't use ``%=`` operator, unless you wanted to use the value repeatedly. So, let's see more of my another attempt again and let's just skimmed through it, because it just me guessing the correct way.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/second-attempt-even-or-odd-script.png){: width="650"}
_Second Attempt Even or Odd Code_ 
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/third-attempt-even-or-odd-script.png){: width="650"}
_Third Attempt Even or Odd Code_
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/fourth-attempt-even-or-odd-script.png){: width="650"}
_Fourth Attempt Even or Odd Code_  
And that is how I find the correct code to find even and odd number inside the sequence. I admit, I play around a bit and there's also me being a dumbo but hope you guys learn something too from this. To the next task.

- Our next task is a **True/False** condition, here's the exercise.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/true-false-condition-exercise.png){: width="650"}
_True/False Condition Exercise_  
For this exercise, it is quite straight forward, we have been assigned to find **True** or **False** of that condition.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/true-false-condition-output.png){: width="650"}
_Output True/False Condition_  
That is how I do the task, for me it is quite self explaining, the script is processing **AND**, a logic gate operation of computer. There's various logic gate on computer that you could [here](https://en-m-wikipedia-org.translate.goog/wiki/Logic_gate?_x_tr_sl=en&_x_tr_tl=id&_x_tr_hl=id&_x_tr_pto=tc). In short, the code being run here is a **AND** logic gate, where the value needed to be matched each other for the condition to be **TRUE**.
 
- We move to another subject of datatypes and that is strings, here's the task given for us to play around with it.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/datatype-string-exercises.png){: width="650"}
_Strings Datatype Exercises_
I'll begin with number 1, as you can see the instruction says to initialize strings with name `s` and calculate it's length, also to print its value multiple times. So, here's my answer to the task.
First, find the length, it's pretty simple and straight forward.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/counting-string-length.png){: width="650"}
_Counting String Length Using len Function_
As you can see, I just use `len()` function to let the do it for me. And you can also see that I use `str()` function surrounding it and that is to convert the `len()` function into string so that I could concatenate it with other string. And for the result variable, I left it there for the this next task. Let's get into it.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/multiplying-string.png){: width="650"}
_Multiplying String Output_
And I change result variable to multiplying and concatenating the string index. I take ChatGPT suggestion into consideration this time for not using loop because it would be an overkill and the result still the same.  

- To the second task and with different string value of `aaabbbccc`, the task is to find first occurrence of string `b` and `ccc`. The second task is to replacing `a` to `X`. Let's get into the learning of it.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/finding-and-replacing-string-attempt-one.png){: width="650"}
_First Attempt Finding and Replacing String_
This is my first attempt to answer the task, from the picture you could see that I use function `find()` and `replace()`. At first I didn't know such function exist and when I search on internet and find that [W3Schools](https://www.w3schools.com/python/ref_string_replace.asp) suggest me this and also ChatGPT told me that it also has for finding value. I just try using it and here I'm. Now, when I back to ChatGPT for checking is my code already correct based on the given task. It says that I would not need to concatenate the result of both replace. Because it would overlap with the other list result of it. So, here's my fix to it.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/finding-and-replacing-string-attempt-two.png){: width="650"}
_Second Attempt Finding and Replacing String_
And that is my fix, from the output we know that replace function is making new version of output version with each their usage.

- Last question, with `aaa bbb ccc` as the value of the variable. We've been asked replace the given value of it and here's some discovery and learning that I got.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/replacing-string-value.png){: width="650"}
_Replacing String_
The discovery that I know by doing this is that in Python you could use multiple function at once and as you can see from the picture, I used three function at once and it's just work fine. But there's also discovery that I made that made us possible using replace function.
![Desktop View](assets/img/posts/2025-06-23-python-learning-part-1-datatypes/other-method-using-replace-function.png){: width="650"}
_Other Method Using Replace Function_
With this method, we initializing the first process, save it to other process and reusing it when needed. The nature of Python that saves the value of the variable made this possible. And as far as my current shallow knowledge of Python, this made the code more readable.

## Thank You Words

Thank you all for your time reading this little learning journal that I made. My planning is to made this frequently as I progressively learn Python until I finish all of the material in the book. Thank you again.

## References

| References                                                                                                                                            |
| :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| fmottamendes Repository. [Hacking-related-books](https://github.com/fmottamendes/Hacking-related-books/tree/master)                                   |
| João Ventura Python Book. [Full Speed Python](https://joaoventura.net/books/)                                                                         |
| GeeksforGeeks Python. [Python Tutorial Learn Python Programming Language](https://www.geeksforgeeks.org/python/python-programming-language-tutorial/) |
| W3Schools Python. [Python Tutorial](https://www.w3schools.com/python/)                                                                                |
| Logic Gate Definition. [Logic gate](https://en-m-wikipedia-org.translate.goog/wiki/Logic_gate?_x_tr_sl=en&_x_tr_tl=id&_x_tr_hl=id&_x_tr_pto=tc)       |  |
