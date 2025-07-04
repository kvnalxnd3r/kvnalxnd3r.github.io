---
title: Python Learning, Part 2. Lists
date: 2025-06-23 15:41 +0700
categories: [Programming, Python, Learning]
tags: [programming, python, learning, lists, learning journal]
---

## Chapter 4: Lists

Next chapter after Datatypes, in this chapter it's more focused on how to comprehend how's the lists work in Python, how to use them, and how to process it. Without further ado, here's the stuff that I learned throughout this chapter:

### First lists exercise

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/list-first-exercise.png){: width="800"}
_Chapter 4: Lists_

In this chapter, there's only one basic exercise but with a multiple task that follows it, so I'll go through each one of the task being given. So here's my first attempt solving first task.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/slicing-list-task.png){: width="700"}
_Slicing Lists_

In that code, I just concatenate the two sliced part of the lists whilst I also could do this instead:
```python
l = [1, 4, 9, 10, 23]
print(l[1:3])
print(l[3:5])
```
But I feel like it would be more neat to just meshed them together. The next task is appending the list and here's how my learning goes like.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/appending-list-task.png){: width="700"}
_Appending Lists_

Let me explain the learning process that I just go through here. So my first initial thought would be, "How about if I just use append function but use **AND** operator, I mean it would be joined the two value right?". And to my surprise it's not that simple Ferguso lol. The result shows that I just appending value `0`, so I got curious and search on the internet explanation from [GeeksforGeeks](https://www.geeksforgeeks.org/python/python-bitwise-operators/) about it and it says that **AND/&** operator is processing value integer that being given is in bitwise operation. Here's what it would look like.

```python
1 & 2  → bitwise AND  → 0001 & 0010 = 0000 → 0
```

And for a little bit context, **AND** will only result `1` if both value, in this case binary is both `1`. So yeah, what I thought would be easy task turns out there's a lot to it. But how can we appending multiple value then to the list? And that would be concatenate operator at the of the code. The concatenate at the end shows us that we could adding multiple values to the lists and not replacing the previous values inside it but you could also use it to just adding one value but it's such a waste to not use it for more than one values right?.

There's also one function that does that and that is `.extend([])`. You could just use it like this:

```python
l = [1, 2]
l.extend([3, 4, 5])
print(l)  # → [1, 2, 3, 4, 5]
```

And with that, the values being added just like the way concatenation does it. The next task is calculating the average of all the values inside the list. So here's my learning goes on.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/summing-and-average-lists.png){: width="700"}
_Summing & Average Lists_

Well would you look at that, the average output of the summing is `122` with average `17.4285...` and I feel like it's not the right amount, so when I calculate it with all of the number listed there the result sum has to be `292` average of `32,4444`. But why is the result like that? 

Turns out if we just concatenate the list, we only just joins the value into the lists but in a non-destructive way. What it means is that, we didn't change anything not until we save it with some variable. And notice that only `append()` function that change the list right? Welp let's see how is it if we save the concatenation inside variable, in this case let's save it again in `l` list variable again.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/summing-and-average-lists-fixed.png){: width="700"}
_Summing & Average Lists After Fix_

And with that, we fixed the code. This is really new to me too, because usually I thought it would be automatically saved but welp, the more I know. Okay, unto the next task again, removing values out of list.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/removing-list-values.png){: width="700"}
_Removing Lists Values_

Yes, I know the task told me to just remove `[4, 9]` but I wanted to experiment a little bit. So here's interesting to that happened when I done my little experiment. First I tried removing value by using `.remove()` function, it worked. Try using `del` module combine with defining the range of the index(remember, it start from 0 and end in one index more of the last index) is also working. But when I tried to use `-` thinking that it would work because `+` is working when adding values. But to my surprise it's not simple like that. 

So yeah you could just use `.remove()` when removing one values or `del` module combined with index to remove multiple values.
Here's my fixing of the code previously and let's see the result.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/removing-list-fixed.png){: width="700"}
_Removing Lists Values After Fix_

And the code working fine and dandy now, the values are also being removed as you can see from the output. So yeah, there's two ways to removing specific values out of from the list, I could be not knowing if there's another ways to do it but let me know if there is another way. 

Let's go to the next task, **list comprehensions**. Here's the task in the book.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/list-comprehensions-task.png){: width="700"}
_List Comprehensions Exercises_

With that, let's jump straight to the exercise, start with number 1. Number 1 asking to make list of squares of the first 10 numbers. This below is my first attempt to solve the exercise, keep in mind, this attempt I made without reading the book or any ChatGPT hint at first, so this pure my understanding of the assignment.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/square-number-list-comprehension-first-attempt.png){: width="700"}
_List of Fist 10 Numbers Square First Attempt_

When I tried asking ChatGPT for hint to solve it, it gave this hints to fix it.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/chatgpt-list-comprehension-square-numbers-hints.png){: width="700"}
_Hints From ChatGPT_

And I agree with him, why do I need to square it with the variable itself but the one to my surprise is that I don't need `if` condition, and that I should consider `x ** 2` instead `x ** x`. And when I look back in the book again, I got my clue on this explanation.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/list-comprehensions-square-numbers-book-hint.png){: width="700"}
_Hint From The Book_

And then, it clicked on my mind as what the code would look like. So here's how I complete number 1.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/square-number-list-comprehension-second-attempt.png){: width="700"}
_List of First 10 Squares Numbers Second Attempt_

And turns out, it's correct. You don't even need condition for it. Nice to know that you could be so simple when it comes to making list in Python. This is also new stuff for me as well. So let's move on to number two of the exercise.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/cubes-number-list-comprehensions.png){: width="700"}
_List of First 20 Cubes Numbers Second Attempt_

Exercise number 2 is done by just one attempt because I've somewhat got grasps of the logic and not much to be explained here other than the number of the multiplier set to `3`. Next, exercise number 3, that is **even and odd numbers** from the range of `0 - 20`. Here's my learning goes for exercise number 3.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/event-and-odd-numbers-list-first-attempt.png){: width="700"}
_Even & Odd List Numbers First Attempt_

This is done on second attempt, on this first attempt of me make the list, we see that it uses `for` expression to make it looping so that it could continues until the specific range that we set, in this case I set to `20` but turns out this where my little mistake being pointed out by ChatGPT. Here's what it says.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/event-and-odd-numbers-list-chatgpt-suggestion.png){: width="700"}
_Even & Odd List Suggestion by ChatGPT_

And with that, I fixed the code and turns out, the output is also different from my first attempt code.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/event-and-odd-numbers-list-second-attempt.png){: width="700"}
_Even & Odd List Numbers Second Attempt_

Notice on the list of even numbers it's now included number 20? Yep, little mistake right there really make much difference. So I gotta be careful but you'll see me fumbling sometimes on that kind of little mistake throughout this whole journal lol. Now, when to my surprised, ChatGPT giving me a challenge at the end of the prompt. I thought the challenge is interesting and so I try to solve it, the challenge goes like this.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/event-and-odd-challenge-from-chatgpt.png){: width="700"}
_Even & Odd List Challenge_

Well that challenge is really interesting so for me it turned a simple **true or false** output into a readable format. So here's what attempt looks like when I solve that challenge.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/event-and-odd-challenge-first-attempt.png){: width="700"}
_Even & Odd List Challenge First Attempt_

And to that attempt ChatGPT says, it's indeed correct but it could be more effective and it basically suggesting to me that I could mesh together the `if condition` with the list, so it clicked on my mind and here's how I fix it.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/event-and-odd-challenge-second-attempt.png){: width="700"}
_Even & Odd List Challenge Second Attempt_

And this is also a new thing for me, it's so simple, readable and just look cool to me. This what's makes me excited to keep learning Python basically hahahaha. So we could see that the statement can be just initialized first instead and the rest condition will follow soon and lastly we set the `range` with `for` loop. 

For the next task, we've been asked to make code that shows us the sums of the first 20 numbers from `0 - 20` to be exact. But the twist here is that, it needed to be **even numbers** and the result should be **1140**. The task also need us to check the list by printing it and then sums all of the numbers. So here's my attempt to solve that task.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/summing-square-numbers-list-twenty-first-numbers.png){: width="700"}
_Summing Even Numbers List_

And yep, that exercise previously really makes me _clicked_ the logic of comprehensions list and I just solved it on the first try. As the task said too, the result really is **1140** but it should be done by now right? Welp it is but when I asked ChatGPT it gives me another useful insight to use comprehensions list in Python and it really make the code more concise, here's how it goes.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/summing-square-numbers-list-more-concise-version.png){: width="700"}
_Even & Odd List Challenge Short Version_

And would you look at that, I never thought that the list could just be summed up directly like that lol wkwkwkwkwkwk, and yes I know the range this time is `21` but that is just me want to try expanding the range and see what will happen to the output and looks like the output has been increased `400` because square `20` is included. So yep, that is my learning on this number 4 exercise. 

It's time for the last exercise, this is how I attempting to solve the exercise.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/last-exercise-comprehensions-list-first-attempt.png){: width="700"}
_First Attempt of Last Exercise_

In that first attempt I tried with adding `and x / 2` on the logic there and although the result is correct but I feel like the logic just seems off to me and that is why I checked it in ChatGPT to see it myself what is the wrong there. And my guess was right, here's what ChatGPT suggesting me to fix on my logic.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/last-exercise-comprehensions-chatgpt-suggestion.png){: width="700"}
_Even & Odd List Fix Suggestion_

And turns out my guess was right, the `x / 2` is the one that seems off and with this one suggestion from ChatGPT, I agree with it, the logic is just that it needed to be **NOT divisible by 3**, that is the key here, the **NOT** operator. With that being sad, now that I got it clicked on how to fix it, here's my fix of it.

![Desktop View](assets/img/posts/2025-06-30-python-learning-part-2-lists/last-exercise-comprehensions-list-second-attempt.png){: width="700"}
_Second Attempt of Last Exercise_

The result still the same but the logic and the condition also being more solid here on this fix. And with that, the last exercise task being done, I'm fully done with list learning chapter for now. Next is function and modules chapter, see you on the next journal learning.
