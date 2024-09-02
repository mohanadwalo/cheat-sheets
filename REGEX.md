## Regex cheatsheet

### Meta characters
```
By default, it matches a single character unless we run regular expressions multiple times on the same input.
We can also use quantifiers or special characters to change the behavior of meta characters (we'll discuss that later).

\d Matches digits from 0 to 9

examples:

-   regex: \d
    input: 1
    result: matches 1
    reason: 1 is a digit

-   regex: \d
    input: a
    result: no match
    reason: a not a digit
 
\w Matches Latin letters (a to z or A to Z), underscore(_) or any digit(0 to 9)

examples:

-   regex: \w
    input: a
    result: match a
    reason: a is a Latin letter

-   regex: \w
    input: 9
    result: match 9
    reason: 9 is digit

-   regex: \w
    input: _
    result: match _
    reason: underscore is valid with \w regex

-   regex: \w
    input: @
    result: no match
    reason: @ is not Latin letter nor digit or underscore

\b Assert that what we are looking for is a full word(It is also called boundary)

examples:

-   regex: \bsun\b
    input: sun
    result: matches sun
    reason: the input word sun is a full word

-   regex: \bsun
    input: sunflower
    result: matches sun
    reason: sunflower includes the sun word at the beginning

-   regex: sun\b
    input: sunflower
    result: no match
    reason: regex needs sun word to be a boundary at the end

\s Matches any space

examples:

-   regex: \s
    input: Hello there
    result: matches a space
    reason: there is a space between Hello and there

-   regex: \s
    input: hellothere
    result: no match
    reason: no spaces in the input
```

### Assertions negation

This also searches for any single character that meets the rules below.  
```
\D Matches anything but not digit

examples:

-   regex: \D
    input: a
    result: matches a
    reason: a not a digit

-   regex: \D
    input: 1
    result: no matches
    reason: 1 is a digit

\W Matches anything but not Latin letters (a to z or A to Z), underscore(_) or any digit(0 to 9)

examples:

-   regex: \W
    input: @
    result: match
    reason: @ not a Latin letter or digit or underscore

-   regex: \W
    input: 9
    result: no match
    reason: 9 is a digit


\B Assert that a what we are looking for is included in another word , not at the beginning of another word or not at the end of another word

examples:

-   regex: \Bsun\B
    input: sun
    result: no match
    reason: sun is a full word and not a part of another word

-   regex: \Bsun
    input: sunflower
    result: no match
    reason: sun is part of another word but it is at the beginning of a word

-   regex: sun\B
    input: sunflower
    result: match sun
    reason: sun is part of another word and is not at the end of the sunflower

\S Assert none spaces characters

examples:

-   regex: \S
    input: Hello there
    result: first match is H, second match e and so on, it doesn't match the space between Hello and there

-   regex: \S
    input: hellothere
    result: there is 10 matches, all characters
    reason: there is no space
```

### Special characters
These special character modify the pattern comes before it.
To disable their function and match them as string literal, we need to add slash before them(Also called escape). Some programming languages needs  double slash.

```

+ Matches 1 or more characters from the patterns comes before it

examples:

-   regex: \d
    input: 123
    result: 1
    reason: matches a single digit

-   regex: \d+
    input: 123
    result: 123
    reason: + modifies the regex so it matches one or more digit

-   regex: \w+
    input: abc123
    result: abc123
    reason: matched one or more Latin letter or digit or underscore

? Match 0 or one

examples:

-   regex: \d?
    input: 123
    result: 1
    reason: matches single digit

-   regex: \d?
    input: abc
    result: first match a, second match b, third match c
    reason: matches 0 digit

* Match 0 or more(it is mix of + and ?)

examples:

-   regex: \d*
    input: 123
    result: 123
    reason: matches 0 or more digits

-   regex: \d*
    input: abc
    result: first match a, second match b, third match c
    reason: matches 0 digit

. Match any character except new line \n or tab \t

examples:

-   regex: .
    input: 123
    result: first match 1, second match 2, third match 3
    reason: . matches any character

-   regex: .+
    input: 123
    result: 123
    reason: match any character 1 or more times

-   regex: .*
    input: 123
    result: there are two matches, first match 123 and, second match no thing

-   regex: .+?
    input: 123
    result: first match 1, second match 2, third match 3
    reason: adding ? after + or * is called non-greedy or lazy, greedy means scanning the whole input to check if we can find bigger match
    
    support discussion:
    if you search for a pattern that starts with a and ends with b and accept any character in between
-   regex: a.+b
    input: a2bcdb
    result: a2bcdb
    reason: greedy

-   regex: a.+?b
    input: a2bcdb
    result: a2b
    reason: non-greedy, if there is a match in the beginning of the input, it doesn't scan the remaining input.

if the input is multiple lines then we need to use multiple lines flag.


^  Assert that the pattern comes after it is correct only if it is in the beginning of the input

examples:

-   regex: ^\d
    input: 1first text
    result: 1
    reason: sentence starts with digit

If the input is multiple lines, we must use the multiple lines flag to check if other lines also starts with the pattern.

-   regex: ^\d
    input: first text
    result: false no match
    reason: sentence does not start with digit

If the input is multiple lines, we must use the multiple lines flag to check if other lines also end with the pattern.

$ Assert that the pattern comes after it is correct only if it is in the end of the input

examples:

-   regex: \d$
    input: building no3
    result: 3
    reason: sentence ends with digit
      
-   regex: \d$
    input: last building
    result: no match
    reason: sentence does not end with digit
    Note: if multiple lines flag not used then it will try check only the last word in the text

Angle brackets <> , rounded brackets () and pipe | are considered to be special character only when it is used with [groups](#groups)

```

### Character class (aka OR regex)

It is square brackets []. It by default matches exactly one letter from the set between square brackets

```

example:

-   regex: [\d\s]
    input: building no3
    result: first match is a space, if we run the regex again second match is 3
    reason: Match either digit or space
    QA:
        Q: Why we don't use just \d\s
        A: \d\s means we search for any digit followed immediately by a space so with the previous example no match. [\d\s] matches any of the patterns inside the brackets if we found \d it is enough to say there is a match and the order doesn't matter
          
support example:
          
-   regex: [\d\s]
    input: 1result found
    result: first match is 1, second match is space

-   regex: [\d\s]
    input: found 1result
    result: first matche is space, second matche is 1

-   regex: [\d\s]
    input: 1result
    result: matches 1

-   regex: [\d\s]
    input: found results
    result: matches space

^ inside character class has a special meaning:
         
example:
         
-   regex: [^\d\s]
    input: hi 23
    result: first match h, second match i
    reason: ^ inside [] means anything except these patterns, in our case spaces and digits

```

### Escape Characters
```
Characters like .*+?^$[]\{} have a special meaning, if we want to match them literally we have to escape them by adding backslash before them.

examples:

-   regex: \d+
    input: 123
    result: matches 123

-   regex: \+
    input: +
    result: matches +

No need to escape special characters in character class except slash and hyphen

-   regex: [+]
    input: building + no3
    result: matches +

```

### Quantifiers
It is curly brackets {}. It allows us to specify how many characters we want to match.
It takes either one or two parameters, if we use one parameter, it means match exactly that number.
If we use two parameters{a,b}, it means the matches result count should be between a and be inclusive

Note: Using the quantifier with the character class requires that the matching result be consecutive.
```

examples:

-   regex: [\d\s]{2}
    input: building no3
    result: no match
    reason: There is a space and a number but they are not consecutive

-   regex: [\d\s]{2}
    input: building no 3
    result: matches  3
    reason: matches space and then digit, the total matches result is 2 and they are consecutive

-   regex: [\d\s]{2,3}
    input: building no 33
    result: matches  33
    reason: matches space and then two digits, the total match result is 3 and they are consecutive
    note:  3 is also valid(space and a digit), we we use two parameters, it tries to find the longest valid result(greedy)

-   regex: [\d\s]{3}
    input: building3 no3
    result: no match
    reason: there is two digits and a space but they are not consecutive

-   regex: [\d\s]{2,3}
    input: building3 no3
    result: matches 3 
    reason: there is a digit followed by a space
```
### Groups
If we know in advance the literal word or sentence we are looking for then we can use groups.
Groups use rounded brackets(), we can build a pattern of multiple groups or even nested groups.
Groups ordered from left to right from 1 to n and it is called capture group.

Groups maintain the order it appears in the pattern unless we use pipe | or we put them in character class[].
```

examples:

-   regex: (hello|world)
    input: hello world
    result: first match hello, second match world, if we run the regex only one time then it matches hello
    reason: we have one group with two variation
    note: we can add more groups as needed

We can use quantifiers with groups
   
-   regex: (hello|world){3}
    input: hello world
    result: no match
    reason: there is only two match but we need 3. In addition the two result are not consecutive

-   regex: [(hello|world)]{3}
    input: hellohelloworld
    result: hellohelloworld
    reason: there are 3 matches and consecutive

Groups rounded brackets lose meaning inside character class

examples:

-   regex: [(hello|world)]{5}
    input: (olleh hello world
    result: matches (olle, hello, world
    reason: The rounded brackets are considered literal characters so regular expressions will try to match any 5 consecutive 
    characters of "(hello|world)" and the order does not matter.


?<> can be used for named group

-   regex: (?<groupName>hello|world)
    input: hello world
    result: first match hello, second match world
    reason: matches first group variation hello or second group variation world
    note 1: we can use the group name that we included in the regex to get the matching result(this depends on the programming language regex implementation).
    note 2: we also can access the matching result using the group index, in our case group 1(this also depends on the programming language regex implementation).
 
(?:pattern) means don't track this group
Sometimes we need to ignore some nested groups

-   regex: ((firstName)\\s(middleName)\\s(lastName))
    input: firstName middleName lastName
    result: firstName middleName lastName
    reason: it searches for three literal words separated by a space
    note: groups are ordered from left to right from 0 to n in the same order it appears in the pattern including nested groups
       
In the above pattern we have 4 groups ordered from left to right
    group 1 () the outer brackets
    group 2 (firstName)
    group 3 (middleName)
    group 4 (lastName)
  
Since we are interested only about the full name so we can ignore nested groups.

-   regex: ((?:firstName)\\s(?:middleName)\\s(?:lastName))
    note 1: tracking groups is expensive because it slows down performance.
    note 2: none tracked groups are not counted

Now we have only one group, the outer group(group number 1) and we don't have group number 2,3,4 anymore
See how the group numbers starts left to right, counting nested groups as well.

(pattern(?=pattern)) look a head
It searches for patterns with looking at what comes immediately after it

-   regex: (fat(?=\scat))
    input: there is a fat cat in the street
    result: matches fat cat
    reason: there is a word fat followed by space and then the word cat

-   regex: (fat(?=\scat))
    input: there is a fat white cat in the street
    result: no match
    reason: there is a word fat followed not immediately by a space and the word cat

-   regex: (fat(?=\scat))
    input: there is a fat dog in the street
    result: no match
    reason: there is a word fat but not followed by a space and then the word cat

?! look a head negation
It searches for patterns and make sure that a specific pattern doesn't come immediately after it


-   regex: (fat(?!\scat))
    input: there is a fat cat in the street
    result: no match
    reason: there is a word fat followed by a space and the word cat

-   regex: (fat(?!\scat))
    input: there is a fat dog in the street
    result: matches fat dog
    reason: there is a word fat and not followed by a space and the word cat
   


?<= look behind
very similar to the look a head but it checks what comes before the pattern
  
-   regex: ((?<=fat\s)cat)
    input: there is a fat cat in the street
    result: matches fat cat
    reason: there is a word cat preceded by fat

-   regex: ((?<=fat\s)cat)
    input: there is a fat white cat in the street
    result: no match
    reason: there is a word cat preceded not immediately by cat

-   regex: ((?<=fat\s)cat)
    input: there is a fat dog in the street
    result: no match
    reason: there is a word fat but not preceded by cat

?<! look behind negation

It searches for patterns and make sure that a specific pattern doesn't come immediately before it

-   regex: (fat(?!\scat))
    input: there is a fat cat in the street
    result: no match
    reason: there is a word fat preceded by cat

-   regex: (fat(?!\scat))
    input: there is a fat dog in the street
    result: matches fat dog
    reason: there is a word fat and not preceded by cat


Note: $ can be used with replace method to replace groups with different pattern
-   regex: (\d{4})(\d{2})(\d{2})
    input: 20240510
    action: input.replace(regex,"$1/$2/$3")
    result: 2024/05/10
```

### Note
Some programming languages provide two implementations:
First: If you run the same regex on the same inputs again and again the regex will start search from the previous match, it saves the state.

second: It always return the first match regardless of how many time you run the same regex on the same input.

#### See also

Effective Learning Method [here](https://github.com/mohanadwalo/effective-learning-methods)  
Cheat sheets [here](https://github.com/mohanadwalo/cheat-sheets)  
Learning a new language effectively [here](https://github.com/mohanadwalo/learning-a-new-language)  
Learn Java Through Practical Use Cases [here](https://github.com/mohanadwalo/java-use-cases)  
