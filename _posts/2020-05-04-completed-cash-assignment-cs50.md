---
layout: post
author: Joris Veldt
tags: [CS50, Programming]
---
Today I finished the cash assigment of first weeks lecture within one hour!! Yesterday I struggled with the [credit]({% post_url 2020-05-03-completed-the-credit-assignment-of-cs50 %}) assignment and worked on it way longer than I did with this assignment.

This assignment was about returning change with as less as possible coins. In this particular exercise I only had coins of 25, 10, 5 and 1 cents. So if I'd get 41 cents in change I would get 4 coins.

For simplicity I was allowed to prompt to the user the amount of change I owed him. This means that the user could fill in how much change he/she needed to receive. The user was allowed to fill in a float number. If he'd paid with a $10 dollar bill and bought a bread of 25 cents he would get $9.75 in change.

I needed to convert this number to cents and used the following solution for that:

```c
round(userChange * 100);
```

Now I always had a round number that I could use for further problem solving. At this point, I needed to initialize a new variable (coins) because I need to say to my customer how many many coins he/she would get.

Then, I wanted to start a loop to iterate over my conditional logic. I used a while loop <code>while (cents > 0)</code>. My conditional logic was as follows, if the cents were bigger than or equal to 25 then I wanted to divide the cents by 25 and assign the result to the variable coins. I used the operate <code>+=</code> because my code would break if there would be a 50 cent coin added later on to my code. It would 'forget' the number of coins of 50 cents.

Finally, I needed to know the total cents left for further evaluation. I solved this problems by using the modulus operator. For example, <code>41 % 25</code> gives 16. This number could be used for further conditional logic.

Here is my solution:

```c
#include <stdio.h>
#include <cs50.h>
#include <math.h>

int main(void)
{

    float userChange;

    // Prompt user how much change we owe him. No negative numbers allowed
    do
    {
        userChange = get_float("Change owed: ");
    }
    // Negative number? Prompt user again
    while (userChange <= 0);

    // Multiply input * 100
    int cents = round(userChange * 100);
    // Initialize coins for return
    int coins = 0;

    // While change is bigger than 0 then...
    while (cents > 0)
    {
        if (cents >= 25)
        {
            // count coins by dividing by 25
            coins += (cents / 25);
            // count remainder of cents by using modulus
            cents = cents % 25;
        }
        else if (cents >= 10)
        {
            coins += (cents / 10);
            cents = cents % 10;
        }
        else if (cents >= 5)
        {
            coins += (cents / 5);
            cents = cents % 5;
        }
        else
        {
            coins += (cents / 1);
            cents = cents % 1;
        }
    }
    printf("%i\n", coins);
}
```