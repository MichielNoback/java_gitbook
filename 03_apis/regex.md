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

