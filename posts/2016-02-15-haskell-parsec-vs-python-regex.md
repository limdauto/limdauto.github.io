---
title: Parser Combinator in Haskell vs. Regex in Python
tags: haskell, python, parsec, regex
---
This isn't meant to be a comprehensive comparison between the two styles of parsing, because parser combinator is certainly more powerful than plain regular expression, athough I have seen regular expressions that pretend to do just as much. Speaking of which, there is a [recent story on Hacker News about an adventure in regex land which I highly recommend](http://davidvgalbraith.com/how-i-fixed-atom/). The bad news is parser combinator isn't part of the standard library in Python. While there are great open source projects in the area such as [PLY](http://www.dabeaz.com/ply/index.html) or [PyParsing](http://pyparsing.wikispaces.com/), it's tedious to set them up for simple use cases, especially when the regex interface in Python is pretty great. On the other hand, it's stupidly simple to use Parsec in Haskell, while I haven't been able to use Regular Expression in this language beyond some very simple matching (noob alert, duh!). So let's see how they look like when attempting to solve the same (simple) problem. We are going to try to parse a time string in the 12-hour format `(hh[sep]mm[sep]ss[period])`, e.g. "11:00:00AM" into a data structure that holds the hour, minute, second and period (AM, PM) without the separator, e.g. colon.

#### Python implementation with Regex

```lang-python
# using a namedtuple to represent the time data parsed from string
from collections import namedtuple
Time = namedtuple('Time', ['hh', 'mm', 'ss', 'period'])

# parsing with regex
time_digits = "\d+"
periods = "AM|PM"
separators = "[:/-]"
pattern = re.compile(r"({digits}){sep}({digits}){sep}({digits})({periods})".format(
    digits=time_digits, sep=separators, periods=periods))
# or an one-liner: pattern = re.compile(r"(\d+)[:/-](\d+)[:/-](\d+)(AM|PM)")
timeParser = lambda input: Time(*pattern.match(input).groups())
```

#### Haskell implementation with Parsec

```lang-haskell
import Text.ParserCombinators.Parsec
import Control.Monad

data Time = Time Int Int Int String

timeParser = Time <$> (digits <* sep) <*> (digits <* sep) <*> digits <*> period
    where digits    = liftM read $ many1 digit
          period    = choice [string "AM", string "PM"]
          sep       = oneOf ":/-"
```

I think both solutions are very elegant in their own right that showcase some of the best features outside of parsing from both languages (arguments unpacking in Python and applicative combination in Haskell). However, as complexity grows, one can imagine how the Parser solution will be more flexible than regular expression, which will be the topic for another day.

**Disclaimer**: As I am still new to Haskell, I have to ask how to do applicative combination in this case on [StackOverflow](http://stackoverflow.com/questions/35400891/simplify-do-notation/35401824#35401824)
