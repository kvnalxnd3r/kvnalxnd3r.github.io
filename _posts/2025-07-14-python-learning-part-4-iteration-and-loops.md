---
title: Python Learning, Part 4. Iteration & Loops
date: 2025-07-14 10:09 +0700
categories: [Programming, Python, Learning]
tags: [programming, python, learning, lists, learning journal]
---

## Chapter 6: Iteration and Loops

Back again with another Python learning journal, with learning topic of iteration & looping in Python language. This chapter is the important fundamental of every programming language including Python. Because with with it, we are able to tell the computer to iterate each values, enabling us to automate repetitive tasks.

For Python, the most common looping iteration would be either **`for`** and **`while`**. But before we dive more into the exercises, what is exactly those two means? Are they different? When to know their best usage? Here's the explanation.

- **`for`** Loop
This form of looping used when you want to loop over something with a defined structure to iterate over all of items of a list, range, string, etc.

- **`while`** Loop
The looping mechanic that you used when you don't know how many times the loop would be i.e. not having any structured but you just know that the loop needed to continue until the condition reach `false` at the of the loop. But until then, the loop continues if still return `true`.

- **`for`** vs **`while`**?
1. You Use `for` loop when you know how many steps or items you want to go through.
2. `while` loop is used when you don’t know in advance how many times the loop will run.
3. The loop continues as long as the condition is `true` with `while` loop. If the condition never turns `false`, it loops forever.
4. `for` loop is clean, predictable, and fits well with lists, ranges, files, etc.

Alrighty then, the theory part was done explained, now for the challenging stuff, the exercises. Starter exercises on this chapter is exercises using for loop. Let's jump right into it.

### Disclaimer
On this part of my journalizing my Python learning journey, you will notice the change of naming variable format from
```python
name_of_variable → nameOfVariable
```
This changes is for me to see what would the code be look like if I use my style back when I was still coding Kotlin([camelCase style](https://en.wikipedia.org/wiki/Camel_case)), and I also know Python had their own format called **snake_case** where you could read more about it [here](https://peps.python.org/pep-0008/). So......... I kinda sorry for making some eyes hurt when reading this and also at the same time not sorry because I just want to experiment and as per my knowledge this is my attempt to document my leanring, especially the error, the wrong, and the right of my learning. So bear with me for this chapter lol.

### Exercises with the for loop
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/exercises-of-for-loop.png){: width="750"}
_For Loop Exercises_
For loop having 7 tasks really surprises me here. It really emphasizes the important of this loop in Python language, but with this we could discover and learn so many things of this particular looping method. So let's start with number 1 and that is loop that sums all elements inside the list. 

Below is my first attempt solving number 1.
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/sums-list-value-task-first-attempt.png){: width="700"}
_First Attempt Number 1 For Loop Exercises_

As you can see from that wacky logic of mine there's some silliest stuff that I tried to do, such as:
1. I tried to `userList.append()` what is this mean after I search on internet and ChatGPT is that, I just only adding one input and making the list only has one number.
2. I missed to put function parameter there for accepting various input of values.
3. The `return print(sums)` is simply the goofiest of them, what the hell am I thinking back then. Do I need to return it or just print it immediately.....

And with that in mind ChatGPT also give me some advice such as:
- Investigate: input().split() and explore how you can use `.split()` and `int()` inside a loop or comprehension to convert space-separated input into integers.
- Explore: for number in my_list:
- Think about the difference between print() and return

And those advices makes something clicked in my mind. I just remember I've done how to input values into comprehension list, so why not use that? And also parameter? Yeah, I remember `*args` that accepting multiple values and I also remember `split()` used to separate spaces right.... Welp looks like it's coming together... and this is how attempt my next code.
```python
def addFunction(*integer):
    sums=0
    for x in integer:
        sums += x
    print(sums)

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [float(x) for x in userInputList.split()]

addFunction(numberList)
```
Just as I thought I finally got it the output once again surprises me. Here's the output below.
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/number-one-sums-function-first-attempt.png){: width="700"}
_Second Attempt Output Number 1 For Loop Exercises_

Well I guess it's not easy huh, what else do I expect ppfftt. Okay, so what's wrong here? Why it's invalid? What exactly **invalid literal for int() with base 10:** mean? So I searched it on internet and I stumbled upon this [stackoverflow post](https://stackoverflow.com/questions/1841565/valueerror-invalid-literal-for-int-with-base-10).

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/stackoverflow-number-one-error-clue.png){: width="700"}
_Clue For Number 1 For Loop Exercises_ 
Huh, interesting so it's rather a format differences and the value could not be parsed. I started to think.... **If it's the value differences.... Maybe I got it wrong on initializing it on the parameter of the function? How about I just change it without `*` symbol? Maybe it could be working...."**. So with that, I made a change just to erased the `*` on the function parameter.
```python
def addFunction(integer):
    sums=0
    for x in integer:
        sums += x
    print(sums)

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [float(x) for x in userInputList.split()]

addFunction(numberList)
```
And the moment of truth.... Let's see how's the output would look like.
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/number-one-sums-function-second-attempt.png){: width="700"}
_Second Attempt Number 1 For Loop Exercises_ 
And exactly as my guess previously... what a nice feeling when your guess turns out to be true in programming. It's just so addicting lol, but ngl the stress is really overwhelming too but hey, at least it's correct in the end. 

Oh and also why this one is correct because I give the function what it exactly expecting, a single list of numbers, but why it expecting single list now than before? Well it's because this symbol `*` or the name is **wildcard**, it's basically telling computer to expect any values but with a catch of multiple of it, so because I'm working with list, the computer expecting me to input multiple of list with that wildcard. Where if I don't use that wildcard, it just expecting one list.

Okay.... now that task number one is done, unto number 2 then. Here's how I first approaching it with my rough sketch of code. This is me trying to figure it out, so there will be a lot of attempt of me failing to realize the correct code, so bear with me lol.

```python
def biggerThanFunction(integer):
    index=0
    for x in integer:
        indexPointer= index + x
        if  x > indexPointer:
            print(f"The biggest number on the list is: {x}")
        return x
userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [float(x) for x in userInputList.split()]

biggerThanFunction(numberList)
```
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/maximum-value-loop-first-attempt-output.png){: width="700"}
_First Attempt Maximum Value Output_

And as you can see, my first attempt doesn't even printing the output lol, so, I asked again ChatGPT as to why this is happening. So, here's what ChatGPT hinting to me to fix my code.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/maximum-value-loop-hints.png){: width="700"}
_Hints For Fixing Maximum Value Loop_

Whew... that's a lot of hints, but mainly the reason why first attempt looked like that(it's an abomination indeed) because:
1. I think it's necessary to plus the index with the x in order to make it saved the value and comparing it for later usage.
2. I thought the use of `return` can only be done inside the loop.
3. I thought the value would just be saved automatically even without me declaring variable.
4. And I also thought that printing the result can only be be done inside the loop too.

That my initials thought when coding it but turns out ChatGPT clearing my misunderstanding of it and now I have somewhat rough understanding of it because of the given hints. So, with much more clear understanding, then begin my journey to tweak my code here and there lol, ladies and gentleman, I present to you, my dumbass just finding the correct output.

```python
#Attempt 1
def biggerThanFunction(integer):
    index=0
    for x in integer:
        if x >= index:
            print(f"The biggest number on the list is: {x}")

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [float(x) for x in userInputList.split()]

biggerThanFunction(numberList)
→
Input multiple numbers separated by spaces: 1 2 3 4 5
The biggest number on the list is: 1.0
The biggest number on the list is: 2.0
The biggest number on the list is: 3.0
The biggest number on the list is: 4.0
The biggest number on the list is: 5.0
## This code is just straight up a wacky crap, where is always true 
## because the comparison is zero(from index = 0) and that is why every number 
## in the list is always bigger and making the function always printing.

#Attempt 2
def biggerThanFunction(integer):
    index=0
    for x in integer:
        if x > index:
            index = x
            print(f"The biggest number on the list is: {x}")
        return x

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [float(x) for x in userInputList.split()]

biggerThanFunction(numberList)
→
Input multiple numbers separated by spaces: 1 2 3 4 5
The biggest number on the list is: 1.0
## Yep, now I kinda get it now, the return should be outside... 
## But now the logic still wrong because the comparison still the same with 0 but 
## it stops at the first value. 

#Attempt 3
def biggerThanFunction(integer):
    index=integer[0]
    for x in integer:
        if x > index:
            index = x
            print(f"The biggest number on the list is: {x}")
    return x

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

biggerThanFunction(numberList)
→
Input multiple numbers separated by spaces: 1 2 3 4 5
The biggest number on the list is: 2
The biggest number on the list is: 3
The biggest number on the list is: 4
The biggest number on the list is: 5
## Okay this is getting interesting, the output is correctly comparing each values inside the list 
## but the catch here is that the list keep going on and on printing the output 
## where it should've been just output one value.

#Attempt 4
def biggerThanFunction(integer):
    index=integer[0]
    while integer > index:
        print(f"The biggest number in the list is {integer}")

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

biggerThanFunction(numberList)
→
line 79, in biggerThanFunction
    while integer > index:
          ^^^^^^^^^^^^^^^
TypeError: '>' not supported between instances of 'list' and 'int'line 79, in biggerThanFunction
    while integer > index:
          ^^^^^^^^^^^^^^^
TypeError: '>' not supported between instances of 'list' and 'int'
##Yeah, this one is just straight up wrong. This code is just me experimenting with while loop. 
##The next attempt will be back using for loop.

#Attempt 5
def biggerThanFunction(integer):
    index=integer[0]
    for x in integer:
        if x > index:
            index = x
    print(f"The biggest number on the list is: {index}")
    return index

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

biggerThanFunction(numberList)
→
Input multiple numbers separated by spaces: 12 3 45 67 19 20 100
The biggest number on the list is: 100
## Finally, the correct attempt, this time I testing it with various bigger number to see if the logic got it wrong somewhere but turns out it working correctly. 
## Now, from my previous experience with all of the attempt previously, I came to conclusion that 
## the positioning is really matter, so in this attempt I move out the print and return to outside of the loop and now the code working correctly. 
```
Whew... that is a lot for number two. I'm kinda sorry for you to have witness the bullshit code that I attempted there but I feel like to documenting a lot of my progress here so that I could reflect upon it in the future. Number two done, now let's solve number 3 task.

For number three, here's how I first approaching the task with this code.
```python
def biggerThanPointerFunction(integer):
    maxValue=integer[0]
    index = enumerate(integer)
    for x in integer:
        if x > maxValue:
            maxValue = x
    print(f"The biggest number on the list is: {maxValue}, {index}")
    return maxValue

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

biggerThanPointerFunction(numberList)
```
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/maximum-number-with-index-position-output.png){: width="700"}
_First Attempt Output of Printing Max Value In The List Along It's Index Position_

Okay.... the logic is just me copying it from the previous task but when I tried to combined printing out index straight in the string whereas index here has been declared as enumerated integer, the position just being showed in hex format and unreadable for humans. 

So, let's ask ChatGPT again for some clues that might leads us to the right answer, and here it is below the right answer of it.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-maximum-number-with-index-clues.png){: width="700"}
_Hints For Fixing Maximum Value With Index_

So, from the clues, now I know that it is possible to declare two variables in loop. Because previously until now, I just know that you could just use one variable in the loop but now I know that you could use more than one, I will put new variable that could saved and tracked index. I also know from the clues that you could just use `enumerate()` wrapping the argument with it in the loop.

The last clue pointing that I still not save and track index. This because as I said, I don't know you could just use more than one variable of for loop in Python. Okay, now that the misunderstanding and mistake has been corrected and given hints, I tried my best to make the code. And here's my result of it.

```python
def biggerThanPointerFunction(integer):
    maxValue=integer[0]
    maxIndex = 0
    for x, value in enumerate(integer):
        if value > maxValue:
            maxValue = value
            maxIndex = x
    print(f"The biggest number on the list is: {maxValue}, at index: {maxIndex}")
    return maxValue

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

biggerThanPointerFunction(numberList)
```
Now, the moment of truth, here's the output.
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/maximum-number-with-index-position-second-attempt-output.png){: width="700"}
_Second Attempt Output of Maximum Value With Index_

And would you look at that, I got it correct on my second attempts. So the logic here is that we need to point direction correctly, don't forget to save the value on a variable and lastly, the enumerate is needed because it returns pairs of `(index, value)` that helped the loop unpack the pairs directly in the loop. And welp.... looks like I'm done for task too, now let's go tfor task number 4.

For number 4, here's how my first attempt look like. I first try to understand what the task wanted me to do because I'm still confused with the words "keep adding the value in reversed order", so I asked ChatGPT again for some clearance and here's what it says.
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/asking-clearance-for-number-4-task.png){: width="700"}
_Hints For Solving Task Number 4_

```python
def reversingListFunction(input):
    newList = []
    for x in reversed(input):
        newList.append(x)
    print("The revered list is: ", newList)
    return newList

inputOriginalList = input("Input your list to be reversed: ")
originalNumberList = [int(x) for x in inputOriginalList.split()]

reversingListFunction(inputOriginalList)
```
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/reverse-for-loop-function-first-attempt-output.png){: width="700"}
_First Attempt Output Reverse List Number 4_

As you can see, the function is working properly but there's some bug there, when I input the list with spaces, the spaces also got counted as part of the list and because of that, it also being reversed alongside the values. So.... I asked ChatGPT again and it pointed out that this line below:

```python
reversingListFunction(inputOriginalList)
```
That one line the cause of the bug because I am passing raw string from it where instead I should've been passing ``originalNumberList`` variable that I just made with ``.split()`` function. and here is my code after I fixing it.

```python
def reversingListFunction(reversedList):
    newList = []
    for x in reversed(reversedList):
        newList.append(x)
    print(f"The reversed list is: {newList}")
    return newList

inputOriginalList = input("Input your list to be reversed: ")
originalNumberList = [int(x) for x in inputOriginalList.split()]

print("Your original list:", originalNumberList)
reversingListFunction(originalNumberList)
```
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/reverse-for-loop-function-second-attempt-output.png){: width="700"}
_Second Attempt Output Reverse List Number 4_

And as you can see, the list working perfectly now even we input it with or without spaces. Well, now that number 4 was done, we move to number 5.

On this task we've been asked to sort the list and if the list is in-order for example [1,2,2,3] it will return **true** because it is in order and if it is [1,3,2,4] it's **false**. And seeing that this kind of suited to use **while loop** in which I know, yes this is a for loop exercises section but because the condition itself is a **true** or **false** condition, that is my main reason choosing while loop.

So here comes my little experiment with many attempt to solve it using while loop on this exercise.

```python
def is_sorted(sortedNumber):
    x = 0
    while x > len(sortedNumber) - 1:
        if sortedNumber[x] < sortedNumber[x+1]:
            return False
        x = x + 1
    return True
        

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

print(is_sorted(numberList))    
```
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/sorted-function-first-attempt-output.png){: width="700"}
_First Attempt Output Sorted Function Number 5_

Welp, my function do the funny and now it is returning true in a supposedly false condition...... Welp I have already applied the fibonacci thingies from my previous [part 3](/posts/python-learning-part-3-modules-and-functions/) learning journal post with that `..... - 1:` and `< sortedNumber[x+1]:` part and looks like it's kind working????

But I could not help but to notice that, maybe this function of mine is stop comparing on the first value in the list because it literally bigger than value zero..... But to make sure, I'll try checking it to ChatGPT if I have any missed and yep... turns out my suspicion was correct... The caused is my comparison that failing me.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-soted-function-hints.png){: width="700"}
_ChatGPT Hints To Fix Number 5 Function_

And with that hints, I can fix my code to be like this.
```python
def is_sorted(sortedNumber):
    x = 0
    while x < len(sortedNumber) - 1:
        if sortedNumber[x] > sortedNumber[x+1]:
            return False
        x = x + 1
    return True
        

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

print(is_sorted(numberList))
```

And here is false condition output from the code above.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/sorted-function-second-attempt-output-false-condition.png){: width="700"}
_Second Attempt False Condition Output Sorted Function Number 5_

And here the true condition output.
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/sorted-function-second-attempt-output-true-condition.png){: width="700"}
_Second Attempt True Condition Output Sorted Function Number 5_


And just like that, a simple fix on comparison could change the output. Don't forget to always remember to check the logic of the comparisons guys. So with task number 5 done, now we got to number 6.

For this task again I need ChatGPT help to made me understand it because at the time I am doing this exercise, I was done night shift and only had 3 hours sleep lol, so my mind scatter all over place.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-descended-sorted-list-hints.png){: width="700"}
_Hints For Solving Task Number 6_

So in short, the task asking me to check whether the list is descending orderly. Welp, looks like the clues in the task that said it is similar to previous task is correct. So what I am gonna do now is to firstly check this first attempt code that I just changed the logic here and there.

```python
def is_sorted_dec(sortedNumberDec):
    x = 0
    while x > len(sortedNumberDec) - 1:
        if sortedNumberDec[x] < sortedNumberDec[x+1]:
            return False
        x = x + 1
    return True
        

userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

print(is_sorted_dec(numberList))
```
As for the output, here is what it says.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/sorted-dec-function-first-attempt-output.png){: width="700"}
_First Try Attempt Output Sorted Dec Function Number 6_

And when I asking ChatGPT, what is wrong with that code of mine, turns out my suspicion about the comparisons being off was true. Especially this line said ChatGPT:

```python
while x > len(sortedNumberDec) - 1:
```

I thought at first that all of comparisons that should be changed but turns out the only one that needed to are the `if` condition but that comparison. Why? Because the `x= 0` and this made the condition always true, making the comparison returning true too early without comparing other value. 

So what am I gonna do next is to change that line and keep the `if` condition comparison and see the result.

```python
def is_sorted_dec(sortedNumberDec):
    x = 0
    while x < len(sortedNumberDec) - 1:
        if sortedNumberDec[x] < sortedNumberDec[x+1]:
            return False
        x = x + 1
    return True
        

userInputDecList = input("Input multiple numbers separated by spaces: ")
numberDecList = [int(x) for x in userInputDecList.split()]

print(is_sorted_dec(numberDecList))
```

Here's the moment of truth, let's test it with both condition, starting from `false` condition.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/sorted-dec-function-second-attempt-false-condition-output.png){: width="700"}
_Second Attempt Output Dec Sorted Function False Condition Number 6_

Looks like it's working, now let's test the `true` condition.
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/sorted-dec-function-second-attempt-true-condition-output.png){: width="700"}
_Second Attempt Output Dec Sorted Function True Condition Number 6_

Okay.... both condition are fulfilled, now we are going next to task number 7. Let's solve it.

For task number 7, I needed to confirm whether in Python able to use loop inside loop. So, here's what I got as hints from ChatGPT.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-duplicate-checker-function-hints(1).png){: width="700"}

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-duplicate-checker-function-hints(2).png){: width="700"}
_Hints From ChatGPT For Task Number 7_

So from that clue in my understanding we need to check all of the values in range of it and within the length of it too. After that for every value in range we add 1 to it and it need to be in length  of it. And after it looped through all of that we then compare previous loop with the second loop and after that, we return true. Sounds about right...... let`s what kinda slop code that I will made now.

```python
def has_duplicates(duplicatesNumList):
    x = 0
    for a in range(len(duplicatesNumList)):
        for b in range(x + 1, len(duplicatesNumList)):
            if duplicatesNumList[a] == duplicatesNumList[b]:
                return True
            else:
                return False
        
userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(x) for x in userInputList.split()]

print(has_duplicates(numberList))
```

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/duplicate-checker-function-first-attempt.png){: width="700"}
_First Attempt Duplicate Checker Task Number 7_

Hahahahahahaha lol, even with all of that hints from ChatGPT I still could not solve this... Oh well, skill issue on my side. So let's ask ChatGPT what went wrong here.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-duplicate-checker-function-first-attempt-hints.png){: width="700"}
_Hints From ChatGPT For First Attempt Task Number 7_

Ok... so what I am wrong on the first attempt is that the `false` returning it's value too early because if it just find one pair that is not equal this value will show up. And I see that it mention the value being declared as zero........ and this also causing it to return `false` too early. But then as scroll down, I see this.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-goofing-up.png){: width="700"}
_Really ChatGPT....... _

Oh look at that, ChatGPT already answering it even though I already made it clear to not include answer and just a hint. Oh well... it is what it is I guess. Let's follow along with the code, consider it as a bonus I guess. Here's my code after following that.

```python
def has_duplicates(duplicatesNumList):
    # x = 0
    for a in range(len(duplicatesNumList)):
        for b in range(a + 1, len(duplicatesNumList)):
            if duplicatesNumList[a] == duplicatesNumList[b]:
                return True
            
    return False
        
userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [int(a) for a in userInputList.split()]

print(has_duplicates(numberList))
```
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/duplicate-checker-function-second-attempt.png){: width="700"}
_Second Attempt Duplicate Checker Task Number 7_

Now it's working properly.  And yeah, my suspicions was true, it's the declaring variable `x= 0` and using it with `x + 1` was the cause. I should instead just use the previous looping variable and then add one to it and then comparing it. After that, return `true` but make it return `false` on outside of the loop. And it fixed it.

Whew....... `for` loop exercises section done, now there's only one left, the `while` loop. Let's move on to that section.

### Exercises with the while statement

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/while-loop-exercise-section.png){: width="700"}
_While Loop Section Exercise_

And that is all for `while` section exercise, just one task and that task is asking for defining even and odd number but the twist is that we need to identify it by making it clear in the output it says "Even number: 10....". So we've done checking even and odd numbers previously and this time we've been asked to combined it with `while` loop. Let's dive in and see my first attempt solving this.

```python
def evenOddFunction(x):
    while x >= 0:
        if x % 2 == 0:
            print(f"Even number: {x}")
            x -= 1
        else:
           print(f"Odd number: {x}")
           x -= 1
           
userInputEvenOddList = float(input("Input number: "))

print(evenOddFunction(userInputEvenOddList))
```
![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/while-loop-even-odd-checker-first-attempt.png){: width="700"}
_First Attempt Even Odd Numbers Checker Number 1_

This first attempt really interesting to me, because the logic appear to work, but the only problem is that it's still printing `none` at the end of the operation. In this code, my logic is keep subtract the number because apparently the subtraction only work inside the `if else`, but this turns out wrong on the explanation from the next ChatGPT hint prompt. But keep in mind this is me being exhausted but still keep forcing to learn lol. 

At this time, I've just back home from 12 hours night shift and let me tell you self doubting and what they **call impostor** is really strong here. It makes me really stupid and stressed because this somewhat easy task seems bigger than it seems. Maybe because the tiredness from the long shift? I don't know, but with it, I just ask ChatGPT again to help because at this point I can't really think straight lol.

So here's the hints given to me by ChatGPT. Despite this hints, I still failed to understand it and making a slop code again lol. But here ya go,

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-even-odd-while-loop-checker(1).png){: width="700"}

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/chatgpt-even-odd-while-loop-checker(2).png){: width="700"}
_Hints Explanation by ChatGPT For Even Odd Checker While Loop_

Even though all of the hints is clear from the start, I still confused and just understand the `x -= 1` needed to be printed outside but how I interpreted **outside** here is that like this.

```python
def evenOddFunction(x):
    while x >= 0:
        x -= 1
        if x % 2 == 0:
            print(f"Even number: {x}")
        else:
           print(f"Odd number: {x}")

userInputEvenOddList = float(input("Input number: "))

evenOddFunction(userInputEvenOddList)
```

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/while-loop-even-odd-checker-second-attempt-output.png){: width="700"}
_Second Attempt Even Odd While Loop Checker_

Okay, now I correctly moving the subtract outside but the loop keep subtracting even after reaching `0.0` making it `-1.0`. And that is just wack output lol. And then being me confused and scattered mind, here's my another attempt lol.

```python
def evenOddFunction(x):
    while x >= 0:
        if x % 2 == 0:
            return f"Even number: {x}"
        else:
           return f"Odd number: {x}"
        x -= 1

userInputEvenOddList = float(input("Input number: "))

print(evenOddFunction(userInputEvenOddList))
```

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/while-loop-even-odd-checker-third-attempt-output.png){: width="700"}
_Third Attempt Even Odd While Loop Checker_

I just keep changing back and forth here until I stuck and straight up just asking ChatGPT for the right answer and here's what it needed to be right code exactly, seeing this and reflecting upon it, I realized I need to rest fully until my mind clear to code instead just keep forcing my limit lol.

But before I'm giving up, I asked one ChatGPT one last question about what is exactly made the `None` shows up at the of my original code, and here's what the ChatGPT giving its explanation.

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/explanation-for-print-and-return-in-function(1).png){: width="700"}

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/explanation-for-print-and-return-in-function(2).png){: width="700"}
_Explanation For Return and Print Usage In A Function_

Okay.... Now I kinda get it, you use `return` when you later wanted to use it for later by calling it with `print`. And as for the `print`, yes you could see it just like printing the `return` but it's just that and the value is gone, poof! Vanished into the void just like that. And at this time around it clicked on my hazy mind(despite being already explain on the previous hints lol).

So with that, I got it clicked in my head and start changing my previous code. Because the answer just directly in front of my face. Here's it is, my code first attempt but with understanding of `print` vs `return`.

```python
def evenOddFunction(x):
    while x >= 0:
        if x % 2 == 0:
            print(f"Even number: {x}")
            x -= 1
        else:
           print(f"Odd number: {x}")
           x -= 1
           
userInputEvenOddList = float(input("Input number: "))

evenOddFunction(userInputEvenOddList)
```

![Desktop View](assets/img/posts/2025-07-14-python-learning-part-4-iteration-and-loops/while-loop-even-odd-checker-fourth-attempt-output.png){: width="700"}
_Output After Understand How `return` and `print` Works_

And would you look at that, I got it correct now, despite being given such clear answer previously lol. But, ChatGPT has better suggestion than mine, it's basically just like my second attempt, to move the subtraction outside of the `if else` condition like this.

```python
def evenOddFunction(x):
    while x >= 0:
        if x % 2 == 0:
            print(f"Even number: {x}")
        else:
           print(f"Odd number: {x}")
        x -= 1
           
userInputEvenOddList = float(input("Input number: "))

evenOddFunction(userInputEvenOddList)
```
And the output still the same but in more efficient way to executing the logic. But, I asked ChatGPT that supposed I want to use `return` for that kind of task, because when I first tried the third attempts, I got the subtraction is not considered(simply just being grayed out). So, here is what ChatGPT suggesting the usage of `return` in this kind of task.

```python
def evenOddFunction(x):
    result = []  # an empty list to store results
    while x >= 0:
        if x % 2 == 0:
            result.append(f"Even number: {x}")
        else:
            result.append(f"Odd number: {x}")
        x -= 1
    return result  # now return the entire list

userInput = int(input("Input number: "))
messages = evenOddFunction(userInput)

for message in messages:
    print(message)
```
And I can see that you wanted to collect all of the values first in an empty list, after that doing the comparison, then appending it(save it) into that empty list and then saving it using `return result`(unlike my third attempt where `return` that being declared follow by the operation, making the operation grayed out/not considered because `return` is where function is stop its process and saving the value gained by that process). So in that ChatGPT code, after the value saved in `return`, we now could input the value, then using `for` loop to helps us printing each result automatically using loop.

And that is all of the learning material and exercises in this section, really such a long learning but with this I gained somewhat fundamental understanding of core subject and that being `return`, `function`, `print`, `loop` logic, and function basic understanding.

## Thank you words
Thank you again for Professor João Ventura for this wonderful learning resource, I will keep my learning journey in Python using this book because of the simple and direct way of learning and teaching. Thank you so much again Prof. and much respect from me for making this available for all people.

## References

| References                                                                                                                                                                            |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Camel Case Style Writing. [Camel Case](https://en.wikipedia.org/wiki/Camel_case)                                                                                                      |
| Standard Python Rule For Writing Style. [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)                                                                      |
| Stack overflow ValueError Case. [ValueError: invalid literal for int() with base 10: ''](https://stackoverflow.com/questions/1841565/valueerror-invalid-literal-for-int-with-base-10) |

