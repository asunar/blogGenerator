---
title: Learning from failure
date: 2021-10-05 09:01:43
tags: interview
---

I have failed a coding challenge some time ago. It looked like a simple enough problem. However, afer struggling  to come up with an answer in the allowed time, I blew it.

As a software professional with about two decades of progressive experience I was disappointed. I decided to do a mini retrospective to see what I can learn from this experience; what went wrong and how I can do better next time. I skimmed through the "Cracking the Coding Interview" book in preparation for the interview. However, I still made the most common interviewing mistake. I jumped into coding too soon without formulating a solution in my head first.

The task looked pretty simple; write a function that converts an integer to roman numeral. I thought I had a pretty good understanding of the problem and that I would be able to come up with an algorithm on the spot. I was unable to find a pattern to code.

I remembered the advice to practice solving coding challenges on paper for practice and decided to give it a try. I have had good luck with what I call "table approach" where you put all the relevant variables in a table and track their values for each iteration.

Started with writing down the rules:
* M: 1000, D: 500, C: 100, L: 50, X: 10, I: 1
* Symbols to the right increment, symbols to the left decrement
* Max 3 consecutive roman numerals are allowed
* Maximum number representable in roman numerals is 3999
* 0 has no equivalent symbol

The last three rules were not immediately obvious before writing them down. It's easy to see the early exit conditions. If the number is 0 or greater than 3999, there is nothing to do.

Here is a table that shows the roman numerals to represent numbers 1-9 in different places.


|   | 1000s | 100s | 10s                                                      | 1s                             |
|:-:|:--------------:|:--------------:|--------------------------------------------------------------|-------------------------------------|
| 1 |        M       |        C       | X    | I                     |
| 2 |        MM       |        CC       | XX | II          |
| 3 |        MMM       |        CCC      | XXX    | III |
| 4 |        -       |        CD     | XL                        | IV        
| 5 |        -       |        D     | L                        | V        
| 6 |        -       |        DC     | LX                        | VI        
| 7 |        -       |        DCC     | LXX                        | VII        
| 8 |        -       |        DCCC     | LXXX                        | VIII        
| 9 |        -       |        CM     | XC                        | IX        

Here is the pattern I see:

|   | Pattern | 
|:-:|:--------------:|
| 1 |        a       |
| 2 |        aa       |
| 3 |        aaa       |
| 4 |        ab       |
| 5 |        b      | 
| 6 |        ba       |
| 7 |        baa      |
| 8 |        baaa     |
| 9 |        ac       |

Let's code this pattern in javascript:
```javascript
function convertDigit(c, p, gp, digitNumber){
  // c: current numeral
  // p: parent numeral
  // gp: grandParent numeral
  let digitNumberRomanNumeralTransform = new Map([
    [1, (c) => c],
    [2, (c) => c + c],
    [3, (c) => c + c + c],
    [4, (c, p, _) => c + p],
    [5, (_, p) => p],
    [6, (c, p) => p + c],
    [7, (c, p) => p + c + c],
    [8, (c, p) => p + c + c + c],
    [9, (c, _, gp) => c + gp]
  ])

  const getTransformFunc = digitNumberRomanNumeralTransform.get(digitNumber)
  const result = getTransformFunc(c, p, gp)
  return result
}	
```

Every digit in an integer can be represented by 3 roman numerals:
* 1s: I, V, X 
* 10s: X, L, C
* 100s: C, D, M

Sounds like a simple Map.

```javascript
function getDigitRomanNumeralsMap(){
  let digitRomanNumeralsMap = new Map([
  [1, { "numeral": "I", "parentNumeral": "V", "grandParentNumeral": "X" }],
  [10, { "numeral": "X", "parentNumeral": "L", "grandParentNumeral": "C" }],
  [100, { "numeral": "C", "parentNumeral": "D", "grandParentNumeral": "M" }],
])

  return digitRomanNumeralsMap
}
```

Now I can:
* Iterate over each digit
* Pick the 3 roman numerals for the digit place
* Convert each digit to roman numeral
* Concatenate roman numerals

See the code in action [here](https://replit.com/@asunar/Roman-Numerals).

Retro time... What have I learned from this experience?
* Read the entire question, ask for clarification and make assumptions explicit 
* Read the entire question, ask for clarification and make assumptions explicit (not a typo) 
* Do not skip the steps above regardless of how easy the question might seem
* Avoid multitasking, formulate a solution *and then* write the code
* Draw a table to visualize the available data
* Try to detect a pattern
* Optionally write some pseudocode to mentally test the pattern
* Write code to implement the pattern/algorithm

You're welcome, future self.








