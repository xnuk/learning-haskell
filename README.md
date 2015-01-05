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
