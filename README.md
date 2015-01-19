Intro & LICENSE
===============
이 내용은 [learnyouahaskell.com](http://learnyouahaskell.com/)(©Miran Lipovača)에서 하스켈을 공부하면서 적은 요약 노트이며, 이 저작물과 원문은 [Creative Commons Attribution-Noncommercial-Share Alike 3.0 Unported License (Creative Commons 저작자표시-비영리-동일조건변경허락 3.0 Unported) (CC BY-NC-SA 3.0)](http://creativecommons.org/licenses/by-nc-sa/3.0/)로 이용하실 수 있습니다. Pull Request 및 Fork 시 이 라이선스에 동의한 것으로 간주합니다.

기여자: [@xnuk](https://github.com/xnuk)

Starting out
============
Operators
---------
- Booleans: True, False

- Logical: not, &&, ||
- Compare: `==`, `/=`(not equal), `<`, `<=`, `>`, `>=`
 - List끼리는 사전 순서로 비교
- `[List] ++ [List]`: Array::concat
- `T : [List<T>]`: Array::unshift
 - `[1,2,3] == 1:2:3:[]`
- `[List] !! index`: 리스트의 index번째 원소
- Texas ranges `..`
 - `[1..5] == [1,2,3,4,5]`
 - `[1,3...8] == [1,3,5,7]`
 - floating point에서는 쓰지 말 것.

Conditions
----------
- __if__ condition __then__ statement1 __else__ statement2

Comments
--------
```haskell
-- This is a single line comment

{- 
   This is 
   multiline
   comment
-}
```

Functions
---------
- precedence: `+`, `-` < `*`, `/` < __Functions__
- `succ x = x+1`
- `min x y`, `max x y`
- `mod x y`
- `odd x`, `even x`

### List
- `head`: 맨 앞의 원소
- `last`: 맨 뒤의 원소
- `tail`: 맨 앞을 제외한 리스트
- `init`: 맨 뒤를 제외한 리스트
- `length`
- `null`: 리스트가 비었는지 여부
- `reverse`: 역순 리스트
- `take i [List]`: 맨 앞에서부터 `i`개의 리스트
- `drop i [List]`: 맨 앞에서부터 `i`개를 버린 리스트
- `maximum`, `minimum`
- `sum`, `product`
- `elem x [List]`: x가 리스트에 들어있는지 여부


- `cycle [List]`: 리스트를 무한정 붙여 뿜음 (`take 10 (cycle [1,2,3]) == [1,2,3,1,2,3,1,2,3,1]`)
- `repeat x`: 원소 x만으로 구성된 무한 리스트
- `replicate x y = take x (repeat y)`

### Tuple - 원소가 두 개일 때만 적용되는 함수:
- `fst`: 첫 번째 원소
- `snd`: 두 번째 원소
- `zip [List] [List]` 가능할 때까지 두 리스트의 각각의 원소들을 엮은 Tuple의 리스트
```haskell
zip [5,3,2,6,2,7,2,5,4,6,6] ["im","a","turtle"] -- [(5,"im"),(3,"a"),(2,"turtle")]
```

List Comprehension
------------------
`[출력 | 변수 <- [범위], 조건식, 등등]`
```haskell
[x*2 | x <- [1..10]] -- [2..20]
[x*2 | x <- [1..10], x*2 >= 12] -- [12..20]
let boomBangs xs = [ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x]
boomBangs [7..13] -- ["BOOM!","BOOM!","BANG!","BANG!"]
```

Tuple
-----
List는 같은 형식만 박을 수 있지만 Tuple은 다른 형식도 박을 수 있음.
단 원소 내의 형식 순서와 원소 개수가 같아야 같은 타입의 Tuple이라고 인정.
```haskell
[(1,2),(8,11,5),(4,5)] -- invalid
[(1,2),("One",2)] -- invalid
("Christopher", "Walken", 55) -- triple
("Christopher", "Walken", 55, []) -- 4-tuple
```

Types and Typeclasses
=====================
Types
-----
- Int: 32bit, 64bit OS에 따라 최댓값, 최솟값이 다름. signed.
- Integer: 제한 없음. >>> *I mean like really big.* <<<
- Float
- Double
- Bool
- Char

함수 형식
--------

### with static types
```haskell
functionName :: Type -> Type
functionName -- function definition here

removeNonUppercase :: [Char] -> [Char]
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]

addThree :: Int -> Int -> Int -> Int
addThree x y z = x + y + z
```

### with type *variables*
```haskell
ghci> :t head
head :: [a] -> a

ghci> :t fst
fst :: (a, b) -> a
```

### with typeclasses
```haskell
ghci> :t (==)  
(==) :: (Eq a) => a -> a -> Bool -- The type of those two values must be a member of the Eq class
```
- `=>`: class constraint
- `Eq` 타입클래스: All standard Haskell types except for IO and functions are a part of the `Eq` typeclass.

Typeclass
---------
- Eq: 함수는 `==`랑 `/=`. 동일성 비교.
- Ord: order. `>`, `<`, `>=`, `<=` 같은 함수. 멤버이기 위해선 `Eq` 클럽의 일류 회원이어야 함<?
 - `compare` 함수: 같은 타입의 `Ord` 멤버 두 개를 가져와 ordering을 반환.
 - `Ordering` 타입: `GT`(Greater than), `LT`(Lesser than), `EQ`(EQual)
- Show: can be presented as strings. 주로 쓰이는 함수에는 `show`가 있음. 일단 문자열로 바꾸고 보는 함수. (->[Char])
- Read: `Show`의 반대. 함수에는 주로 `read`가 있음. 일단 문자열을 파싱하는 함수. ([Char]->)
 - 주: `read`를 사용 시 암시적 또는 명시적으로 타입을 알아채게 해야 함.
```haskell
read "True" || False -- True
read "8.2" + 3.8 -- 12.0
read "5" - 2 -- 3
read "[1,2,3,4]" ++ [3] -- [1,2,3,4,3]
read "4" -- >>> Error <<<
read "4" :: Int -- 4
read "(3, 'a')" :: (Int, Char) -- (3, 'a')
```
- Enum: 연속적으로 열거 가능한 정렬된 타입 클래스. list range에서 쓸 수 있음. 함수는 `succ`(값에 1을 더함), `pred`(값에서 1을 뺌)
 - `()`, `Bool`, `Char`, `Ordering`, `Int`, `Integer`, `Float`, `Double`.
- Bounded: 최대, 최소 경계. `minBound`, `maxBound` (`:: Bounded a => a`)
```haskell
ghci> minBound::Char
'\NUL'
ghci> minBound::Int
-9223372036854775808
ghci> minBound::Bool
False
ghci> maxBound::Bool
True
ghci> maxBound :: (Bool, Int, Char)
(True,9223372036854775807,'\1114111')
```
- Num: 숫자 타입 클래스. `Int`, `Integer`, `Float`, `Double` / 사칙 연산, 등등
```haskell
ghci> :t (*)
(*) :: (Num a) => a -> a -> a

(5 :: Int) * (6 :: Integer) -- Error
5 * (6 :: Integer) -- 30
```
- Floating: `Float`, `Double`밖에 없음.
- Integral: `Int`, `Integer`밖에 없음.
 - `fromIntegral` 함수: `Integral`을 `Num`으로 바꿔줌.
```haskell
fromIntegral :: (Num b, Integral a) => a -> b

fromIntegral (length [1,2,3,4]) + 3.2 -- 7.2
```

Syntax in Functions
===================
Pattern matching
----------------
- `(함수명) ((값/형식/인자)..) = (expression)`
- 맨 위부터 아래로 가면서 경우를 검색하며 매칭되는 경우가 없으면 에러를 뿜음.
- Pattern matching으로 쪼갤 때 `:`는 사용 가능하나 `++`는 사용 가능하지 않음.

```haskell
sayMe :: (Integral a) => a -> String  
sayMe 1 = "One!"
sayMe 2 = "Two!"
sayMe 3 = "Three!"
sayMe 4 = "Four!"
sayMe 5 = "Five!"
sayMe x = "Not between 1 and 5"

factorial :: (Integral a) => a -> a
factorial 0 = 1
factorial n = n * factorial (n - 1)

charName :: Char -> String
charName 'a' = "Albert"
charName 'b' = "Broseph"
charName 'c' = "Cecil"

addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)
addVectors (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)

tell :: (Show a) => [a] -> String
tell [] = "The list is empty"
tell (x:[]) = "The list has one element: " ++ show x
tell (x:y:[]) = "The list has two elements: " ++ show x ++ " and " ++ show y
tell (x:y:_) = "This list is long. The first two elements are: " ++ show x ++ " and " ++ show y

length' :: (Num b) => [a] -> b
length' [] = 0
length' (_:xs) = 1 + length' xs

capital :: String -> String
capital "" = "Empty string, whoops!"
capital all@(x:xs) = "The first letter of " ++ all ++ " is " ++ [x] -- all == (x:xs)
```

Guards
------
```haskell
{-
(함수명) ((값/형식/인자)..)
    | (Bool) = (expression)
    | otherwise = (expression)
	where (지역변수) = (expression)
-}

-- 다음 세 코드는 서로 같음
-- ---------------------------------------------------------------------------
bmiTell :: (RealFloat a) => a -> a -> String
bmiTell weight height
    | weight / height ^ 2 <= 18.5 = "You're underweight, you emo, you!"
    | weight / height ^ 2 <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"
    | weight / height ^ 2 <= 30.0 = "You're fat! Lose some weight, fatty!"
    | otherwise                 = "You're a whale, congratulations!"

bmiTell :: (RealFloat a) => a -> a -> String
bmiTell weight height
    | bmi <= 18.5 = "You're underweight, you emo, you!"
    | bmi <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"
    | bmi <= 30.0 = "You're fat! Lose some weight, fatty!"
    | otherwise   = "You're a whale, congratulations!"
    where bmi = weight / height ^ 2

bmiTell :: (RealFloat a) => a -> a -> String
bmiTell weight height
    | bmi <= skinny = "You're underweight, you emo, you!"
    | bmi <= normal = "You're supposedly normal. Pffft, I bet you're ugly!"
    | bmi <= fat    = "You're fat! Lose some weight, fatty!"
    | otherwise     = "You're a whale, congratulations!"
    where bmi = weight / height ^ 2
          skinny = 18.5
          normal = 25.0
          fat = 30.0
{-
    where bmi = weight / height ^ 2  
      (skinny, normal, fat) = (18.5, 25.0, 30.0)
-}
-- ---------------------------------------------------------------------------

myCompare :: (Ord a) => a -> a -> Ordering
a `myCompare` b
    | a > b     = GT
    | a == b    = EQ
    | otherwise = LT

max' :: (Ord a) => a -> a -> a
max' a b | a > b = a | otherwise = b
```

Let, Where
---
`let <bindings> in <expression>`
```haskell
4 * (let a = 9 in a + 1) + 2 -- 42
(let a = 100; b = 200; c = 300 in a*b*c, let foo = "Hey "; bar = "there!" in foo ++ bar) -- (6000000, "Hey there!")

calcBmis :: (RealFloat a) => [(a, a)] -> [a]
calcBmis xs = [bmi | (w, h) <- xs, let bmi = w / h ^ 2]

initials :: String -> String -> String
initials firstname lastname = [f] ++ ". " ++ [l] ++ "."
    where (f:_) = firstname
          (l:_) = lastname

calcBmis :: (RealFloat a) => [(a, a)] -> [a]
calcBmis xs = [bmi w h | (w, h) <- xs]
    where bmi weight height = weight / height ^ 2
```

- `let`은 expression이고 `where`는 문법 구조.

Case expressions
----------------
```haskell
case expression of pattern -> result
                   pattern -> result
                   pattern -> result
                   ...

describeList :: [a] -> String  
describeList xs = "The list is " ++ case xs of [] -> "empty."  
                                               [x] -> "a singleton list."
                                               xs -> "a longer list."

describeList :: [a] -> String
describeList xs = "The list is " ++ what xs
    where what [] = "empty."
          what [x] = "a singleton list."
          what xs = "a longer list."
```
