---
layout: page
title: "Advent of Code"
permalink: "/adventofcode.html"
---
## Why
I didn't have much else to do in the lead-up to finals, and I figured that it's a good idea to brush up on my coding skills (and learn some Python in the process). I'm going to be using this page to document my solutions and some comments (ex ante and ex post) that I have on the process.

## Solutions By Day

### Day 1:
```python
f = open('advent_1.txt','r')
elfsum = 0
elfcalories = list()
for line in f:
    if line in ['\n','\r\n']:
        elfcalories.append(elfsum)
        elfsum = 0
    else:
        elfsum += int(line)
elfcalories.sort(reverse=True)
top3elves = elfcalories[0]+elfcalories[1]+elfcalories[2]
print(top3elves)
# This is code for part 2, I already did part 1, which was just the max value instead of the max 3 values
```
Comments ex post:
- I found this problem pretty easy, thank god for the ``sort`` method
- I started on Day 2 and did Day 1 for the stars

### Day 2
```python
finalscore = 0
f = open('advent_2.txt','r')
s = f.readline()
while len(s)>0:
    c = s[0]
    d = s[2]
    mychoice = ord(d)-87
    opponentschoice = ord(c)-64
    if mychoice == opponentschoice:
        finalscore += 3
    else:
        if mychoice == 1:
            if opponentschoice == 2:
                finalscore+=0
            else:
                finalscore += 6
        elif mychoice == 2:
            if opponentschoice == 1:
                finalscore+=6
            else:
                finalscore += 0
        elif mychoice == 3:
            if opponentschoice == 2:
                finalscore += 6
            else:
                finalscore += 0
    finalscore += mychoice
    s = f.readline()

print(finalscore)
f.close()
f2 = open('advent_2.txt','r')
finalscore = 0
for line in f2:
    oppchoice = line[0]
    roundresult = line[2]
    if roundresult == 'Y':
        finalscore += ord(oppchoice) - 64 + 3
    elif roundresult == 'X':
        if oppchoice == 'A':
            finalscore += ord('C') - 64
        elif oppchoice == 'B':
            finalscore += ord('A') - 64
        elif oppchoice == 'C':
            finalscore += ord('B') - 64
    elif roundresult == 'Z':
        if oppchoice == 'A':
            finalscore += ord('B') - 64 + 6
        elif oppchoice == 'B':
            finalscore += ord('C') - 64 + 6
        elif oppchoice == 'C':
            finalscore += ord('A') - 64 + 6
print(finalscore)
```
Comments ex post:
- Saw someone on Twitter who used dictionaries to solve this
- Nested conditionals are annoying to track

### Day 3
```python
f = open('advent_3.txt','r')
sum1 = 0
for line in f:
    midpoint = int(len(line)/2)
    s1 = set(line[0:midpoint])
    s2 = set(line[midpoint:])
    indexChar = list(s1.intersection(s2))[0]
    if indexChar.isupper():
        sum1 += ord(indexChar) - 64 + 26
    else:
        sum1 += ord(indexChar) - 96
print(sum1)
f.close()
sum2 = 0
f2 = open('advent_3.txt','r')
rucksacks = f2.readlines()
for i in range(0,len(rucksacks),3):
    set1 = set(rucksacks[i]).intersection(set(rucksacks[i+1])).intersection(set(rucksacks[i+2]))
    set1.remove('\n')
    indexChar = list(set1)[0]

    print(indexChar)
    if indexChar.isupper():
        sum2 += ord(indexChar)-64+26
    elif indexChar.islower():
        sum2 += ord(indexChar)-96

print(sum2)
```
Comments ex post:
- I almost used absurdly massive conditional statements on this, but a friend told me about sets before then, thank god
- This was the first time I realized there was a part 2, so I needed to go back and rework Day 2 and 1

### Day 4
```python
# get a line from the text input
# split based on the comma (figure out how to do this), use s1 and s2
# draw up a set whose minimal value is the first number up until the hyphen and number after hyphen to end of s1
# similarly draw up set based on s2
# if s1 is subset of s2 or vice versa, add 1 to sum

f = open('advent_4.txt','r')
contained_ranges = 0
overlaps = 0
for line in f:
    ranges = line.split(',')
    range_1 = ranges[0].split('-')
    range_2 = ranges[1].split('-')
    set1 = set()
    set2 = set()
    for i in range(int(range_1[0]),int(range_1[1])+1,1):
        set1.add(i)
    for i in range(int(range_2[0]),int(range_2[1])+1,1):
        set2.add(i)
    if set1.issubset(set2) or set2.issubset(set1):
        contained_ranges += 1
    if not(set1.intersection(set2) == set()):
        overlaps += 1
print(contained_ranges)
print(overlaps)
```

### Day 5
```python
f = open('advent_5.txt','r')
# read all lines into array
# read first 9 columns vertically into 2 dimensional array, remove all blank spaces from each column
# delete values 'move', 'from', and 'to' from lines with index 10 onward
# for each line, first value denotes iteration (number of operations), next - 1 denotes initial column, last - 1 denotes final column
# iterate along iteration values, pop index zero into string s, then insert string s into index zero of final list
# iterate until the end of file
# at the end, create string final_crates which is the value of each list at zero, then remove brackets
advent_5_list = f.readlines()  
print(len(advent_5_list))
crates = []
for i in range(9):
    crates.append([])
# reading a 9 element empty list
for i in range(0,8):
    for j in range(0,35,4):
        crates[int(j/4)].append(advent_5_list[i][j:j+3])
#reading the first 8 rows vertically into the list
for i in crates:
    while i.count('   ')>0:
        i.remove('   ')
#clearing the list of space characters

for i in range(10,len(advent_5_list),1):
    advent_5_list[i] = advent_5_list[i].replace('move','')
    advent_5_list[i] = advent_5_list[i].replace('from','')
    advent_5_list[i] = advent_5_list[i].replace('to','')
    # remove excess words from each line
    numlist = advent_5_list[i].split()
    # split each new line into list with three numbers
    print("now dealing with line",i)
    count = int(numlist[0]) # count is the number of boxes we move
    initbox = int(numlist[1]) # take from second value
    finalbox = int(numlist[2]) # deposit at front of third value
    for i in range(count)[::-1]: #for part 1, we don't use the slicing and just count forward
        #we need to pop from i-1 to 0 for part 2
        s = crates[initbox-1].pop(i) # for part 1, pop 0 instead of i
        crates[finalbox-1].insert(0,s)

for i in crates:
    print(str(i[0]))
```
Comments ex post:
- I spent most of my time trying to figure out how to read in the first 8 lines vertically, and I figured I would just hard code it
- Used a slicing operator in the for loop for the first time to get the removal order for Part 2
- I didn't clean the output to produce the proper strings

### Day 6
```python
f = open('advent_6.txt','r')
datastream_buffer = f.readline()
for i in range(0,len(datastream_buffer)-3,1):
    # we want to convert string from i to i+3 into a set and check if it has length four
    test_set = set(datastream_buffer[i:i+14]) #for part 1, we use 4, but for part 2 we use 14
    if len(test_set) == 14:
        print(i+14)
        #for part 1, we process 4 characters, but for part 2 we process 14
        break
```
Comments ex post:
- thatwaseasy.mp4

### Day 7
Comments ex ante:
- I spent like 2 hours looking up how trees work and I still don't know how to do this
- Finals getting into higher gear so I think I will push this and all the days up to the 14th until I go home for winter break.
- I'll ask my mom for help
- I tried a couple methods and it still didn't work

### Update on December 15
- I don't think I will be able to do any of these other problems, none of the following problems are any easier than what we have currently 
- It was a good run, and I did learn a bit of stuff like sets and lists in python, maybe I'll try to take a stab at this next time around
