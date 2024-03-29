# Advent of Code 2022 Walkthrough

## Table of Contents
* [Day 01 - Calorie Counting](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-01---calorie-counting)
* [Day 02 - Rock, Paper, Scissors](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-02---rock-paper-scissors)
* [Day 03 - Rucksack Reorganization](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-03---rucksack-reorganization)
* [Day 04 - Camp Cleanup](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-04---camp-cleanup)
* [Day 05 - Supply Stacks](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-05---supply-stacks)
* [Day 06 - Tuning Trouble](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-06---tuning-trouble)
* [Day 07 - No Space Left On Device](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-07---no-space-left-on-device)
* [Day 08 - Treetop Tree House](https://github.com/noah-kg/AdventOfCode/blob/main/2022/README.md#day-08---treetop-tree-house)

## Day 01 - Calorie Counting
[Problem](https://adventofcode.com/2022/day/1) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_01_Calorie_Counting.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
We are given a list of integers representing the amount of calories for each snack that each elf is carrying. Each elf's list of snacks is separated by a new line. We need to parse this list, separate it into "chunks" (one chunk would be a single elf's snacks), and return the value of the elf with the most calories.

First, we need to parse the input into chunks (each chunk represents a singluar elf) - which can be done with the [split()](https://docs.python.org/3/library/stdtypes.html#str.split) method. Then we use [list comprehension](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) and the [map()](https://docs.python.org/3/library/functions.html#map) function to convert each item in a chunk to an integer:

```python
chunks = fin.read().split('\n\n')
chunks = [tuple(map(int, chunk.split())) for chunk in chunks]
```

Then it's just a matter of returning the biggest number by using the [max()](https://docs.python.org/3/library/functions.html#max) function:

```python
most = max(map(sum, chunks))
```

### Part 2
For part two we are told that instead of getting the elf with the most calories, we need the top 3 elves with the most calories. We can easily achieve this using the [sort()](https://docs.python.org/3/library/stdtypes.html#list.sort) method. We use ```key=sum``` so that the list of chunks is sorted based on the sum of their values. We also need to sort in descending order, so we set ```reverse=True```.

```python
chunks.sort(key=sum, reverse=True)
top3 = sum(map(sum, chunks[:3]))
```

## Day 02 - Rock, Paper, Scissors
[Problem](https://adventofcode.com/2022/day/2) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_02_Rock_Paper_Scissors.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
We are given a list of strings depicting rounds of the famous game Rock, Paper, Scissors. Each string is a combination of two letters: ```ABC``` for player one, and ```XYZ``` for player two (us). We don't quite know how to decode the list, but we do know the following: ```A = Rock```, ```B = Paper```, and ```C = Scissors```. Our initial assumption is to assume that ```XYZ``` is what we must play in order to win the round. There are 9 total combinations: ```AX```, ```AY```, ```AZ```, ```BX```, ..., ```CZ```, etc. Each combination has a unique score attached to it. The score is calculated based on two things: the shape we picked (the ```XYZ```) and the outcome of the round (win/lose/draw). A loss counts as ```0 points```, a draw counts as ```3 points```, and a win counts as ```6 points```. We can create a simple dictionary that has 9 keys representing the possible outcomes, with 9 values representing the scores.

```python
scores1 = {
    'AX': 4, #1 + 3 # A (rock)     vs X (rock)     -> draw
    'AY': 8, #2 + 6 # A (rock)     vs Y (paper)    -> win
    'AZ': 3, #3 + 0 # A (rock)     vs Z (scissors) -> loss
    'BX': 1, #1 + 0 # B (paper)    vs X (rock)     -> loss
    'BY': 5, #2 + 3 # B (paper)    vs Y (paper)    -> draw
    'BZ': 9, #3 + 6 # B (paper)    vs Z (scissors) -> win
    'CX': 7, #1 + 6 # C (scissors) vs X (rock)     -> win
    'CY': 2, #2 + 0 # C (scissors) vs Y (paper)    -> loss
    'CZ': 6 #3 + 3  # C (scissors) vs Z (scissors) -> draw
}
```

We can then loop through the list and add up each rounds respective scores to find the answer to part 1.

```python
ans1 = 0
for line in lines:
    a, b = line.strip().split()
    ans1 += scores1[a+b]
```

### Part 2
We are now informed that our initial assumption was incorrect, and that instead of ```XYZ``` needing to be the winning move, it now represents the desired outcome. ```X = lose```, ```Y = draw```, and ```Z = win```. We simply create a second dictionary with the updated scores, and we can calculate the answer to part 2 in the same loop used for part 1!

```python
scores2 = {
    'AX': 3, #3 + 0  A (rock)     vs X (scissors) -> loss
    'AY': 4, #1 + 3  A (rock)     vs Y (rock)     -> draw
    'AZ': 8, #2 + 6  A (rock)     vs Z (paper)    -> win
    'BX': 1, #1 + 0  B (paper)    vs X (rock)     -> loss
    'BY': 5, #2 + 3  B (paper)    vs Y (paper)    -> draw
    'BZ': 9, #3 + 6  B (paper)    vs Z (scissors) -> win
    'CX': 2, #2 + 0  C (scissors) vs X (paper)    -> loss
    'CY': 6, #3 + 3  C (scissors) vs X (scissors) -> draw
    'CZ': 7  #1 + 6  C (scissors) vs X (rock)     -> win
}

lines = list(map(str.strip, fin))
ans1 = ans2 = 0

for line in lines:
    a, b = line.strip().split()
    ans1 += scores1[a+b]
    ans2 += scores2[a+b]
    
advent.print_answer(1, ans1)
advent.print_answer(2, ans2)
```

## Day 03 - Rucksack Reorganization
[Problem](https://adventofcode.com/2022/day/3) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_03_Rucksack_Reorganization.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
We get a list of strings, where each string represents one elf's rucksack. The rucksack is divided into two even compartments, where elements of one compartment are (supposed to be) unique to that compartment. Unfortunately, the main packing elf made a mistake, and some items were put into both compartments by mistake. We have to identify the misplaced item (the one that shows up in both compartments) as well as assign a priority value to it. We then add all the values to get the answer for part 1.

For this, we can make great use of the [set()](https://docs.python.org/3/library/functions.html#func-set) function. This function takes in an object, and returns a set of unique elements within that object. But first, we need to divide each string in half to identify the compartments. Then, we simply initialize a set for each compartment.

```python
comp1 = line[:len(line)//2] #first half
comp2 = line[len(line)//2:] #second half

set1 = set(comp1)
set2 = set(comp2) 
```

The next line is an [intersection](https://docs.python.org/3/tutorial/datastructures.html#sets) operation on the two sets. This finds the element that exists in both sets.

```python
misplaced = set1 & set2
```

So that's part of the equation. The other part is converting each item to a priority. We're told that lowercase letters have priority ```1-26``` and uppercase letters have priority ```27-52```. Thankfully, this is made easier by using [string.ascii_letters](https://docs.python.org/3/library/string.html), which gives us one giant string of letters in alphabetical order. Then, we simply find the index of our misplaced item and add it to our ans1 variable.

```python
LETTERS = string.ascii_letters #concatenation of lowercase and uppercase ascii letters
idx = LETTERS.index(list(misplaced)[0])
ans1 += idx + 1
```
### Part 2
The second half of today's puzzle is a slight variation on the first part. Instead of looking at each rucksack as having two compartments, we now look at groups of three rucksacks, and we look at each rucksack as a whole. We no longer have to worry about compartments. Instead, we have to find which item is common between all three elves in a group. This follows the same process as part 1, with some slight modifications to our loop. We group them by threes, then we use some slick [list comprehension](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) to get the set of each elf's items.

```python
while (idx + 2) < len(rucksacks):
    group = rucksacks[idx:idx+3]
    elves = [set(elf) for elf in group]
```

After that, the process is identical to part 1. We have to remember to increment our ```idx``` variable by 3, because we have to group them by threes.

```python
    misplaced = elves[0] & elves[1] & elves[2]
    p = LETTERS.index(list(misplaced)[0])
    ans2 += p + 1
    idx += 3
    
advent.print_answer(1, ans1)
advent.print_answer(2, ans2)
```
## Day 04 - Camp Cleanup
[Problem](https://adventofcode.com/2022/day/4) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_04_Camp_Cleanup.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
Today we are given a list of pairs of section IDs for which each elf is responsible for cleaning. We need to figure out which pairs have a full overlap of section IDs. For example:

```
Given     Visually
 2-8      .2345678.
 3-7      ..34567..

 6        .....6...
 4-6      ...456...

 2-6      .23456...
 4-8      ...45678.
```

For part one we are only concerned about finding the number of assignment pairs where one range *fully* overlaps the other. In the above, it would be the first and second pairs.

To start, we just need to parse the input and [split()](https://docs.python.org/3/library/stdtypes.html#str.split) it into appropriate pieces of data, while not forgetting to [map](https://docs.python.org/3/library/functions.html#map) the inputs to integers:

```python
for line in fin:
    a, b   = line.strip().split(',')
    a1, a2 = map(int, a.split('-'))
    b1, b2 = map(int, b.split('-'))
```

Then we need to calculate the points where the two ranges overlap. We can compute the extremes of the overlap by simply calculating the maximum between the two range start values and the minimum between the two range end values. We can make good use out of the [min()](https://docs.python.org/3/library/functions.html#min) and [max()](https://docs.python.org/3/library/functions.html#max) functions here. 

```python
o1 = max(a1, b1)
o2 = min(a2, b2)

if o1 == a1 and o2 == a2 or o1 == b1 and o2 == b2:
    full_overlap += 1
    
advent.print_answer(1, full_overlap)
```

### Part 2
For part 2, we're tasked with finding out the number of range pairs that overlap paritally *or* fully. The number of pairs from part 1 will still count for part 2. We simply need to calculate partial overlaps. To do this, we can calculate the two extremes of the overlap calculated from part 1:

```
    a1|------------|a2     |            a1|--------|a2
b1|---------|b2            |   b1|--|b2
    o1|-----|o2            |        |o2 o1|
      overlap (o2 >= o1)   |       no overlap (o2 < o1)
```

All of this simply means adding one check to our part 1 code, and since we know that a full overlap is a special case of a partial overlap, we can move the part 1 check inside the part 2 one like so:

```python
if o2 >= o1:
    overlap +=1
    if o1 == a1 and o2 == a2 or o1 == b1 and o2 == b2:
        full_overlap += 1
        
advent.print_answer(2, overlap)
```

## Day 05 - Supply Stacks
[Problem](https://adventofcode.com/2022/day/5) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_05_Supply_Stacks.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
Today is the first puzzle (so far) where parsing the input is more difficult than the actual problem. In today's puzzle, we are simulating a giant crate-moving crane. We are given our initial state of crates:

```
            [M] [S] [S]            
        [M] [N] [L] [T] [Q]        
[G]     [P] [C] [F] [G] [T]        
[B]     [J] [D] [P] [V] [F] [F]    
[D]     [D] [G] [C] [Z] [H] [B] [G]
[C] [G] [Q] [L] [N] [D] [M] [D] [Q]
[P] [V] [S] [S] [B] [B] [Z] [M] [C]
[R] [H] [N] [P] [J] [Q] [B] [C] [F]
 1   2   3   4   5   6   7   8   9
 ```
 
 After a line break we are also given a long list of instructions in the form ```move [n] from [i] to [j]```, like the following:
 
 ```
move 1 from 7 to 4
move 3 from 4 to 7
move 4 from 3 to 4
```

After executing all instructions, we need to combine the letters from each of the topmost crates in each stack. For example, if the above configuration was the outcome after all instructions, our answer would be ```GGMMSSQFG```. The easiest way of doing this is to effectively transpose each row of input, which would essentially give us the columns. The [zip()](https://docs.python.org/3/library/functions.html#zip) function, as well as an [unpacking operator](https://docs.python.org/3/tutorial/controlflow.html#unpacking-argument-lists) can make this much easier.

```python
inp = []

for line in fin:
    if line == '\n': #we stop at the line break
        break
    inp.append(line)
```

Then we use zip() to transpose the input. After transposing, we only need to take 1 from every 4 characters (the capital letter). Using the [modulo](https://docs.python.org/3.3/reference/expressions.html#binary-arithmetic-operations) operator makes this simple too. I won't go into too much detail about parsing this, because frankly it was really annoying.

```python
stacks = [None]
moves = []

cols = list(zip(*inp)) #transposes each column

for i, col in enumerate(cols):
    if i % 4 == 1:
        stacks.append(''.join(col[:-1]).lstrip())
        
original = stacks[:] #create copy for part 2
```

We then need to parse the actual instructions, which is thankfully more straightforward than the previous bit since we only care about the numbers, not the words.

```python
for line in fin:
    line = line.split()
    moves.append((int(line[1]), int(line[3]), int(line[5])))
```

Basically, what we need to do is isolate the "chunk" of crates that we need to move, update the state of the stack it came from, and then update the stack where the chunk is going. Because part 2 is similar, I've already turned the code into a function.

```python
def move_crates(stacks, moves, rev=True):
    for n, i, j in moves:
        if rev: chunk = stacks[i][:n][::-1] #part1
        else: chunk = stacks[i][:n]         #part2
        stacks[i] = stacks[i][n:]           #updates source stack after removing top crate
        stacks[j] = chunk + stacks[j]       #updates destination stack after adding crate
    
    return ''.join(s[0] for s in stacks[1:]) 
    
ans1 = move_crates(stacks, moves)
advent.print_answer(1, ans1)
```

### Part 2
For part 2 we learn that our crate-moving crane is actually capable of moving multiple boxes at the same time, as opposed to moving only one at a time. We now how to redo the instructions again using this new information. All that changes really, is how we define the "chunk" that gets moved. Previously, we needed to reverse it since each crate was moved individually. We no longer have to reverse it since all the crates get moved together. The important thing is to restore our stacks to their original state, which is why we made a copy earlier.

```python
stacks = original #restores from copy we made earlier
ans2 = move_crates(stacks, moves, False)

advent.print_answer(2, ans2)
```

## Day 06 - Tuning Trouble
[Problem](https://adventofcode.com/2022/day/6) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_06_Tuning_Trouble.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
We're given one very long string (which I have named ```buffer```) filled with seemingly random characters. Our goal is to find the ```start-of-packet``` message, which contains ```4``` unique characters. This is incredibly simple with the use of the ```set``` data structure, and a simple ```for loop```. All that we need to do is start and the first character ```i```, and create a set that goes to ```i + 4```. If the length of that set is equal to 4, we have found our message.

```python
for i in range(len(buffer) - 4):
    if len(set(buffer[i:i+4])) == 4:
        return i + 4 
```

### Part 2
Part 2 is identical to part 1, except now instead of checking for a packet that is 4 unique characters long, we need to find a packet that is 14 characters long. Following the [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) principle, instead of repeating our code from part 1, we can turn it into a function instead. We can pass in the length of the message we're looking for as an argument.

```python
def find_marker(m_len):
    for i in range(len(buffer) - (m_len)):
        if len(set(buffer[i:i+m_len])) == m_len:
            return i + m_len        
        
ans1 = find_marker(4)  #start-of-packet
ans2 = find_marker(14) #start-of-message

advent.print_answer(1, ans1)
advent.print_answer(2, ans2)
```

## Day 07 - No Space Left On Device
[Problem](https://adventofcode.com/2022/day/7) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_07_No_Space_Left_On_Device.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
Not going to lie, this was the hardest part so far. We need to find the sum of all directories whose size is < 100,000. Parsing the input into a sensible, logical file system isn't necessarily difficult, just confusing if you don't know where to start (hint: I did not know where to start, and had to look at other people's code). Essentially, we need to have two functions: 1) a function to parse the filesystem and create a data structure that we can manipulate. 2) A function that calculates the size of a given directory (technically a path).

For the first part, we can describe our filesystem as a [tree](https://en.wikipedia.org/wiki/Tree_(data_structure)) structure, where our root node is ```/```. We can parse the input line by line and determine what the path of the current directory is. We will then store that path in a dictionary with the form ```{path: list_of_contents}```. For each line we must do the following:

* If we encounter the ```cd``` (change directory) command, we will add a new component to the path. If the ```dir``` we ```cd``` into is ```..```, then we remove the last added component.
* If we encounter the ```ls``` (list) command, we need to read each following line until we encounter another ```$``` command. Each line read in this section will be either one of two things: a subdirectory ```dir```, or ```<size> <filename>```. If it's a directory we can safely ignore it, but if it's a filesize, we need to add it to the list of contents for that specific directory.

For this, two tools can help out tremendously: [defaultdict](https://docs.python.org/3/library/collections.html#collections.defaultdict) and [deque](https://docs.python.org/3/library/collections.html#collections.deque). A ```defaultdict``` comes in handy because we don't have to worry about a path not being in our dictionary already (```defaultdict``` gives every key a default value, in our case it will be a ```list```). A ```deque``` also helps process the input line by line while being able to peek at the next line without consuming it, since we want to stop parsing the output of the ```ls``` command whenever we encounter a line starting with ```$```. We can peek the first element of a deque with ```d[0]```, and consume it by popping it ```d.popleft()```.

```python
from collections import deque, defaultdict

def parse_filesystem(fin):
    lines = deque(fin)
    fs    = defaultdict(list)
    path  = ()
    
    while lines:
        line = lines.popleft().split() #['$', 'cd', 'foo']
        command = line[1] #'cd' or 'ls'
        args    = line[2:] #dir name, file, or nothing
        
        if command == 'ls':
            while lines and not lines[0].startswith('$'):
                size = lines.popleft().split()[0]
                if size == 'dir': #ignore if it's a directory
                    continue
                fs[path].append(int(size)) #adds file size to contents of path
        else: #cd into different dir
            if args[0] == '..':
                path = path[:-1]
            else:
                new_path = path + (args[0],)
                fs[path].append(new_path)
                path = new_path
    return fs
    
ffs = parse_filesystem(fin)
```

The result of the above is our filesystem, which would look something like this:

```python
fs = {
    ()        : [('/',)]
    ('/',)    : [1000, 699, ('/', 'a')]
    ('/', 'a'): [100, 200]
}
```

Ultimately, we now have a dictionary of all the paths in our filesystem, as well as their contents. Now we need our second function to go through and calculate the size of each path. We need to traverse our tree, and sum up and file sizes we find along the way. The simplest way is by doing a [depth-first search](https://en.wikipedia.org/wiki/Depth-first_search). We can also use this function recursively, to help simplify the process. Given a path, we are going to check each item in that path's contents (using our newly created dictionary) and check whether or not it's an ```int``` or a ```dir```. If it's an ```int```, we can add it to the total size. If it's not a file, we can call the function recursively to determine that subdireectory's size. 

```python
@lru_cache(maxsize=None)
def directory_size(path):
    size = 0

    for subdir_or_size in fs[path]:
        if isinstance(subdir_or_size, int):
            size += subdir_or_size
        else:
            size += directory_size(subdir_or_size) #recursively calculates size

    return size
```
That's the basic gist of it. We will iterate through each key of ```fs``` which represents every path to all the directories we have) and call the ```directory_size``` function on it - making sure to sum only those whose sizes are less than 100,000. You'll notice the decorator at the top of our second function. This is to help reduce the need of recalculating expensive functions multiple times. This is a topic referred to as [memoization](https://en.wikipedia.org/wiki/Memoization) - a concept that was absolutely brutal for me to wrap my head around last year (it still is, actually). Luckily, Python 3 already provides us with a decorator to implement memoization out of the box: [`functools.lru_cache`](https://docs.python.org/3/library/functools.html#functools.lru_cache).

Regardless of how many times we call `directory_size()`, the calculation is only performed *once* for any given path on the first call and cached automatically by `lru_cache` to be reused for subsequent calls. We finally have enough to answer part 1:

```python
dir_size = 0
for path in fs:
    size = directory_size(path)
    if size <= 100000:
        dir_size += size
        
advent.print_answer(1, dir_size)
```

### Part 2

Now we need to find a single directory to delete to free up some space on our device (hence the puzzle name). The total size of our device is `70000000`. The amount of space we need for the update is `30000000`. We're now tasked with finding the *smallest* directory we can delete to have at least `30000000` units of free space. This is a simple matter, as all the of the hard work is done already.

```python
dir_size = 0
used = directory_size(('/',))
free = 70000000 - used
need = 30000000 - free
min_size_to_free = used

for path in fs:
    size = directory_size(path)
    if size <= 100000:
        dir_size += size
    if size >= need and size < min_size_to_free:
        min_size_to_free = size
        
advent.print_answer(1, dir_size)
advent.print_answer(2, min_size_to_free)
```

That was a rather involved puzzle, and I know it's only going to get worse. :)

## Day 08 - Treetop Tree House
[Problem](https://adventofcode.com/2022/day/8) - [Solution](https://github.com/noah-kg/AdventOfCode/blob/main/2022/solutions/Day_08_Treetop_Tree_House.ipynb) - [Back to top](https://github.com/noah-kg/AdventOfCode/tree/main/2022#advent-of-code-2022-walkthrough)

### Part 1
We are (finally) given a grid of numbers representing the heights of trees. The elves want to bulid a secluded treehouse that is not visible from outside the grid. We need to calculate the total number of trees that are visible from outside of the grid. Our input is a bunch of strings, but we don't need to convert them to integers because we're still dealing with ASCII characters, and their byte values are ascending from 0 through 9.

We read and split the input like so:

```python
trees = fin.read().split()
```
To check each tree, we'll need to traverse through our grid using two of our favorite [`for`](https://docs.python.org/3/tutorial/controlflow.html#for-statements) loops with the help of ['enumerate'](https://docs.python.org/3/library/functions.html?highlight=enumerate#enumerate). This will help us keep track of each row and column index.

```python
for r, row in enumerate(trees):
    if r == 0 or r == maxr: #edges
        continue
        
    for c, tree in enumerate(row):
        if c == 0 or c == maxc: #edges
            continue
```

Because we need to check if a tree is visible from all sides, we have to have 4 separate checks for each cardinal direction. Once we figure out one direction, the others are essentially the same. Checking east & west first is easiest however, because we're already looping through each column in the grid (we're going one row at a time, checking each tree from left to right, ignoring the edges because those trees are always visible). Using a simple [generator expression](https://peps.python.org/pep-0289/), we can compare the height of our current tree, to all trees before/after it.

```python
        west  = (tree > t for t in row[:c])                      #checks trees to the left
        east  = (tree > t for t in row[c+1:])                    #checks trees to the right
        north = (tree > trees[i][c] for i in range(r-1, -1, -1)) #checks trees directly above
        south = (tree > trees[i][c] for i in range(r+1, H))      #checks trees directly below
```

Each one of those statements generates a True/False value if our current tree is taller than the other trees in that specific direction. We can then use the [`all()`](https://docs.python.org/3/library/functions.html#all) function to see if the tree is taller than *all* the other trees in that specific direction. We can then combine all four of our generator expressions with a simple `or` command to determine if the tree is visible at all.

```python
        if all(west) or all(east) or all(north) or all(south):
            total_visible += 1
            
advent.print_answer(1, total_visible)
```

### Part 2
Now the elves want to figure out the optimal building site for their tree house. Their goal is to see the most trees, so now we need to calculate a "scenic score", which is the product of each tree's "viewing distance" for all four directions. The viewing distance is the number of trees whose height is lower than the given tree. So we need to iterate through the trees again, and check each tree in all four directions to see whether or not it meets the criteria.

Similar to part 1, once you figure out one direction, the other three are essentially the same process.

```python
for west in range(c-1, -1, -1):
    if row[west] >= tree:
        break

for east in range(c+1, W):
    if row[east] >= tree:
        break

for north in range(r-1, -1, -1):
    if trees[north][c] >= tree:
        break

for south in range(r+1, H):
    if trees[south][c] >= tree:
        break
```

In our case, we are keeping track of the first tree that is *taller than or equal to* our current tree. We keep track of the index of the taller tree in the direction variable. After we calculate all four directions, we need to carefully calculate the score, remembering to get the difference between the current tree and the tall tree.

```python
score = (c-west) * (east-c) * (r-north) * (south-r)

if score > hi_score:
    hi_score = score
    
advent.print_answer(2, hi_score)
```
