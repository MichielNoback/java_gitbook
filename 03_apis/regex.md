# Regular Expressions

Often, when your data is well-structured, a `String.split()` with a simple separator is all you need to process your data,maybe with some conversion methods for creating ints and doubles and such from strings. However, there are cases when you need more: finding restriction enzyme sites, parsing zip codes in addresses for instance.

This part only introduces the Java regex API
It is NOT a tutorial on regex!
(PS, I borrowed some examples from http://www.vogella.com)

Regular expressions are used to search for patterns in text
For instance, if you want to find occurrences of Dutch zip codes, you could use this: `[a-zA-Z]{4} ?[0-9]{2}`


## What is a regex?

A regular expression is used to describe a pattern that is not literal - different distinct character strings could match the pattern. 

## Backslashes

Backslashes have meanings both in String literals and in regex. They denote special characters, but also negate the special meaning
Thus, to match a literal backslash, your regex will be `\\\\` !! 

## Character classes

You can specify groups of characters that are all equally valid to match a position using character classes. They can be specified in many ways.

| Character class | Description                                                                                                        |
|-----------------|--------------------------------------------------------------------------------------------------------------------|
| .               | Any character                                                                                                      |
| []              | Set definition, eg [a-z] matches all lowercase characters;  [aAbB1234] matches an a, A, b, B or numbers 1, 2 ,3, 4 |
| [&#94;]             | Negated set definition, eg [&#94;a-z] matches anything BUT lowercase characters                                        |
| X&#124;Z             | Matches X or Z (either one will suffice)                                                                           |
| &#94;regex          | Anchors regex at beginning of line                                                                                 |
| regex$          | Anchors regex at end of line                                                                                       |
| \d \D           | Any digit, [0-9];  any non-digit, [&#94;0-9]                                                                           |
| \s \S           | Any whitespace character;  any non-whitespace character                                                            |
| \w \W           | A word character, short for [a-zA-Z_0-9];  same but negated                                                        |
| \b              | Word boundary                                                                                                      |

## Regex quantifiers

Quantifiers let you modify how often a group or character is allowed.

| Quantifier | Description                                                                                              |
|------------|----------------------------------------------------------------------------------------------------------|
| {x}        | Occurs exactly x times                                                                                   |
| {x, }      | Occurs at least x times                                                                                  |
| {, x}      | Occurs at the most x times                                                                               |
| *          | Occurs zero or more times; same as {0, }                                                                 |
| +          | Occurs one or more times; same as {1, }                                                                  |
| ?          | Occurs zero or one time; same as {0, 1}                                                                  |
| *?         | Non-greedy: "?" after a quantifier makes it a reluctant quantifier, it tries to find the smallest match |


## Grouping

Using parentheses, you can group parts of your regex. 
They can be used to
- Retrieve or substitute parts of a regex
- Apply quantifiers to groups

Via the `$` you can refer to a group. `$1` is the first group, `$2` the second, etc (see examples).

## String, Pattern & Matcher
For Java regex, these classes are related to regular expression matching (and replacement):  

- `java.lang.String` has several useful methods working with regexes
    - d
- java.util.regex.Pattern
- java.util.regex.Matcher

Many of the common tasks can be performed using the String class only. Here are some examples.

```java
String input = "Dogs rule this doggin' world";
//replace() works with literal string!
System.out.println(input.replace("[Dd]og", "Cat"));
System.out.println(input.replace("Dog", "Cat"));
//replaceAll() works with regex!
System.out.println(input.replaceAll("[Dd]og", "Cat"));
System.out.println(Arrays.toString(input.split("[Dd]")));
//matches() looks at whole target string.
System.out.println(input.matches("[Dd]ogs"));
System.out.println(input.matches("^[Dd]ogs.+"));
```

outputs 


<pre style="color:white;background-color:black;font-weight:bold;font: 1.3rem Inconsolata, monospace;">
Dogs rule this doggin' world
Cats rule this doggin' world
Cats rule this Catgin' world
[, ogs rule this , oggin' worl]
false
true
</pre>

## Volgende
