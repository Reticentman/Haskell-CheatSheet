# Haskell

## Syntax

(This section needs work.)

`--`
: Inline comments

## The First Section: Haskell in Y Minutes

[Source](https://learnxinyminutes.com/docs/haskell/).

The first section consists of, what I thought to be most useful and most
important, along with some extra explanation.

## Small Stuff With Numbers

Division is not integer division by default :

```haskell
35 / 4 -- 8.75
35 `div` 4 -- integer division, returns 8
‘Cannot use single quotes for strings’ -- that doesn’t work
```

## Cool List stuffs

```haskell
[0,2..10] -- [0, 2, 4, 6, 8, 10] Can create steps
[1..10] !! 3 -- 4 (Lists start with 0)
```

You can also have infinite lists in Haskell:

```haskell
[1..] -- a list of all the natural numbers
```

Haskell only evaluates things when it needs to. If you ask for the 1000th
element:

```
[1..] !! 999 -- 1000 but haskell evaluated 1-1000 and the rest doesn’t exists yet.
```

Lists join as expected:

```
[1, 2] ++ [6, 7]
```

## More list Stuff

Adding to head of list:

```
0:[1..5] -- [0, 1, 2, 3, 4, 5]
```

Some list functions:

- head: `[1..5]` returns `1`
- tail: `[1..5]` returns `[2, 3, 4, 5]`
- init: `[1..5]` returns `[1, 2, 3, 4]`
- last: `[1..5]` returns `5`
- list comprehensions: `[2x | x <- [1..5]]` returns `[2, 4, 6, 8, 10]`
- with a conditional: `[2x | x <- [1..5], 2x > 4]` returns `[6, 8, 10]`

## Tuples Access

(i.e. a tuple of length 2):

```
fst ("haskell", 1)` -- "haskell"
snd ("haskell", 1)` -- 1
```

## Function Placement

A simple function that takes two variables: `add a b = a + b`

```
add 1 2 -- 3
1 `add` 2 -- 3
```

## Guards - For branching in functions

```
fib x
   | x < 2 = 1
   | otherwise = fib (x - 1) + fib (x - 2)
```

'|' can be read as 'when', for example: `fib` takes argument `x` and when `x` is
less then 2, return a value of 1.


## Pattern Matching

Pattern matching on lists. Here `x` is the first element in the list, and `xs`
is the rest of the list. We can write our own map function

```haskell
myMap func [] = [] -- This says 'myMap takes a function and applies it to a list then returns a List'
myMap func (x:xs) = func x:(myMap func xs)
-- As you can see here^ func x is being applied to the head of the func xs, then recurses into the rest of the list..
```



## Anonymous Functions

Create an anonymous function with a backslash:

```
myMap (\x -> x + 2) [1..4]``` -- [3, 4, 5, 6]
```

Using fold (called 'inject' in some languages) with an anonymous
function. foldl1 means fold left, and use the first value in the list as the
initial value for the accumulator:

```
foldl1 (\acc x -> acc + x) [1..5] -- 15
```

## Partial Application

This is a term to describe how haskell only applies what is passed. So if you
don’t pass in all the arguments to a function, it returns a function that takes
the rest of the arguments. For instance:

```
add a b = a + b
foo = add 10 -- foo is now a function that takes a number and adds 10 to it
foo 5 -- 15
```

Here, foo doesn’t show that it should be taking any arguments, but add
takes 2. So the 10 is supplemented in for the 2nd value of add.

## The Compose Operator: “.”

The '.' chains functions together, right-to-left.

For example, composing `(4*)` and `(10+)` means: take a number, add 10 to it,
then multiply it by 4.

```
foo = (4*) . (10+)
```

## The Apply Operator: "$"

Haskell has an operator called `$`. This operator applies a function to a given
parameter. In contrast to standard function application, which has highest
possible priority of 10 and is left-associative, the `$` operator has priority
of 0 and is right-associative. Such a low priority means that the expression on
its right is applied as the parameter to the function on its left. This allows
you to group together functions by splitting others with `$` like this:

```
even (fib 7) -- false
```

Equivalently:

```
even $ fib 7 -- false Here you can see how fib 7, is applied before being applied to even
```

## Type Signatures: Basics

```
5 :: Integer
"hello" :: String
True :: Bool not :: Bool -> Bool
add :: Integer -> Integer -> Integer
-- Add takes two arguments, then returns an integer
```

A little more on the `_` later, but using an underscore in the type signature
make it act like a wildcard, giving you a good hint at what the type signature
should be when your app fails with the error being the hint.

## If Expressions

```
haskell = if 1 == 1 then "awesome" else "awful”`
```

But it looks nicer on multiple lines!!

```
haskell = if 1 == 1
    then "awesome"
	else "awful”
```

## Case Expressions

Here's how you could parse command line arguments

```
case args of
    "help" -> printHelp
	"start" -> startProgram
	 _ -> putStrLn "bad args"
```

The underscore (`_ ->`) is to handle all other cases. You should keep it last,
since it technically always matches to the pattern, so anything below will be
disregarded and won’t be able to attempt to match.

## Recursion (Instead of loops)

```
map (*2) [1..5] -- [2, 4,6, 8, 10] here, map is used to apply a function to every element in the list 1-5.
```

you can make a for function using map:

```
for array func = map func array
-- and then use it:
for [0..5] $ \i -> show I
for  [0..5] show
```

Both return the same.

## Foldl and Foldr

```
foldl <fn> <initial value> <list>
foldl (\x y -> 2\*x + y) 4 [1,2,3] -- 43
```

This is the same as:

```
(2 \* (2 \* (2 \* 4 + 1) + 2) + 3)   -- foldl is left-handed, foldr is right-handed
foldr (\x y -> 2*x + y) 4 [1,2,3]    -- 16
(2 * 1 + (2 * 2 + (2 * 3 + 4)))
```

## Data

To create your own data type:

```
data Color
    = Red
	| Blue
	| Green
```

And to use it in a function:

```
say :: Color -> String
say Red   = “You are Red!”
say Blue  = “You are Blue!”
say Green = “You are Green!”
```

## Data types can have parameters too

```
data Maybe a = Nothing | Just a
```

These are all of type Maybe

```
Just "hello"` -- of type Maybe String
Just 1` -- of type Maybe Int
Nothing` -- of type Maybe a for any a
```


## HASKELL IO (Prepare yourself)


While IO can't be explained fully without explaining monads, it is not hard to
explain enough to get going. When a Haskell program is executed, 'main' is
called. It must return a value of type 'IO a' for some type 'a'. For
example:

```
main :: IO () main = putStrLn $ "Hello, sky! " \++ (say Blue)` -- putStrLn has type String -> IO ()
```

This means that `main` is what your app will run on! It is the engine and you
give it the power.

It is easiest to do IO if you can implement your program as a function from
String to String. The function `interact :: (String -> String) -> IO ()` inputs
some text, runs a function on it, and prints out the output.

```
countLines :: String -> String
countLines = show . length . lines

main' = interact countLines
```

You can think of a value of type `IO ()` as representing a sequence of actions
for the computer to do, much like a computer program written in an imperative
language. We can use the `do` notation to chain actions together. For example:

```
sayHello :: IO ()
sayHello = do
    putStrLn "What is your name?"
    name <- getLine -- this gets a line and gives it the name "name"
    putStrLn $ "Hello, " ++ name
```

## DO

You may have noticed the part after the = that is ‘do’. This allows you to put
your contents below, all together in a block, so your actions are chained
together.


## THE REPL!!

The Repl is a cool way to run some Haskell code and add any new values as
well. Type: ghci

From here you can check the types of any value. E.g.:

```
> :t
foo foo :: Integer
```
