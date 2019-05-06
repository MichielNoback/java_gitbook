# Design rules

## equals() and hashCode()

- Always implement equals() and hashCode(), when objects are going to live in collections (which is quite often!)
- Always implement both equals and hashCode; never only one!
- And have your IDE generate it for you...

## Code against interfaces, not implementations

Use interfaces and abstract classes as the declared type of your variables. That way, implementation details may change without serious repercussions.

## Exceptions

- Never let exceptions pass silently
- Deal with exceptions at an appropriate location
- Nowadays, the trend is to prefer unchecked exceptions over checked exceptions (see discussion at http://www.yegor256.com/2015/07/28/checked-vs-unchecked-exceptions.html)
- Never ever try to catch an Error


## SRP



