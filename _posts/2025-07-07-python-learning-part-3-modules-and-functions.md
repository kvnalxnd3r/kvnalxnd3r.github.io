---
title: Python Learning, Part 3. Modules and Functions
date: 2025-07-07 12:15 +0700
categories: [Programming, Python, Learning]
tags: [programming, python, learning, lists, learning journal]
---

## Chapter 5: Modules and Functions

This chapter of my Python learning journey is really interesting for me, because I always fascinated as to how computers being able to process complex mathematic problem and with Python it is possible when we `import math` modules that containing mathematical function, that way we could perform something like this:

```python
import math
print(math.cos(0.0)) 
print(math.radians(275))
```

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/cos-and-radians-math-operations.png){: width="700"}
_Example of Mathematical Operations Using Math Module_

After I trying out the math module, next I tried defining function as per instructed by the book, to see just got the feels of how to make one of it.

```python
def printTexts():
    print("This is")
    print("a text")

printTexts()
```

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/firstly-defining-a-function.png){: width="700"}
_Trying Out Defining Function First Time_

And after I tried writing function in Python, I arrived at this section in the book.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/return-in-function.png){: width="700"}
_Return Instruction Inside Python Function_

This thing called **return** is really bothering me since the first time I learned programming in highschool, so I tried asking ChatGPT to make an example out of it. An example that maybe even a child could understand, so here's how it is being explained.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/example-of-explaining-return-in-function.png){: width="700"}
_Example of Return Explanation_

From that example, I could make understanding that **return** is how we telling computer to **returning** the result/output back to us instead just lost into the void. So when I tried defining function with mathematical processing inside it but without **return** here's how the output.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/defining-function-without-return.png){: width="700"}
_Example of Function Without Return_

And yep, without `return` the code just made the operations but the result vanished, resulting in code printing **None** as the result because it hold none of the result from the value that being process.

Welp looks like I got the hang out of it or something like that, so with that in mind, let's just start straight to the exercises, for this chapter there's three parts of exercise and each with their own objective that will teaches us different ways to use math module and also combining it with function that we just learn. Here's the first part of it below.

### Exercises with the math module

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/first-exercise-of-math-module.png){: width="700"}
_Part One Exercises of Math Module Chapter_

From the first part, we will begin by solving first exercise, to find **the greatest common divisor** of the designated numbers. At first, I don't understand what is greatest common divisor and when I search on the internet what does it mean I got this result.
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/gcd-modules-in-python.png){: width="700"}
_Greatest Common Divisor/GCD_

Okay, so Python have their own template module for this kind of numerical operation, welp let's try this to the first task. We being task finding bunch of pairs, here's my code  

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/gcd-math-operation.png){: width="700"}
_Code of GCD_

I tried first approach by directly printing it along with it's mathematical operation but then an idea struck my head, "how about if i directly defining function for it showing multiple gcd operations?" and that is the story behind that function. Also ChatGPT giving me some advice as to making it more effective and cleaner, and with that suggestion I now know something new again about function and that is it could be made to accept value using parameter like this.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/making-gcd-function-more-effective-and-cleaner.png){: width="700"}
_Making GCD More Cleaner & Effective_

With that now, I now get the rough idea how to make function now. Function can be made without using parameter or with one and we could also make it accepting value/values with that parameter. Let's move on to second task and here's my first attempt to solve it.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/log2-task-first-attempt.png){: width="700"}
_First Attempt Log2 Code_

I tried my first attempt by straight made the module processing a whole list of value but turns out in Python can't do that and yep, it asked me to assigning it to real number and not a list. And when I see ChatGPT explanation about log2 operation I got this result.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/chatgpt-log2-logic.png){: width="700"}
_ChatGPT Log2 Logic_

So, with that kind of logic I came up with this as second attempt.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/log2-task-second-attempt.png){: width="700"}
_Second Attempt Log2 Code_

I just error because I tried to log2 value `0` and when I search and asked ChatGPT, I just know that value `0` returns error because it's like telling the code how many times do the given number be divided with 2 until it reach the given number(yep, I really bad when explaining math, oh well I'm stupid when it comes to math as well). To make illustration, here's how in practice would look like:

```shell
log₂(8) = 3 → Because 2 × 2 × 2 = 8
log₂(1) = 0 → Because 2⁰ = 1
```

And now imagine what number can be raise 2 that will give us `0`? Well, none right? That is why the machine is getting error. So to fix that, I just go with this code.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/log2-task-third-attempt.png){: width="700"}
_Third Attempt Log2 Code_

Yep, just simply making the log2 of `0` is a printable string and the rest is the real math operation. And as you can see, I use function too for processing multiple log2 operation but the best way to make this possible other than making the code processing the log2 each time manually, we could use loop but because for now I just follow the books canonically now, making the loop out of question for now but rest assured, loop will appear for the next chapter. For now, let's just use only function. With that, second task is complete, now we go to third task.

The third task is quite tricky, because it involve with user input and to that, I first making a rough sketch of the code so that I could feel working my way through the correct code. Here's how making the logic with the sketch code that I made.

```python
print("Please input your numbers: "{user_input = input()})

def calculus_function(x):
    cos(user_input)
    sin(user_input)
    tan(x)
print calculus_function()
```

As you can see I got it wrong on a several places and it's all over my code. The fix will explain in the next attempt that I do to make this code worked, but for now let's see why I failed and it can be listed as such:
1. String Formatting Mistake
The mistake is on the code line `print("Please input your numbers: "{user_input = input()})`. I still not quite familiar with how python worked at this time, so I just go with how I know taking an input from my badly memory from my Kotlin programming day. And yep, turns out I'm wrong.
2. Using sin(), cos(), and tan() Without Importing
```python
cos(user_input)
sin(user_input)
tan(x)
```
Those lines are so wrong because looking back at it, what the hell am I thinking, calling cos, sin, tan without import the math module? Yep, always remember to check all of the imported module before calling their function guys.
3. Using a Parameter (x) vs Input Variable
```python
def calculus_function(x):
cos(user_input)
sin(user_input)
tan(x)
```
This is one was also me tripping, why do I called ``user_input`` when I declare the parameter as `x`.
4. Calling the Function
``print calculus_function()`` line really shows how deep my Kotlin habit implanted and when I tried asking ChatGPT, it says that I'm using Python 2 style when in fact I should be using `print(calculus_function(....))`.

With that, I tried another attempt with this code.
```python
user_input = input("Please input your number: ")

user_number = float(user_input)

def calculus_function(x):
   math.cos(x)
   math.sin(x)
   math.tan(x)
print(calculus_function(user_number))

→ None
``` 
The output of those code is just `None` and with that attempt I discover another things to know about Python, when the function reaches the end of it, if you don't declaring it to returning anything, it will simply return nothing. That is why the result is just none. Another things I discover and let's move to another failed attempt from me lol.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/function-not-processing-anything.png){: width="700"}
_Function Not Processing Inputted Value_

You see here that my attempt this time is not processing the value being inputted just straight up printing it. When I asked ChatGPT, it says that I literally just told my code to **"Take value of `x` and do some math thingy(sin, cos, tan) but don't store it or return it's value."** ChatGPT giving me hint that I need to **store the results of the process and then return it on my function**, and with that clue, here's another slop code from me.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/function-working-properly-but-still-return-none.png){: width="700"}
_Function Working Properly But Still Return None_

Yet another slop code from me, this time I do some wacky stuff. Letting the function worked properly but then revert to returning none wkwkwkwkwk. This wacky stuff happen because I still doesn't instructing my function to returning anything but the `print` itself just printing anything that being instructed to it. It doesn't store anything into function, even though I've already declaring it inside a variable. So here's yet another attempt of mine.

```python
import math
user_input = input("Please input your number: ")

user_number = float(user_input)

def calculus_function(x):
   cos_process = math.cos(x)
   sin_process = math.sin(x)
   tan_process = math.tan(x)
   final_result = cos_process, sin_process, tan_process
   return final_result
print(calculus_function(user_number))

→
Please input your number: 90
(-0.4480736161291701, 0.8939966636005579, -1.995200412208242)
```
As you can see, I just delete the `print()` from the variable and now the function working properly, because now the value being saved to the variable properly. But I'm not done yet, I think just showing numbers that look like a maniac rambling wouldn't make sense to the reader, so I asked ChatGPT how could I make the output more recognizable, because at this point I don't know how to combining variable with strings output(kinda like labeling it). And ChatGPT answered me with this suggestion:

```python
final_result = f"cos: {cos_process}, sin: {sin_process}, tan: {tan_process}"
return final_result
```
And for me that is a new discovery, because I before that point still don't know how to do that kind of thing. So when I discovered it by ChatGPT, that is adding new knowledge to my head, thx ChatGPT. But oh yeah, there's also another suggestion such as:
```python
final_result = {
    "cos": cos_process,
    "sin": sin_process,
    "tan": tan_process
}
return final_result
```
inside the function and we still get the same result but what's nice about this suggestion is that we could accessing the each variable individually like this:
```python
result = calculus_function(user_number)
print(result["tan"])
```
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/accessing-calculus-function-individually.png){: width="700"}
_Accessing Variable Individually_

We use string as the pointer to pointing at the specific variable for then we could see the result of it. Quite fancy eh, really thank you so much ChatGPT for the nice insight. With that, we done with first exercise section, now we proceed continue to the second section exercise.

### Exercises with functions

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/exercises-with-function-section.png){: width="700"}
_Function Exercise Section_

Here comes another exercises, this time it is more challenging for me. Welp let's see how my attempt goes and by that I mean let's see another slop that I made.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/add2-function-first-attempt.png){: width="700"}
_Completing Task On First Try, Cihuy!🎉_
Looks like for this task I completing it in just one try and quite amusing when I got it clicked in the first try oh and for convenience, let's just straight complete `add3` function too, at this time in my understanding you just simply put another one input instruction. Here's the code:
```python
add2_input_a = float(input("Input your first number: "))
add2_input_b = float(input("Input your second number: "))
add2_input_c = float(input("Input your third number: "))

def add2_function(a, b, c):
    sums_function = f"The sum up of your two number is: {a + b + c}"
    return sums_function
print(add2_function(add2_input_a, add2_input_b, add2_input_c))
→
Input your first number: 1   
Input your second number: 2
Input your third number: 3
The sum up of your two number is: 6.0
```
But! I'm not done yet, I got curious about what other way I could do this more efficient or maybe there's some improvement can be done here, so I asked ChatGPT again for that, and here's some suggestion that it made:

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/chatgpt-suggestion-for-multiple-argument-function.png){: width="700"}
_ChatGPT Suggestion_

This suggestion really intrigues me, because it such a simple way of saying "grab whatever the fuck you want". But how's that possible? Well that is possible because of the wildcard(*) operator. It basically telling the computer to input whatever being inputted, be it an **integer** or **strings**. Soo I got an idea, how about I try using that and also some template function to simplify my code previously. Well here how it goes:
```python
add2_input_a = float(input("Input your first number: "))
add2_input_b = float(input("Input your second number: "))
add2_input_c = float(input("Input your third number: "))

def add2_function(*args):
    return f"The sum is: {sum(args)}"
print(add2_function(add2_input_a, add2_input_b, add2_input_c))
→
Input your first number: 1   
Input your second number: 2
Input your third number: 3
The sum up of your two number is: 6.0
```
Notice the difference on the function logic, I just change the parameters first so that the input can be as many as you wanted, I also changed the `a + b + c` to simply just using `sum(....)`. Just with a simple tweak like that could make the code goes a long way, I am getting more excited to learn next task, well... here's my attempt on the next task below.

Second task is comparison between two value that being input. At first I don't really understand what it really wants. So I asked ChatGPT for clearance.
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/asking-chatgpt-for-comparison-function-task-hint.png){: width="700"}
_ChatGPT Helping For Clearance_

So with that help from ChatGPT, I start make the code for my first attempt.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/comparison-function-first-attempt.png){: width="700"}
_Comparison Function First Attempt_

To my surprise, this first attempt working perfectly but ChatGPT made suggestion about the way that I call previous variable(the input). That is not wrong, but for more cleaner and reusable I could change the code to:
```python
comparingInput_a = float(input("Input your number to be compared: "))
comparingInput_b = float(input("Input your number to be compared: "))

def comparing_number_function(a, b):
    if a > b:
        return f"The number {comparingInput_a} is greater than {comparingInput_b}"
    else:
        return f"The number {comparingInput_b} is greater than {comparingInput_a}"

print(comparing_number_function(comparingInput_a, comparingInput_b))
```
That way my code is much more cleaner and the output still the same. Well looks like that's it for number one, unto number two.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/divisible-function-first-attempt.png){: width="700"}
_Divisible Function_
And yep, the function this time working properly as intended and returning the correct output. But when I asked ChatGPT about it, there's suggestion that making the code more cleaner. Here's the suggestion:
```python
def is_divisible(a, b):
    result = a % b == 0
    print(f"The number {a} is{' ' if result else ' not '}divisible by {b}")
    return result

# Test
a = int(input("Enter number a: "))
b = int(input("Enter number b: "))

print("Result:", is_divisible(a, b))
```
To my surprise, in Python you could do something like this and it really nice things to know. It made the code much more readable and simple. The conditional can be used in one line and with it you could just use return value one time, making it more efficient.

Last task for me is quite ambiguous for what it meant. Is it wanted me to pass a fixed input or me being able to input whatever value list into the code? So I asked ChatGPT for clearance for what it meant by that.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/asking-chatgpt-for-list-average-function.png){: width="700"}
_ChatGPT List Average Suggestion_

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/chatgpt-suggestion-for-multiple-argument-function.png){: width="700"}
_Fucking ChatGPT, I have not yet told it to solve it......._
As you can see from those two pictures, I have confirmation that it is indeed possible to pass input and made it into a list. But when I scroll down my ChatGPT, to my surprise it already answered my question, fucking AI. I'm not instructing it to solve it, but oh well here we are getting spoiled. But I still attempt to solve it my own way, and if ChatGPT suggesting using function `average()` I will do it manually.

```python
def is_divisible(a, b):
    if a % b == 0:
        return f"The number {a} is divisible with number {b}"
    else:
        return f"The number {a} is not divisible with number {b}"
    
userInputList = input("Input multiple numbers separated by spaces: ")
numberList = [float(x) for x in userInputList.split()]
averageList = sum(numberList) / len(numberList)
print(averageList)
→
Input multiple numbers separated by spaces: 2 3 4 5
3.5
```
Well that's it from me, it's still the same function as ChatGPT answered but what I learned from here is that, **comprehensions list can be combined with input to make a list.** Oh and also ChatGPT suggesting me to made the print like this.
```python
print(f"The average of the numbers you entered is: {averageList}")
```
Yep, that's it for exercise with functions in Python, there's so much new things that I learned from this exercise alone. But there's still one exercise section left for us to learn from this chapter of the book. So let's get into it.

### Exercises with recursive functions
The last section of exercise of this chapter and we will go a bit of mathematical logic for this function(well it's in the name duh...). Here's the task for this section.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/recursive-functions-exercises-section.png){: width="700"}
_Recursive Functions Exercises Section_
As you can see it is only just three task but for the first task it's quite easy, just a plain factorial operation and would you look at that, it's already answered by the book above, take a look.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/recursive-function-factorial-operation-example.png){: width="700"}
_Already Answered lol_

As you can see it's already answered and I just shamelessly copy paste it and here's what it look like in the code.
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/shamelessly-copy-paste-it.png){: width="700"}
_Just Shamelessly Copy Pasting It_

Well not much that I have to say about this other than when I checked it on the calculator the result is correct. Well that is that for first task, now for the real challenge, task number 2. Because I'm slow and kinda dumb when it comes to math, I try asking ChatGPT about the clue for the second task, and looks like I got the key clue about it, here's the clue.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/recursive-function-suggestion-chatgpt.png){: width="700"}
_Suggestion From ChatGPT For Recursive Function_

So, with that on mind, I try my first attempt with this code below.
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/sums-up-recursive-function.png){: width="700"}
_First Attempt of Sums Up Recursive Function_
That is my first attempt to solve the second task, so I tried checking whether it's true that it is the result or not, and to my surprise ChatGPT answering that it is the right call to **change * to + and I still defining x - 1 so it could call function recursively.** But there's some minor minor mistakes, such as:
1. I still naming the function `factorial` but it should be changed to a suited name because it might confused my future self or other people that read my code, so ChatGPT suggesting to change it to something like ``sum_up_to``.
2. I still returning value `1` where instead I should return `0`. So the code should be like this:

```python
def factorial(x):
    if x == 0:
        return 1
    else:
        return x + factorial(x-1)
    
print(factorial(10))
→
55
```
The output is different now, it's 55 now instead of 56. Why? Because when the ``return`` value is `1` it just `+ 1` at the end of summing up. But when it's `return 0`, it will be just ` + 0` at the end of it. That is why it's 55 now. And now it's done for the second task, now we move to the last one, so here's my attempt to do it.

![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/first-attempt-solving-fibonacci-function.png){: width="700"}
_First Attempt Fibonacci_
Now that my stupid ass attempting to solve it failed and it turns into error loop I once again asking ChatGPT for clues and what surprises me that this one right here, below.
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/fibonacci-clues-in-plain-sight.png){: width="700"}
_In Front of My Eyes.... lol_
The clues already mentioned and just right in front of me but I thought that "no way this is the answer" and yep, my skill issue brain got slapped by ChatGPT again.
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/fibonacci-hints-from-chatgpt.png){: width="700"}
_Fibonacci Hints from ChatGPT_

And yep the clue already in front of me but yeah, I'm just that dumb to realize it, so yeah, here's my code using all of those clues.
![Desktop View](assets/img/posts/2025-07-07-python-learning-part-3-modules-and-functions/fibonacci-function-final-answer.png){: width="700"}
_Fibonacci Function Final Answer_
And yep, the function work properly and returning the correct answer. But what is Fibonacci exactly? Fibonacci in simple term is numbers in a set that being summed by the previous values on that set, so something like this:
```bash
n	fib(n)
0	0
1	1
2	1 (1 + 0)
3	2 (1 + 1)
4	3 (2 + 1)
5	5 (3 + 2)
6	8 (5 + 3)
7	13 (8 + 5)
...	...
```
So as you can see from the logic we will be returning 0 if the first value on the set is 0, or else if it's value 1 we will returning one but other than those number we will be doing minus 1 to match previous number(which is 1) and for that point on ward we will use minus 2 to match previous number(being 2) and so on forth.  

And yeah all of the exercises for this chapter is now done and thank you again for follow along my learning journal for this chapter. I'll do next journal for loop and iterations chapter and once again, thank you so much for your time reading this.


