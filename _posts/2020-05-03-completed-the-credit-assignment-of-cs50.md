---
layout: post
author: Joris Veldt
tags: [CS50, programming]
---
I did it! Yesterday after dinner I started with the first week assignment of the [CS50 course](https://courses.edx.org/courses/course-v1:HarvardX+CS50+X/course/). Better known as the credit assignment. I spend the whole night and this morning and afternoon to solve this problem. And it feels damn good that I finally did it! :smile:

So the exercise was about Luhn's Algorithm. This algorithm lets you determine if a credit card number is (syntactically) valid with three (simple - ahum ahum) steps:
1. Multiply every other digit by 2, starting with the number’s second-to-last digit, and then add those products’ digits together.
2. Add the sum to the sum of the digits that weren’t multiplied by 2.
3. If the total’s last digit is 0 (or, put more formally, if the total modulo 10 is congruent to 0), the number is valid!

In my deepest and weakest moment it was very tempting to Google for the solution. But I resisted the temptation and kept puzzling on my own. There were a couple of times that I seriously had the feeling that I came close to the solution. But not all test cases made it so I needed to puzzle again.

I think the most important lesson that I learned today is that I sometimes underestimate how much effort something can be. Could be the case that I don't have a clear understanding (yet) of how much time certain tasks take. I bet senior programmers solve this question within an hour but as junior (I'm calling myself a junior junior junior) it might take longer.

The point is that you think about the structures and every option you have in programming. I considered all option and thought about how the code would work. While loop, Do While loops and For loops. Then, building in the conditional logic. There was even a point I thought about using Switch to evaluate all cases of the first two digits.

I didn't know that I could be so happy with actually solving a problem on my own. Hopefully it is a start to brake the pattern of being trapped in my vicious circle. **Spoiler alert** - here is my solution:

```c
#include <stdio.h>
#include <cs50.h>

int main(void)
{

    // Prompt user to input credit card number
    long cardNo = get_long("Number: ");

    // Initialize variables
    int sum = 0;
    bool sDigit = false;
    int count = 0;

    long firstTwoDigits = cardNo;
    long firstDigit = cardNo;

    // Get the first two digits of credit card
    while (firstTwoDigits >= 100)
    {
        firstTwoDigits /= 10;
    }

    // Get first digit of credit card
    while (firstDigit >= 10)
    {
        firstDigit /= 10;
    }

    // While credit card number is bigger than 0
    while (cardNo > 0)
    {
        // If sDigit is False then
        if (!sDigit)
        {
            // In first run get last number of credit card
            sum += (cardNo % 10);
            cardNo /= 10;
        }

        // If sDigit is True then
        if (sDigit)
        {

            // Initialize value
            int value = (cardNo % 10) * 2;

            // if value bigger than 10
            if ( value >= 10)
            {
                // while value bigger than 10 do the following
                while ( value >= 10)
                {
                    // get last digit and add it to sum
                    sum += (value % 10);
                    // get second digit and add it to sum
                    sum += (value / 10);
                    // divide value by then and go to while loop
                    value /= 10;
                }
                // divide card number with 10 to get next number in run
                cardNo /= 10;
            }
            // if not bigger than 10 do
            else
            {
                // add the number multiplied with two to the sum
                sum += (((cardNo) % 10) * 2);
                // divide by ten to get next number of credit card
                cardNo /= 10;
            }

        }

        // Change to true or false for next loop
        sDigit = !sDigit;
        // counter for future evaluation of credit card company
        count++;

    }

    // if sum modulus 10 is zero then it is a valid credit card
    if (sum % 10 == 0)
    {
        // if first two digits are 34 or 37 and total number of digits is 15 then
        if ( (firstTwoDigits == 34 || firstTwoDigits == 37) && count == 15)
        {
            printf("AMEX\n");
        }
        // else if first two digits are 51, 52, 53, 54 or 55 and count is 16 then
        else if ( (firstTwoDigits == 51 || firstTwoDigits == 52 || firstTwoDigits == 53 || firstTwoDigits == 54 || firstTwoDigits == 55) && count == 16)
        {
            printf("MASTERCARD\n");
        }
        // else if first digit is 4 and count is 13 or 16 then
        else if ( firstDigit == 4 && (count == 13 || count == 16))
        {
            printf("VISA\n");
        }
        else
        {
            printf("INVALID\n");
        }
    }
    // if sum modulus 10 is not zero than it is invalid
    else
    {
        printf("INVALID\n");
    }

}
```