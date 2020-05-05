---
layout: post
author: Joris Veldt
tags: [CS50, programming] 
---
Today I completed the readability assignment on CS50. I was asked to write a program that could read the reading level of a book or text. This could be calculated with the Coleman-Liau index: <code>index = 0.0588 * L - 0.296 * S - 15.8</code>

In the morning I followed the lecture videos of week 2. Around 3 hours of lecture material to watch. I planned to work on it for the whole day because I had a day off from work (it is Liberation Day in the Netherlands). However, my grandparents and sister dropped by over the day so I had my social activities as well. Furthermore, I force myself to gym and/or run everyday because it keeps me energetic. I didn't do as much as I hoped to do but still managed to watch all course material and finished the readability assignment.

Woops! Almost forgot to tell that I organized a (digitial) Kahoot! quiz for my university friends. That also took ±2 hours of my coding time. I don't care that much that I invested less time as I hoped to because it was nice to speak to my friends!

So today I learned to work with functions and arrays. While watching the lecture material I seemed to have some difficulties to understand the difference between passing arrays by reference vs passing other variables by value. Now I get the difference.

```c
int main(void)
{
	int a = 10;
	int b[4] = { 0, 1, 2, 3 };
	set_int(a); // Here I had difficulties. The problem was that I didn't see that the value was not assigned to a variable here (stupid me)
	set_array(b);
	printf("%d %d\n", a, b[0]);
}
```

Finally, the readability assignment. So it chopped it down in pieces. First, I figured out how to get the total number of uppercase or lowercase alphabetic characters in the text. Second, I puzzled a little bit to get the total number of words in the text. No difference in characters separated by a space. So, "sister-in-law" should be considered as one word, not three. Finally, I wrote a function to count the number of sentences by considering that any sequence of characters that ends with a . or a ! or a ? to be a sentence.

At the end I used all the functions that I build in my main function to calculate the Coleman-Liau index and determine the reading (grade) level. Here is my final submission:

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <ctype.h>

// Tell program to look down further for functions
int count_letters(string text);
int count_words(string text);
int count_sentences(string text);

// Main function
int main(void)
{
    string text = get_string("Text: ");

    // Initialize variables and call functions
    float letters = count_letters(text);
    float words = count_words(text);
    float sentences = count_sentences(text);

    // L is the average number of letters per 100 words in the text
    float L = (letters / words) * 100;
    // S is the average number of sentences per 100 words in the text
    float S = sentences / words * 100;

    // Calculate the Coleman-Liau index
    float index = 0.0588 * L - 0.296 * S - 15.8;
    // Round the index number to nearest whole number
    int grade = (int) round(index);

    // Conditional logic, if grade bigger than 16 then..
    if (grade >= 16)
    {
        printf("Grade 16+\n");
    }
    // Check if grade is smaller than 1
    else if (grade < 1)
    {
        printf("Before Grade 1\n");
    }
    // Else print the following
    else {
        printf("Grade %i\n", grade);
    }
}

// Function to count uppercase and lowercase alphabetic characters
int count_letters(string text)
{
    // Get the length of the text given by the user
    int characters = strlen(text);

    // Initialize letters to start counting
    int letters = 0;

    // For each character in text given by the user
    for (int i = 0; i < characters; i++)
    {
        // If character is alphabetic character, then...
        if (isalpha(text[i]))
        {
            // Add 1 to letters
            letters++;
        }
    }
    return letters;
}

int count_words(string text)
{
    // Get the length of the text given by the user
    int characters = strlen(text);

    // Initialize letters to start counting
    int words = 1;

    // For each character in text given by the user
    for (int i = 0; i < characters; i++)
    {
        // If character is alphabetic character, then...
        if (isblank(text[i]))
        {
            // Add 1 to letters
            words++;
        }
    }
    return words;
}

int count_sentences(string text)
{
    // Get the length of the text given by the user
    int characters = strlen(text);

    // Initialize letters to start counting
    int sentences = 0;

    // For each character in text given by the user
    for (int i = 0; i < characters; i++)
    {
        // If character is alphabetic character, then...
        if (text[i] == '.' || text[i] == '?' || text[i] == '!')
        {
            // Add 1 to letters
            sentences++;
        }
    }
    return sentences;
}
```
