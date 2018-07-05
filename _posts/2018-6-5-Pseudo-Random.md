---
layout: post
title: Everything you need to know about pseudo random numbers
---
In the field of Computer Science we often need random numbers to solve problems. In cryptography we need random numbers to encrypt
messages. In game developement we need random numbers to create enemies at random locations. Earlier humans used to roll a dice
to get a random number between [1,6]. 1 and 6 are included , its a closed interval. With computers you have the ability of generating
really long numbers. 
This post is not about the importance of random numbers ! In order to know more about random numbers visit this 
[A Breif history of Random Numbers](https://medium.freecodecamp.org/a-brief-history-of-random-numbers-9498737f5b6c).

In data science we need numbers which are random but upon every execution of the script generates the same number. 
I know the previous line is hard to digest but the following code will make this clear. 

```python
import random
print(random.random())
```

The above code will generate random numbers everytime. Wont it be great if we would generate numbers that are random but for 
every execution of script will give us the same number ? Well this type of random numbers are known as Pseudo Random Numbers. The 
protagonist of this blog. 

Try this code

```python
import random
random.seed(0)
print(random.random())
```

I have complete faith in python and I am sure that whatever the number comes as the output but it will give the same output
everytime you execute it. 
Consider a sample formula used in generating random numbers.
        X(n+1)=(aX(n)+b) mod(m)

The sequence of number that it generates is X0,X1,X2,X3,….
In this sequence X0 if it is the same every time the numbers following it in the sequence will be the same. So the Random Number
Generators can generate the first number randomly when this X0 is different. This can be considered as the seed of the random 
number generator or is sometimes derived from the seed in some manner.
So random.seed() sets a state and when random.random() is called it uses this particular state to generate new random number.
Added if you know the seed value you can predict the numbers. This  is the reason its called Pseudo Random Numbers.

What if you dont call random.seed() ? Well it turns out that it uses the UNIX epoch time as seed. UNIX epoch time is the time in 
milliseconds from 1 January 1970 00:00:00 UTC. Its a 32bit number.

Let me tell you a really cool problem which we wont face now but we gonna face it eventually or to be exact we will face on 
19 January 2038. 32 bit number have a maximum of 2^31-1 and after adding 1 to it, it will turn negative or it will overflow.
This is what will happen on 19th January 2038. This UNIX  epoch time is 32bit number and will fail us and we might need a better alternative.

Hope you enjoyed the post.
If you find errors in the post please do mail the corrections at [stormivofficial@gmail.com](mailto:stormivofficial@gmail.com)
