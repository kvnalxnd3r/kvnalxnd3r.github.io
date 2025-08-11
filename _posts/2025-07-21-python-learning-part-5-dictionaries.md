---
title: Python Learning, Part 5. Dictionaries
date: 2025-07-21 17:02 +0700
categories: [Programming, Python, Learning]
tags: [programming, python, learning, lists, learning journal]
---

## Chapter 7: Dictionaries

Another week, another learning journal and this time I'm learning Dictionaries, the part where I will spent most of time to comprehend it(as of time writing this, I've already spent roughly 15 days to just understand the basic and fundamental of it, I know, skill issue from my side. But I'll just write it nonetheless kek...). You will see a lot of "headbanging my head to wall" just to understand this part.

But why am I spending most of my time trying to understand this part? Because in my work as Jr. SOC, I've seen one of my colleague making automation script for SIEM(ELK Stack), and in his script, it was written with Python and I see the codes more closer, his Python codes most of the time use dictionary and list. This really struck me deep and making me think "Welp looks like I can't rush this part until I really understand it" And thus begin my trial and error journey of Python dictionaries.

## Welcome to Dictionaries

huft........... So let's just start, at first glance dictionaries appear to be somewhat "easy" for me personally. You initialize it and put some values inside it, what could possibly hard about that?? Right?? Welp, sike, the part where I don't really understand this chapter lies in how to called and processed the items/values inside of it.

But at first, what is **Dictionaries** in Python? From the book it defines it as "dictionaries are data structures
that indexes values by a given key (key-value pairs)". So from that definition we could assume that we need those key for accessing it. Sounds simple in theory but when I tried solving the task for the first, I got my ass humbled quick. Here's how my attempts looked like.

### Exercises with dictionaries

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/first-task-exercises-with-dictionaries.png){: width="800"}
_Exercises With Dictionaries Section_

For the first part, we see that we have 4 exercises/task that revolved around given dictionary that is `ages`. In this dictionary
is bunch of ages from various students. So without further a do, let's start by task number 1, finding how many students in the dictionary.

- My first attempt for task number one is this.

```python
ages = {
"Peter": 10,
"Isabel": 11,
"Anna": 9,
"Thomas": 10,
"Bob": 10,
"Joseph": 11,
"Maria": 12,
"Gabriel": 10,
}

for names in ages.items():
    print(len(names))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-1-first-attempt-output.png){: width="750"}
_Task Number 1 First Attempt Output_

In here, my goofy ass thinking that, "Oh maybe because I was learning loop previously and combining it with `.items()` to check each items inside the dictionary that I also needed it now to check each of the items inside right? Welp, let's do the looping then". 

That was what I previously thought of it, but as you can see, it is indeed looping each items inside the dictionary but the length that it counted were the length of each items. Where in this case, each items containing two values, the name and age of the students.

So, you guess it, I asked ChatGPT for what is wrong here, did I misunderstood something? or what? And here is ChatGPT response.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/chatgpt-suggetion-to-fix-task-number-1-attempt.png){: width="750"}
_ChatGPT Suggestion For Number 1_

And turns out I am indeed misunderstood the assignment, and that I don't need to loop through each items inside it because all I need to do just count the key of the dictionaries......... Because again, dictionary consist of key and values. And the key represent the values also, so in this case we could just straight using `len()` function. 

So with that, this is how my second attempts for number 1.

```python
print(len(names))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-1-second-attempt-output.png){: width="750"}
_Task Number 1 Second Attempt Output_

With this change, the function just counting the keys within dictionary and the result was 8 keys or 8 students in that dictionary. Whew.... this is just number 1 from many trial and error of me in this chapter and there will be **a lot** of it, so let's resume to the next task and see what kind of slop that I made lol.

- We now move on to number two where we got assigned to calculate the average ages of students contained there. Here's my first attempt.

```python
ages = {
"Peter": 10,
"Isabel": 11,
"Anna": 9,
"Thomas": 10,
"Bob": 10,
"Joseph": 11,
"Maria": 12,
"Gabriel": 10,
}

def agesAverage(x):
    for age in ages.items():
        return sum(age)
    
str(agesAverage(ages))
```
The output of that is an error messages like this one below.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-first-attempt-output.png){: width="750"}
_Task Number 2 First Attempt Output_

Okay, as you can see, those code above is my first attempt to make rough code sketch of this task. Because I was at first didn't quite catch it, my initial thought was that, I first needed to defining function that accepting a variable, then use loop that checks every items in that dictionary and returning it with after done sums it up. Here at the return line, I forgot to make calculation of dividing it with the length of the dictionary.

And finally my initial thought was that I needed to convert it to string before I was able to make shows up on the terminal. And as you can see from the error messages, I got all wrong. The message clear state that it's unsupported operand type for a string and integer to be paired with(if my wording is correct, but cmiiw).

But now I can clearly see on what and I'm where I'm wrong, I can now proceed to my second attempt. But a little bit about context as to why my second attempt would be error like that is because I was using this section when reading it from official [Python documentation of dictionary](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict).

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-second-attempt-references.png){: width="750"}
_References For Number 2 Second Attempt_

In my dumbo understanding, you needed to declare variables in order to accessing the values, which is not true, you could just call `.values()` function, but,  this is when I was still in the dark about what is dictionary. And based on that, here's my second attempt.

```python
ages = {
"Peter": 10,
"Isabel": 11,
"Anna": 9,
"Thomas": 10,
"Bob": 10,
"Joseph": 11,
"Maria": 12,
"Gabriel": 10,
}

name = ages.name()
age = ages.age()

def agesAverage(x):
    x = 0
    for x in age:
        x += age
        return x

agesAverage(ages)
```

And here's the output of my second attempt.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-second-attempt-output.png){: width="750"}
_Task Number 2 Second Attempt Output_

Yeah... when I look back at this attempt of mine, I can't help but to wonder, "WTF did I just wrote?! Am I acoustic or just drunk or stupid or all of it...." And yes, this is so fucking cringe, wtf did I think to just straight up calling `.name()` and `.age()`? Where in God's green earth I initialize that? The reference clearly state `.values()` but oh well, I guess I just pass that phase now because I'm a bit more understand dictionary compared to when I made this mess.

So, without other options left, I once again asked ChatGPT about what is wrong there, and here's what ChatGPT says about my horrendous code.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-chatgpt-first-suggestoin.png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-chatgpt-second-suggestoin.png){: width="750"}
_Task Number 2 Clues From ChatGPT_

Okay... I somewhat got a better understand of it, so basically I need to loop through each of items that contained inside, declaring an empty integer type variable so that it could be filled with number that being added, and finally dividing all of those total number with the length i.e. how much numbers contained in the dictionary. Welp sounds alright, let's see how I done this on my third attempt.

```python
def agesAverage(x):
    totalAge= 0
    for name, age in ages.items():
        totalAge += age
    average= totalAge / len(ages)
    return average

print(agesAverage(ages))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-third-attempt-output.png){: width="750"}
_Task Number 2 Third Attempt Output_

And now we're talking, the suggestion working perfectly and showing the correct answer. Oh and one thing to note on line
`for name, age in ages.items():` you could see variable `name` on the loop is not being used anywhere but why is it still necessary to be put there? Well, this is the thing that I discover when I was playing around with the suggestion of ChatGPT. I'm thinking, how about if I just put one variable on the loop? I mean I just wanted the ages(numbers/integers), it can be just called if we declared only one variable right? Sike! here's my code and what Python say about that wishful thinking of me.

```python
def agesAverage(x):
    totalAge= 0
    for age in ages.items():
        totalAge += age
    average= totalAge / len(ages)
    return average

print(agesAverage(ages))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/what-happened-when-using-one-variable-to-loop-dictionary.png){: width="750"}
_What if you wanted to use one variable but Python said....._

And it's an error, well what I read on the internet is that this happen because incompatibility between tuple(a collection of items or values) being processed directly with an integer data type. So, when I tried digging more information on the internet, I came up with conclusion that the name variable is needed because something is needed to contain the tuple/keys of the dictionary before someone could accessing values. And with it, we should use variable for that task so that the values could be accessed.

We almost done with number 2 but when I check that answer to ChatGPT, to my surprise, it suggesting me I could change my code to this to be more effective. And that is instead of using directly the `ages` dictionary directly, we could just use the `x` parameter that being passed on the function and the code would be look like this.

```python
def agesAverage(x):
    totalAge = 0
    for name, age in x.items():
        totalAge += age
    average = totalAge / len(x)
    return average

print(agesAverage(ages))
```

Voila! Our code not wasting any variable and more efficient. I think that is all from me for number 2, let's move to number 3.

- Now, we're arrived to task where I struggle and I attempt so many codes and logics on this number, and it still baffles me how much of a goofball I'm lol. This task assigning me to find oldest student in the dictionary, at first I was thinking "Well, how hard could it be? It's just `if else` comparisons" and oh sweet summer child... So, please enjoy this many slop codes that I made lmao. 
 
Oh, I made this part just showing code and no output screenshot, in my opinion because of the amount of trial and error, showing screenshot of each attempt would make this journal too long for just showing a slop code that bound to fail, so I just made the output and the code shows together.  

```python
# Attempt 1
def oldestStudent(x):
    oldestAge = age[0] 
    for name, age in ages.items():
        if age > oldestAge:
            oldestAge = age
    print(f"The oldest student is: {name}")
    return oldestAge

oldestStudent(ages)

→ Output:
line , in <module>
oldestStudent(ages)

oldestAge = age[0]
                ^^^
UnboundLocalError: cannot access local variable 'age' where it is not associated with a value

# Attempt 2

def oldestStudent(x):
    x = ages[0]
    for name, age in ages.items():
        if age > x:
            x = age
    print(f"The oldest student is: {name}")
    return x

oldestStudent(ages)

→ Output:
ine  , in <module>
    oldestStudent(ages)
oldestStudent
    x = ages[0]

# Attempt 3
def oldestStudent(x):
    oldestAge = x[ages["Peter"]]
    for name, age in ages.items():
        if age > oldestAge:
            oldestAge = age
            print(f"The oldest student is: {name}")
    return oldestAge

oldestStudent(ages)

→ Output:
line , in <module>
    oldestStudent(ages)

in oldestStudent
    oldestAge = x[ages["Peter"]]
                ~^^^^^^^^^^^^^^^
KeyError: 10

## Attempt 4
def oldestStudent(oldestAge):
    oldestAge = ages['Peter']
    for name, age in ages.items():
        if age > oldestAge:
            oldestAge = age
    print(f"The oldest student is: {name}")
    return oldestAge

oldestStudent(ages)

→ Output:
The oldest student is: Gabriel ## Hah!, now it's outputting something, but I think the 
## return is on the outside of the function and that is why it's only 
## showing Gabriel which is not true, this is because my guess is that 
## function outputting it outside of the loop making only the last value that being shows.
## And in this code, I compared oldestAge variable which the same being the age of
## Peter and that way Peter could served as comparisons to find the oldest age. But
## As you can see, it doesn't work that way so there's that.

# Attempt 5
def oldestStudent(oldestAge):
    oldestAge = ages['Peter']
    for name, age in ages.items():
        if age > oldestAge:
            oldestAge = age
            return name

print(oldestStudent(ages))
→ Output:
Isabel
## Now, I tried to see what happened if I placed the return inside the loop and
## turns out it's working yes, but it stopped when it find already biggest number
## than Peter age(10) and that is Isabel. Making all other student age didn't
## get chance to be compared.

## Attempt 6
def oldestStudent(oldestAge):
    oldestAge = 0
    for name, age in ages.values():
        if age > oldestAge:
            oldestAge = age
            return name

print(oldestStudent(ages))
→ Output:
in <module>
    print(oldestStudent(ages))
          ~~~~~~~~~~~~~^^^^^^

in oldestStudent
    for name, age in ages.values():
        ^^^^^^^^^
TypeError: cannot unpack non-iterable int object
## On this sixth attempt, I tried to accessing the values directly by using `.values()`
## but alas, the compatibility issue got me stuck. I can't just straight using integer
## with non-iterable(list, dictionary, etc) object. 

## Attempt 7
def oldestStudent(oldestAge):
    oldestAge = 0
    for name, age in ages.items():
        if age > oldestAge:
            oldestAge = age
            return name

print(oldestStudent(ages))

→ Output:
Peter
## This the last attempt I made before I went to ChatGPT for hints, so in this attempt
## I tried back again using `.items()` function because seeing that I needed it for
## when accessing the dictionary. And to my surprise, this discovery of mine returning
## something instead error message but not the answer we seeking, because as I read
## this code again, I see that I just comparing 0 with whatever first number that 
## appear when accessing dictionary and this time that number is Peter age(10)
## and just like that the code assuming the oldest number there is 10 and stopped
## the loop. This is not the answer we want, so after this, I consult to ChatGPT
## as to what am I missing lol.
```
With that being said, the tips given by ChatGPT.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-oldest-age-function-hints(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-oldest-age-function-hints(2).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-oldest-age-function-hints(3).png){: width="750"}
_Hints From ChatGPT For Finding Oldest Age_

As you can see, the hints suggesting changing `return` position to outside again, initializing variable to save the name of the oldest student as an empty variable with data type of string, and lastly, this is the crucial part, I need to declare that the `oldestAge` and `oldestName` is the same with `name` and `age` from the dictionary. 

So what it mean from my understanding is that **you first declared `age` is 0 → and the loop happen → it start checking the dictionary → code begin comparing age which is 0 at the start → then because the first age it encounter is Peter age(10), we saved that again as the new value of `age` and as well because it contained the `name` we also save it again to the `oldestName` variable → and this keep looping until the last value of the `ages` dictionary → after the loop done, the loop then return the `oldestName` as the result of the comparison that just happen in the loop operation**. 

```python
def oldestStudent(students):
    oldestAge = 0
    oldestName = ""
    for name, age in students.items():
        if age > oldestAge:
            oldestAge = age
            oldestName = name
    return oldestName

print(oldestStudent(ages))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-eigth-attempt-output.png){: width="750"}

I feeling so dumb when not realizing that you only just need to declared inside the loop that the `oldestAge = age`, and the fact that I also think that you wouldn't need to save name variable because the age already there. I just start face palm but oh well, part of learning is that realize how cringe and stupid you're before you have the knowledge. But we still have many more goofy stuff ahead of us, so let's go to the next number 4!

- For task number 4, we've been tasked to return new dictionary where that new dictionary contain a specific age of student that you wanted to search, for example new_ages(ages,10) returns a copy of `ages` where each student is 10 years older.

But this is where I misunderstood that this task wanted me to add 10 to the students ages and not searching age of students that equal to 10 🤦(silly me). But well, here's what this dum dum made below.

```python
def oldestStudent(newStudentDict):
    ageTenStudents= {}
    studentName= ""
    studentAge= 0
    for name, age in newStudentDict.items():
        if age == 10:
            name == studentName
            age == studentAge

    return ageTenStudents

print(oldestStudent(ages))

→ Output:
{}
## God.... what the fuck did I think this code will accomplish.... I just
## punching air if it's like that.... I didn't adding anything to the 
## empty list and just straight up returning empty bag.... 
## And yes, I forgot to change the function name lol, I got no excuse
## for that other than me being lazy bump.

## Attempt 2
def oldestStudent(newStudentDict):
    ageTenStudents= {}
    studentName= ""
    studentAge= 0
    for name, age in newStudentDict.items():
        if age == 10:
            studentName = name
            studentAge = age
            ageTenStudents = {studentName, studentAge }
    return ageTenStudents

print(oldestStudent(ages))

→ Output:
{10, 'Gabriel'}
## Lmao... I got to admit, this part is because my half assed understanding 
## and my curiosity and yep I kinda surprised this code work in the first
## try even though it's wrong lol.
```

So with that, I ask ChatGPT again as to what's wrong here. First ChatGPT help me clear the misunderstanding by this answer.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-4-explaining-misunderstanding.png){: width="750"}
_ChatGPT Help Me To Understand The Task Assignment_

Now that I understand it, we then continue to the hints that ChatGPT suggest.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-4-plus-10-age-function-hints(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-4-plus-10-age-function-hints(2).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-4-plus-10-age-function-hints(3).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-4-plus-10-age-function-hints(4).png){: width="750"}
_Hints From ChatGPT For Solving Task Number 4_ 

At this point, I know that:
1. I was right to define an empty variables for each properties(dictionary, name, age).
2. I was wrong to understand that you need to find students with age equal of 10 but I need to add 10 for each age of students.
3. The loop need to add 10 for each of the ages.

But also there's my another misunderstanding from that hints Iwas thingking that **_dictionary_ is declared with `[]` and _set_ is declared `{}`** so I thought "Oh okay, all should do now just add an empty dictionary, add the plus 10 age to it then return the dictionary" so, here's how my next went.

```python
def newStudentDict(newDict):
    plusTenAge= []
    for name, age in newDict.items():
        plusTenAge[name] = age +10
    return plusTenAge

print(newStudentDict(ages))

→ Output:
in <module>
    print(newStudentDict(ages))

in newStudentDict
    plusTenAge[name] = age +10
    ~~~~~~~~~~^^^^^^
TypeError: list indices must be integers or slices, not str
```

And yep, this really confuses me previously, because I thought huh? I thought "why the incompatibility? I thought dictionary was **[]** and the list is **{}**" so yep, time to ask ChatGPT(yet again...... lol) to clear up how and where did I wrong.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/clearing-misunderstanding-of-list-and-dictionary(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/clearing-misunderstanding-of-list-and-dictionary(2).png){: width="750"}
_Clearing Misunderstanding of What Is List vs Dictionary_ 

Okay, now this really confuses me again because up to this point, from my understanding, I got it switched, so I kinda crash out a bit with ChatGPT as you can see from this picture below.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/asking-nicely-chatgpt-to-explain-list-and-dictionary(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/asking-nicely-chatgpt-to-explain-list-and-dictionary(2).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/asking-nicely-chatgpt-to-explain-list-and-dictionary(3).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/asking-nicely-chatgpt-to-explain-list-and-dictionary(4).png){: width="750"}
_Asking ChatGPT Nicely What Is List vs Dictionary_ 

From the hints you can see I already solved it but what I wanted to say that, yes, I previously confusing code that looked like this `my_set = {"Peter", "Anna"}` with this `my_dictionary = {"Peter": 10, "Anna": 12}` and that is because I was so fixed to the idea that set is using curly bracket lol. So, here's my final code after long trial and error along with many hints from ChatGPT.

```python
def newStudentDict(newDict):
    plusTenAge= {}
    for name, age in newDict.items():
        plusTenAge[name] = age +10
    return plusTenAge

print(newStudentDict(ages))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-4-plus-10-age-function-final-attempt-output.png){: width="750"}
_Final Attempt Output_ 

With that, I was learning so much, start from what is exactly dictionary, interacting key and values in dictionary, using Loop to process each values in dictionary. Up next, more learning of dictionary and this is where I was stuck so long learning dictionary because this was so new to me, especially after coming back from not learning programming for 2 years and that is **sub-dictionaries**. Let's get into it!

### Exercises with sub-dictionaries

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/second-task-exercise-with-sub-dictionaries.png){: width="750"}
_Second Exercises Section With Sub-dictionaries_

For me, this exercises really makes me learning new things **a lot** because so many things that I thought previous I got it understand turns out to be just straight up wrong and this makes me asking ChatGPT so many times lmao. So, let's go to the learning and see what things that I still got it wrong.

- We start again with exercise number 1 of this section counting how many students on the number 1 dictionary.

```python
students = {
    "Peter": {"age": 10, "address": "Lisbon"},
    "Isabel": {"age": 11, "address": "Sesimbra"},
    "Anna": {"age": 9, "address": "Lisbon"},
}

len(students)

→ Output:
3
```

Nothing new for task number 1, it's just we calling `len()` function and let the function do the rest, so we got `3` as the output. Let's just get straight into number task two.

- Here's my attempts for task number two that assigning us to search for the average age of the student in the dictionary.

```python
## Attempt 1
def studentsAvrgAge(x):
    totalAge= 0 
    for name, age in students.items():
        age += totalAge
        averageAge = totalAge / len(x)
    return averageAge

print(studentsAvrgAge(students))
→ Output:
in <module>
    print(studentsAvrgAge(students))
          ~~~~~~~~~~~~~~~^^^^^^^^^^

in studentsAvrgAge
    age += totalAge
TypeError: unsupported operand type(s) for +=: 'dict' and 'int'

## Attempt 2
def studentsAvrgAge(x):
    totalAge= 0 
    for age in students.values():
        age += totalAge
        averageAge = totalAge / len(x)
    return averageAge

print(studentsAvrgAge(students))

→ Output:
in <module>
    print(studentsAvrgAge(students))

in studentsAvrgAge
    age += totalAge
TypeError: unsupported operand type(s) for +=: 'dict' and 'int'
```
On that attempt I only thought that I was just using `.values()` here wrong, I thought that I needed to use `.item()`, so then I asked ChatGPT thinking that only that part that I was just wrong. But to my surprised here's ChatGPT hints.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-subdictionary-average-function(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-subdictionary-average-function(2).png){: width="750"}
_Hints For Fixing Average Age Sub-dictionary_

I surprised that my first initial thought of using `.values()` to interact with the sub-dictionary turns out to be true. Because I think that `.item()` is used for a single dictionary and that `.values()` used to interacting what is inside the sub-dictionary. But it is my addition that is wrong, and because of that the code thinking that I operating dictionary with an integer. But as you already know, it's just wrong and that you need to be specific as to how you accessing it.

And that is why ChatGPT suggesting to use `age[age]` in order for us to be more specific as to what we accessing. That way, we could start operating `totalAge` with age in the sub-dictionary because the code now knows what values we want to access and it is located in side `age` sub-dictionary, the one containing an integer. 

Now we know the necessary information's to solve this task, here's how I implement it in my code on this final attempt.

```python
def studentsAvrgAge(x):
    totalAge= 0 
    for age in students.values():
        age += totalAge
        averageAge = totalAge / len(x)
    return averageAge

print(studentsAvrgAge(students))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-2-subdictionary-average-function-third-attempt-output.png){: width="750"}
_Third Attempt Output of Task Number 2 Average Age_

And yep, let's double check on the output just in case using calculator.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/double-check-on-sub-dictionary-task-number-2-output.png){: width="750"}
_Okay, The Output Is Correct_

Now that task number 2 done, we to final exercise of this section. Let's get into it.

- We got assigned to make function that will return new list if the address being inputted is correct like the example shown in the question. So, wanna see how I make another slops? Without further ado, let's see it.

```python
students = {
"Peter": {"age": 10, "address": "Lisbon"},
"Isabel": {"age": 11, "address": "Sesimbra"},
"Anna": {"age": 9, "address": "Lisbon"},
}

## Attempt 1
def findStudents():
    newStudentsList= {}
    for name, address in students.values():
        if students[address] == students[name]:
            return newStudentsList

print(findStudents(students, 'Lisbon'))

→ Output:
in <module>
    print(findStudents(students, 'Lisbon'))
          ~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^
TypeError: findStudents() takes 0 positional arguments but 2 were given
## I don't know why the output stating like that, because at this point
## I was still try to grasping the concept of as to how interact with
## sub-dictionary values, so I thought just declaring `students[address]`
## or something like that would be sufficient. BUt oh well, looks like I'm
## just that stupid. So in the next attempt, I tried to put parameter that
## will accept anything i.e. using *args if the issue was like what being stated
## by the error message. Let's see what will be the output.


## Attempt 2
def findStudents(*inputAddress):
    newStudentsList= {}
    for address in students.values():
        if inputAddress == address["address"]:
            return newStudentsList

print(findStudents(students, 'Lisbon'))

→ Output:
None
## And another wacky attempt..... reading this back, yeah....
## what was I thinking returning an empty dictionary, stupid me...
## I have no other explanation that I still need better understanding of this
## dictionary topics.
```

As you can see, all my attempt is just straight goofy even stupid, and at this point too this learning really taking a toll to me and making feel like **really** wanted to quit learning Python or programming in general. Because I think that despite learning all of this and already having so much clues being thrown at, I still doesn't make it clicked in my mind, this whole programming shenanigans. 

But before I really was about to give up again, I once again asking ChatGPT for another guidance to help me pointing out where did I do wrong. And here's what it suggest to me for the fixing.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-new-list-function-chatgpt-hints(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-new-list-function-chatgpt-hints(2).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-new-list-function-chatgpt-hints(3).png){: width="750"}
_ChatGPT Hints For Solving Checking List & Return New List_

From this prompt you can see I complained a bit to ChatGPT lol, because yeah, to me this problem really makes me wreck my brain and really challenges my understanding for this stuff. The function `.values()` the one that I thought correct because I need to only check the sub-dictionary turns out wrong because I was still need to check all of the stuff in the whole dictionary.

The `*args` variable that I thought could be solution for accepting all type of data is wrong too, because it turns the input into tuple. And don't forget the `{}` vs `[]` differences, stuff like this really makes me confused. And what's boggles my mind is that, the overall rough logic of my second attempt is somewhat close enough, and only wrongs in my attempt was just how I calling stuffs, and some forgotten mentioning function. But overall it really close enough..... 

But yeah, here's my last attempt after being given the hints.

```python
def findStudents(studentDict, address):
    newStudentsList= []
    for name, info in studentDict.items():
        if info["address"] == address :
            newStudentsList.append(name)
    return newStudentsList

print(findStudents(students, 'Lisbon'))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/task-number-3-new-list-function-third-attempt.png){: width="750"}
_Final Attempt Output For Solving Task Number 3_

And with that I finally solved all of the task available for the Dictionary chapter from the book, but I feel this is somewhat not enough, so I return to ChatGPT and asked it why this code worked and all of my previous attempt failed. And here's what it prompted me.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/chatgpt-explaining-the-logic-of-the-return-new-list-function(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/chatgpt-explaining-the-logic-of-the-return-new-list-function(2).png){: width="750"}
_ChatGPT Helps Me Explaining The Logic of Why Previous Code Worked_

And yeah, I still complaining because I just thought to myself back then at this point that I just that stupid to not even understand this and asked ChatGPT if I just give up because I previously give up programming too but then ChatGPT says not to and saying that I just need to see that you should learn the why that it shows on the picture above.

So from it's suggestion, ChatGPT saying my code gives tuple not a string and that I need change `{}` to `[]`, use proper comparisons with pointing to correct variable i.e. `info["address"]` that is because I that way it could properly loop through the dictionary first and after that start the comparison unlike my code where it just compare first and then do the loop(I got it switched). 

I also need to add the value after comparison in which that I not do it on my attempts. I just straight returning an empty list, and lastly, to make the code checked everything on the dictionary I should use `.items()` and not `.values()`. That way I could check the entirety and not just the sub-dictionary when doing this kind of function.

Last but not least, some advice from ChatGPT.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/some-advice-before-i-decided-to-quit-choose-not-to.png){: width="750"}
_Advices From ChatGPT_


### Extra dictionary & sub-dictionary exercises

And from that advices what leads us to this section of this learning journal. In this section, I want to try more exercises generated from ChatGPT and challenge myself to what I still lacking and what stuff I should understand more and where my misunderstanding is. That way I could confidently move on to the next chapter of this book, **Classes**. Because when I see what kind of thing classes is, I just thought "yep, I gotta strengthening my understanding first before tackling such material."

Let's what exercises ChatGPT generated for me and also let's try to solve it! Here's the first exercise.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/extra-exercises-number-1.png){: width="750"}
_First Challenge Exercise_

Okay, so this is the first exercise from the custom challenge, it assigning us to group the students by their age. In technical terms, return a new dictionary where the keys i.e. `age` and the values are `names`. So it switch places and with some extra sorting. 

Welp, that's about it for the first exercise, let's see my first attempt below.

```python
students = {
    "Peter": 10,
    "Isabel": 11,
    "Anna": 9,
    "Thomas": 10,
    "Bob": 10,
    "Joseph": 11,
    "Maria": 12,
    "Gabriel": 10,
}

def studentAgeGroup(studentDict):
    newGroup= []
    for name, age in studentDict.items():
        newGroup.sort()
        newGroup.append(age)
    return newGroup

print(studentAgeGroup(students))

→ Output:
[9, 10, 10, 10, 11, 11, 12, 10]
```

So I got the age sorted but no name being displayed.... It's just goofy and silly at the same time seeing me this back again but then what this losers do when he couldn't comprehend something like this? Yep, ask the AI again to help him braining. So here's what ChatGPT says what my error and wrongs really mean.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/first-clues-for-dumbass-of-extra-exercise-number-1.png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/second-clues-for-dumbass-of-extra-exercise-number-1.png){: width="750"}
_Clues For Dumbass_

For the clue, what I got right was that loop through each of the value using `.items()` and I somewhat have the right call to declared variable that will hold the value, like a list or something. But what I still got wrong was that, `{}` is what you need not `[]`(I still confusing the two of them at this point). And something that ChatGPT suggest to look for was that how to exactly group them and not using `.sort()` function. Because you could do it without it only using conditional statement.

And what ChatGPT by that is on second clues above. I still didn't understand how to use this kind of code `some_list[...]` to specifying that I need to loop up inside of the dictionary like that. So yeah, ChatGPT already helped me answering it so let's just test the logic inside of the code.

```python
def studentAgeGroup(studentDict):
    newGroup= {}
    for name, age in studentDict.items():
        if age not in newGroup:
            newGroup[age] = []
        newGroup[age].append(name)
    return newGroup

print(studentAgeGroup(students))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/extra-exercise-number-1-second-attempt-output.png){: width="750"}
_Second Attempt Output_

So... the code was correct but it kinda leaves me doubting myself, because up to this point, I still need to turn back to ChatGPT for solving the task. And leaves me really doubting myself, so I'll just leaves this whole prompt below to really captures what my worried was really about.

But, first, it is indeed me being wheeny whiny about this whole ordeal but I just feels like to put this here for someone just like me that feels the same when learning this kind of stuff. Because it really is that mental draining. Mock me or whatever, but I'll just leave it here.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/some-of-my-worriness(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/some-of-my-worriness(2).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/some-of-my-worriness(3).png){: width="750"}

From those words, I just realized that I just to simply push through this thing, because this is a marathon not a sprint. You have to get good at being so stubborn about this stuffs. So, it came across my mind that I need to do one more section exercise to make it clicked in my mind this whole dictionary thingy. So the next exercise below is from ChatGPT and not the book. 

Let's see what this second extra exercise had in store for me. Here's what the assignment ask us to do.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/second-task-extra-exercises(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/second-task-extra-exercises(2).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/second-task-extra-exercises(3).png){: width="750"}
_First Extra Task Exercise_

```python
## First Attempt
students_scores = {
    "Alice": {"Math": 88, "Science": 90, "History": 75},
    "Bob": {"Math": 70, "Science": 78, "History": 85},
    "Charlie": {"Math": 95, "Science": 80, "History": 70},
    "Dina": {"Math": 60, "Science": 65, "History": 72}
}

def calculate_students_average(scores_dictionary):
    student_grades = students_scores.values()
    student_name = students_scores.keys()
    student_sums = 0
    avg_grade_student_dict = []

    for grade in student_grades:
        student_sums += grade
    return student_sums / len(students_scores)

print(calculate_students_average(students_scores))

→ Output:
line 18, in <module>
    print(calculate_students_average(students_scores))

in calculate_students_average
    student_sums += grade
TypeError: unsupported operand type(s) for +=: 'int' and 'dict'
```

Let's see what I still got it wrongs, it is with the `.values()` and `.items()` here, also the calculation logics too, well because mostly I'm still confused by the whole of it, so I just asked ChatGPT again for tips.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/another-tips-from-chatgpt-for-student-report-function(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/another-tips-from-chatgpt-for-student-report-function(2).png){: width="750"}

And I get it from this onward, and yeah, here's how I fix it.

```python
def calculate_avg_students_score(students_score):
    students_top_avg = 0
    top_students_name = ""

    for students_name, students_grade in students_score.items():
        avg_grade = sum(students_grade.values()) / len(students_grade)
        if avg_grade > students_top_avg:
            students_top_avg = avg_grade
            top_students_name = students_name

    return f"Student: {top_students_name} with the average {students_top_avg}"

print(calculate_avg_students_score(students))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/correct-output-students-report-function.png){: width="750"}
_Correct Output For My Second Attempt_

So with confusion in my mind about `.values()` and `.items()` and as to why my code fails, I asked ChatGPT again to clear things up and here's what it answer me.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/explanation-as-to-why-my-student-performance-report-funciton-fails(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/explanation-as-to-why-my-student-performance-report-funciton-fails(2).png){: width="750"}
_Some More Explanation_

Okay, now I kinda get it, but for now, I take a short break after solving this task. I somewhat feels a bit burned out so I take some good rest for one day and back again to solving another exercise. But this time, I asked ChatGPT to make the difficulty become more increasing gradually.

For this section, I'll just speed running it, as this journal already gone far too long lol.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/another-five-extra-exercises-for-making-me-understand-dictionary(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/another-five-extra-exercises-for-making-me-understand-dictionary(2).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/another-five-extra-exercises-for-making-me-understand-dictionary(3).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/another-five-extra-exercises-for-making-me-understand-dictionary(4).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/another-five-extra-exercises-for-making-me-understand-dictionary(5).png){: width="750"}
_Another Extra Exercises For Understanding Dictionary_

Okay, now I've another exercises being prompted, let's just get into it and see what slops attempt I made and what learning I could gained from solving this.

- For level 1, it's quite easy because I just simply printing string and calling the right function.

```python
books = {
    "1984": "George Orwell",
    "Dune": "Frank Herbert",
    "The Hobbit": "J.R.R. Tolkien",
    "Brave New World": "Aldous Huxley"
}

def printing_book():
    for book_title, book_author in books.items():
        return f"{book_title} was written by {book_author}"
    
print(printing_book())
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-one-extra-exercises-output.png){: width="750"}

Or if you feels fancy, ChatGPT also could suggesting something like this.

```python
def printing_book():
    output = ""
    for book_title, book_author in books.items():
        output += f"{book_title} was written by {book_author}\n"
    return output

print(printing_book())
```
- Unto level 2, here's what my attempt looks like.

```python
fruits = ["apple", "banana", "apple", "orange", "banana", "apple"]

def counting_fruits(fruit_basket):
    for fruit in fruit_basket.items():
        if fruit == fruit_basket.values():
            print()
```

That is my rough attempt for solving level 2, so I kinda get the logic but I still feel like missing something on it, so I asked ChatGPT for hints.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-two-extra-exercises-hints.png){: width="750"}

So, I get it now, I missed to declare new dictionary, the comparison is somewhat correct but just missed on the checking and I don't need the `.items()` function. So with it, here's what my second attempt.

```python
fruits = ["apple", "banana", "apple", "orange", "banana", "apple"]

def counting_fruits(fruit_basket):
    new_basket = {}

    for fruit in fruit_basket:
        if fruit in new_basket:
            new_basket[fruit] += 1
        else:
            new_basket[fruit] = 1
    return new_basket

print(counting_fruits(fruits))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-two-extra-exercise-second-attempt-output.png){: width="750"}


- This one next is my attempts solving level 3, so here it is.

```python
scores = {
    "Alice": 84,
    "Bob": 91,
    "Charlie": 67,
    "Dina": 91
}

def find_highest_score(scoreboard):
    highest_score= 0 
    player_name= ""
    for player_name, player_score in scoreboard.items():
        if player_score > highest_score:
            print("Highest score is ", highest_score, "by: ", player_name)

print(find_highest_score(scores))

→ Output:
Highest score is  0 by:  Alice
Highest score is  0 by:  Bob
Highest score is  0 by:  Charlie
Highest score is  0 by:  Dina
None
```

Okay, so for this first attempt, I wanted to test the water and see where my initial logic wrong, and from this I could see and guess that I wrongly calling the variable loop name, the comparison is also basically just comparing the students score with 0, so of course it always shows it as the biggest number. 

And lastly in my guess I need the `return` because that output still showing `none`. So once again I checked my wrong on ChatGPT and see what it thinks about my guess.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-three-extra-exercises-hints(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-three-extra-exercises-hints(2).png){: width="750"}

Looks like ChatGPT thinking the same thing with me. Let's solving this now shall we?

```python
scores = {
    "Alice": 84,
    "Bob": 91,
    "Charlie": 67,
    "Dina": 91
}

def find_highest_score(scoreboard):
    highest_score= 0 
    top_player= ""
    for player_name, player_score in scoreboard.items():
        if player_score > highest_score:
            highest_score = player_score
            top_player = player_name

    return("Highest score is", highest_score, "by:", top_player)

print(find_highest_score(scores))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-three-extra-exercise-second-attempts-output.png){: width="750"}

And the level 3 done, but ChatGPT had also some advices that could improve my code that my line below.

```python
return("Highest score is", highest_score, "by:", top_player)
```

It's returning a **tuple**, in which is correct logically but a little bit messy. So here's the suggested fix.

```python
return f"Highest score is {highest_score} by: {top_player}"
```

Now it will print output appropriately. We then move to level 4 exercise.

- For number 4, my attempt looks like this. This attempt inspired by my first attempt in the level 3 exercise.

```python
students = {
    "Alice": {"Math": 90, "Science": 85},
    "Bob": {"Math": 70, "Science": 80},
    "Charlie": {"Math": 85, "Science": 95}
}

def calculate_avg_students_score(students_score):
    students_top_avg= 0
    top_students_name= ""

    for students_name, students_grade in students_score.items():
        avg_grade= sum(students_grade.values()/len(students_grade))
        if students_grade > students_top_avg:
            students_top_avg = students_grade
            top_students_name = students_name
    return f"Student: {top_students_name} with the average {avg_grade}"

print(calculate_avg_students_score(students))

→ Output:
in <module>
    print(calculate_avg_students_score(students))

in calculate_avg_students_score
    avg_grade= sum(students_grade.values()/len(students_grade))
                   ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~
TypeError: unsupported operand type(s) for /: 'dict_values' and 'int'
```

Okay... so it's kinda wrong on the data type, so let me ask ChatGPT again for the hints again, because I kinda get it now a little bit but I notice I always lacking on the handling data. So here's what ChatGPT suggest me.

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-four-extra-exercise-hints(1).png){: width="750"}

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-four-extra-exercise-hints(2).png){: width="750"}

And finally here's my final attempt after following the hints from ChatGPT.

```python
students = {
    "Alice": {"Math": 90, "Science": 85},
    "Bob": {"Math": 70, "Science": 80},
    "Charlie": {"Math": 85, "Science": 95}
}

def calculate_avg_students_score(students_score):
    all_averages = {}
    students_top_avg = 0
    top_students_name = ""
    average_class_score = 0

    for students_name, students_grade in students_score.items():
        avg_grade = sum(students_grade.values()) / len(students_grade)
        all_averages[students_name] = avg_grade  # save each average
        if avg_grade > students_top_avg:
            students_top_avg = avg_grade
            top_students_name = students_name
        average_class_score += avg_grade
        class_avg = average_class_score / len(students_score)

    return all_averages, f"Top student: {top_students_name} with avg {students_top_avg}", class_avg

all_avg, top, avg = calculate_avg_students_score(students)
print("All Averages:", all_avg)
print("The top student is:", top)
print("And the class average is:", avg)
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-four-extra-exercise-final-attempt-output.png){: width="750"}

And that's it, but I asked ChatGPT as to why I'm getting error `float object is not iterable` when I tried use `sum()` on the `avg_grade` variable. Because I thought you would just sums it up all, but ChatGPT told me that `sum()` only work on list and anything similar to that and if you use raw number it wouldn't work. So instead, ChatGPT suggesting this code below for my initial logic.

```python
students = {
    "Alice": {"Math": 90, "Science": 85},
    "Bob": {"Math": 70, "Science": 80},
    "Charlie": {"Math": 85, "Science": 95}
}

def calculate_avg_students_score(students_score):
    all_averages = {}
    students_top_avg = 0
    top_students_name = ""

    for students_name, students_grade in students_score.items():
        avg_grade = sum(students_grade.values()) / len(students_grade)
        all_averages[students_name] = avg_grade  # save each average

        if avg_grade > students_top_avg:
            students_top_avg = avg_grade
            top_students_name = students_name

    # Now we calculate the class average correctly
    class_average = sum(all_averages.values()) / len(all_averages)

    return all_averages, f"Top student: {top_students_name} with avg {students_top_avg}", class_average

all_avg, top, class_average_score = calculate_avg_students_score(students)
print("All Averages:", all_avg)
print("The top student is:", top)
print("And the class average is:", class_average_score)
```
So in here we use `.values()` again but it's on the new dictionary that we just declared. Okay, let's move on to the last level.


- Lastly, level 5 is where I tried this logic first and then asking ChatGPT about what's am I missed from this. Because this is just my first initial thought.

```python
employees = {
    "Alice": "Engineering",
    "Bob": "HR",
    "Charlie": "Engineering",
    "Dina": "Marketing",
    "Eve": "HR"
}

def sorting_employees(employees_position):
    employees_books = {}

    for employees_name, employees_division in employees_position.items():
        if employees_books[employees_division] == employees_books[employees_name]:
```

I stopped on the conditional line because this is where I sense something wrong. And yes, when I asked ChatGPT what is wrong on my initial logic, it pointed to my condition logic and suggesting me to put it like this.

```python
if division not in employees_books:
    employees_books[division] = []

employees_books[division].append(name)
```

That `not in` is all I needed to know where I got it wrong, and after that clue, here's my final code.

```python
employees = {
    "Alice": "Engineering",
    "Bob": "HR",
    "Charlie": "Engineering",
    "Dina": "Marketing",
    "Eve": "HR"
}

def sorting_employees(employees_position):
    employees_books = {}

    for employees_name, employees_division in employees_position.items():
        if employees_division not in employees_books:
            employees_books[employees_division] = []
        employees_books[employees_division].append(employees_name)
    return employees_books

print(sorting_employees(employees))
```

![Desktop View](assets/img/posts/2025-07-21-python-learning-part-5-dictionaries/level-five-extra-exercise-final-attempt-output.png){: width="750"}


And........... that's it! I finally completed this journal. Whew, such a long journey. I just wanted to say thank you for reading all of this bullshit. And if you learning something from this crap, I'm really humbled and glad for that. Thank you again and see you on the next learning journal.

