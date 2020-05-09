---
layout: post
author: Joris Veldt
tags: [CS50, programming, algorithmes]
---
The last couple of days I proceeded with the CS50 course. I watched the course material for week 2 and 3 and finished the "easier" assignments. On the one hand, I feel guilty for not finishing the "more difficult" assignments, but on the other hand, I like to move forward and I still have the possibility to finish them later. Honestly, we all know that finishing later means not doing it at all (in most cases). But let's focus on the better part: I made a lot of progress last days so let's elaborate on that!

With some work I finished all the week 2 assignments and noticed that programming involves a lot of structural thinking. I finished an alpha study and thinking in structures feels more like beta so that's new to me. This is part of my learning process. Taking a number and flowing through my code in my mind demonstrated that my computer is smarter and faster than I am. Debugging by logging to my console seems to work for me! This also reminds me that I really should learn to work with the debugger. Some assignments took me longer to solve than I originally was hoping for, but sticking to it and solving it on your own gives a good feeling.

So, I created a program called 'Caesar' that kind of encrypted messages. So, as a user I was prompted to fill in a number and use that number to encrypt the message. Then, I was asked to fill in the text that should be encrypted. For example, I gave 1 as a number and as text `Hello, World!`. This then would become `Ifnnp, Xpsne!`. Do you get? I increased each letter +1. If you don't get it that's fine too. For me it was a huge achievement at that time. 

Like any proud student, I shared this achievement during dinner at my girlfriends parents place. They were happy to see the joy in my face but questioned "What can you do with it". So I explained further and then they ask the question "Why would you use it?". Bummer... I probably should join some forums to share my joy and/or find friends who like to code as well. I think it keeps you more motivated to push it further.

Week 3 was about algorithms and damn... It has been a while that I thought or used mathematics. Although I now understand some algorithms used in Computer Science I still don't know how they are written in code. But I guess I will find out in a later stage. For future me I took some notes and put them down here. 

In CS are the big O and big Omega (Ω) notation used to explain the performance or complexity of an alogrithm. The difference between Big O and Big Ω is that Big O is used to describe the **worst case** running time for an algorithm. Big Ω is used to describe the **best case** running tiem for a given algorithm. Below is an image of most the main complexities (n log n is missing and is between O(n2) and O(n)).

![Complexity in Computer Science to understand runtimes]({{site.baseurl}}/assets/images/efficiency-algorithms-cs.png)
Source: [Medium](https://miro.medium.com/max/1400/1*yiyfZodqXNwMouC0-B0Wlg.png)

There are several algorithms that can be used to solve a problem. It depends on the occassion but I made some notes and created a nice table for it.

Algorithm Name | Basic Concept | O | Ω
--- | --- | --- | ---
Selction Sort | Find the **smallest** unsorted element in an array and swap it with the **first** unsorted element of that array | O(n2) | Ω(n2)
Bubble Sort | Swap **adjacent pairs** of elements if they are out of order, effectively "bubbling" larger elements to the right and smaller ones to the left | O(n2) | Ω(n)
Insertion Sort | Proceed once through the array from left-to-right, **shifting** elements as necessary to insert each element into its correct place | O(n2) | Ω(n)
Merge Sort | **Split** the full array into subarrays, then **merge** these subarrays back together in the correct order | O(n log n) | Ω(n log n)
Linear Search | **Iterate** across the array from left-to-right trying to find the target element | O(n) | Ω(1)
Binary Search | Giving a *sorted* array, **divide and conquer** by systematically eliminating half of the remaining elements in the search for target element | O(log n) | Ω(1)

In week 3 there were assignments on elections. The first assignment was about plurality and was pretty easy to solve. However, the assignment Runoff gave me more trouble to solve it and I must admit that after one day of puzzling and cursing I couldn't resist the temptation to crib on stackoverflow. Although I would have loved to solve the puzzle on my own I felt that this was a point where I could use some help. In the beginning of this point I was talking about thinking in structures and this was where my first mistake was.

There was a matrix array involved in this assignment and I was not able to update the counts of a specific candidate. Below I have added a table to explain it. The first array were the candidates where 0 represented Alice, 1 Bob and so on. The other rows are the votes. The first row shows that the voter first voted for Charlie, then Alice and finally Bob.

0 (Alice) | 1 (Bob) | 2 (Charlie)
--- | --- | ---
2 | 0 | 1
0 | 2 | 1
1 | 2 | 0
2 | 1 | 0
1 | 0 | 2

I was asked to create a function that tabulate votes for non-eliminated candidates. I figured out that I need two for-loops and that I somewhere needed to update the votes and then stop the loops. But assigning the votes to the right candidate was the problem. In particular, this part was where I struggled: `candidates[preferences[i][j]]`. I was not able to see that I could get the number of a specific cell and use that number to link it to a candidate. I was thinking about converting it to a string that represented the name. Way more difficult than the actual solution... This actually is a real life problem, we're overthinking stuff while the solution actually simple! 

The second part where I was struggling was the is_tie method. The method takes an int as parameter. I needed to return true if the election is tied between all candidates, false otherwise. Here I needed to swap the return values true and false. I ended with false because that was what I've seen so far. I think I stared too long to it to actually see that the solution was there all the time.

Here is my final solution:

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

// Max voters and candidates
#define MAX_VOTERS 100
#define MAX_CANDIDATES 9

// preferences[i][j] is jth preference for voter i
int preferences[MAX_VOTERS][MAX_CANDIDATES];

// Candidates have name, vote count, eliminated status
typedef struct
{
    string name;
    int votes;
    bool eliminated;
}
candidate;

// Array of candidates
candidate candidates[MAX_CANDIDATES];

// Numbers of voters and candidates
int voter_count;
int candidate_count;

// Function prototypes
bool vote(int voter, int rank, string name);
void tabulate(void);
bool print_winner(void);
int find_min(void);
bool is_tie(int min);
void eliminate(int min);

int main(int argc, string argv[])
{
    // Check for invalid usage
    if (argc < 2)
    {
        printf("Usage: runoff [candidate ...]\n");
        return 1;
    }

    // Populate array of candidates
    candidate_count = argc - 1;
    if (candidate_count > MAX_CANDIDATES)
    {
        printf("Maximum number of candidates is %i\n", MAX_CANDIDATES);
        return 2;
    }
    for (int i = 0; i < candidate_count; i++)
    {
        candidates[i].name = argv[i + 1];
        candidates[i].votes = 0;
        candidates[i].eliminated = false;
    }

    voter_count = get_int("Number of voters: ");
    if (voter_count > MAX_VOTERS)
    {
        printf("Maximum number of voters is %i\n", MAX_VOTERS);
        return 3;
    }

    // Keep querying for votes
    for (int i = 0; i < voter_count; i++)
    {

        // Query for each rank
        for (int j = 0; j < candidate_count; j++)
        {
            string name = get_string("Rank %i: ", j + 1);

            // Record vote, unless it's invalid
            if (!vote(i, j, name))
            {
                printf("Invalid vote.\n");
                return 4;
            }
        }
        printf("\n");
    }

    // Keep holding runoffs until winner exists
    while (true)
    {
        // Calculate votes given remaining candidates
        tabulate();

        // Check if election has been won
        bool won = print_winner();
        if (won)
        {
            break;
        }

        // Eliminate last-place candidates
        int min = find_min();
        bool tie = is_tie(min);

        // If tie, everyone wins
        if (tie)
        {
            for (int i = 0; i < candidate_count; i++)
            {
                if (!candidates[i].eliminated)
                {
                    printf("%s\n", candidates[i].name);
                }
            }
            break;
        }

        // Eliminate anyone with minimum number of votes
        eliminate(min);

        // Reset vote counts back to zero
        for (int i = 0; i < candidate_count; i++)
        {
            candidates[i].votes = 0;
        }
    }
    return 0;
}

// Record preference if vote is valid
bool vote(int voter, int rank, string name)
{
    // Look for a candidate called name
    for (int i = 0; i < candidate_count; i++)
    {
        // If candidate found, update preferences so that they are the voter's rank preference, and return true
        if (strcmp(name, candidates[i].name) == 0)
        {
            preferences[voter][rank] = i;
            return true;
        }
    }
    // If no candidate found, don't update any preferences and return false
    return false;
}

// Tabulate votes for non-eliminated candidates
void tabulate(void)
{
    // TODO
    for (int i = 0; i < voter_count; i++)
    {
        for (int j = 0; j < candidate_count; j++)
        {
            if(candidates[preferences[i][j]].eliminated == false)
            {
                candidates[preferences[i][j]].votes++;
                break;
            }
        }
    }
    return;
}

// Print the winner of the election, if there is one
bool print_winner(void)
{
    // If any candidate has more than half the vote, print out their name and return type
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes > (voter_count / 2))
        {
            printf("%s\n", candidates[i].name);
            return true;
        }
    }
    // if nobody has won the election yet, return false
    return false;
}

// Return the minimum number of votes any remaining candidate has
int find_min(void)
{
    // Return the minimum number of votes of anyone still in the election
    int min = candidates[0].votes;
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].eliminated == false)
        {
            if (candidates[i].votes < min)
            {
                min = candidates[i].votes;
            }
        }
    }
    return min;
}

// Return true if the election is tied between all candidates, false otherwise
bool is_tie(int min)
{
    // Accepts the minimum number of votes min as input
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].eliminated == false && candidates[i].votes != min)
        {
            return false;
        }
    }
    // Return true if the election is tied between all remaining candidates, and returns false otherwise
    return true;
}

// Eliminate the candidate (or candidiates) in last place
void eliminate(int min)
{
    // Eliminate anyone still in the race who has the min number of votes
    for (int i = 0; i < candidate_count; i++)
    {
        if (candidates[i].votes == min)
        {
            candidates[i].eliminated = true;
        }
    }
    return;
}
```

