# Lab Report 3

Name: Harsh Gurnani

PID: A17342880

Email: hgurnani@ucsd.edu

---

## Command: grep

For this lab, I will be using the grep command and 4 of its command-line options. Grep is used to search files for a line that contains a certain string or expression. It prints that some contents of that file to the command line. With various options, we can change how we use grep to give us more useful outputs.



## -l

Grep normally prints out both the file name and some contents of the file, which can be useful but often just clutters the terminal environment and makes it hard to get any readable output. To fix this, we can use the -l command, which results in just the file names being printed instead of the contents as well. In this way, we free up our terminal from having unnecessary text. Let's look at an example.

### Example 1

Let's try searching for the string "Amsterdam" in two files.

```
grep "Amsterdam" written_2/travel_guides/berlitz2/Amsterdam-History.txt 
written_2/travel_guides/berlitz2/Amsterdam-Intro.txt

written_2/travel_guides/berlitz2/Amsterdam-History.txt:Spanish rule was ruthless but, 
for a while, Amsterdam was left alone. Its position as an important trading post kept 
it apart from the more barbarous behavior in other areas. It saw a threefold increase 
in its population as refugees flooded in from other parts of the empire. Diamond 
polishers from Antwerp and Jews from Portugal brought their influences to the city.
written_2/travel_guides/berlitz2/Amsterdam-History.txt:Amsterdam was already developing 
a reputation of tolerance as these new and disparate groups settled into the city. At 
the same time, Martin Luther’s new Christian doctrine, Protestantism, was spreading 
like wildfire across Europe. It took a firm foothold in the northern provinces of the 
Low Countries — with Amsterdam at its heart. It was at this time that Huguenots (French 
Protestants) came to Amsterdam to flee persecution in their own country.
written_2/travel_guides/berlitz2/Amsterdam-History.txt:To counter
...
...
...
```

As visible, there is a LOT of text involved that we don't care much about. Instead, we can just use the -l option to just print file names.

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -l "Amsterdam" 
written_2/travel_guides/berlitz2/Amsterdam-History
.txt written_2/travel_guides/berlitz2/Amsterdam-Intro.txt

written_2/travel_guides/berlitz2/Amsterdam-History.txt
written_2/travel_guides/berlitz2/Amsterdam-Intro.txt
```

### Example 2

On the flip side, sometimes the -l option takes away some useful options. Let's say we store the names of all the files in `written_2` in a file called `find_results.txt`. To find files with a certain name (in this case, containing "Amsterdam"), we can try doing 'grep -l'.

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ find written_2/ > find_results.txt

Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -l "Amsterdam" find_results.txt
find_results.txt
```

The result is just the name of the file containing all the file names. Instead, we want to just use the normal grep command.

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ find written_2/ > find_results.txt
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep "Amsterdam" find_results.txt

written_2//travel_guides/berlitz2/Amsterdam-WhereToGo.txt
written_2//travel_guides/berlitz2/Amsterdam-WhatToDo.txt
written_2//travel_guides/berlitz2/Amsterdam-History.txt
written_2//travel_guides/berlitz2/Amsterdam-Intro.txt
```

## -i

Originally, grep searches for a string that is case specific, as we can see from the two upcoming examples. The -i option forces grep to ignore case and look at all results that have the same letters. 

### Example 1

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep "vista" written_2/travel_guides/berlitz2/Canada-WhereToGo.txt 
written_2/travel_guides/berlitz2/Costa-WhereToGo.txt

written_2/travel_guides/berlitz2/Costa-WhereToGo.txt:There are only two major sights. 
Abd-er-Rahman III’s massive Alcazaba looms large on the hilltop above the city. 
Although an earthquake caused extensive damage in 1522, the crenellated ocher outer 
walls and a section of the turreted ramparts stand firm, providing wide-ranging 
vistas over the city and the sea. And the forbidding, fortified cathedral that stands 
just inland from the waterfront Paseo de Almería was built during the 16th century, 
when Barbary pirates were terrorizing the coast (see page 18).
```

> Only searches for "vista" with lower-case v

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep "Vista" 
written_2/travel_guides/berlitz2/Canada-WhereToGo.txt 
written_2/travel_guides/berlitz2/Costa-WhereToGo.txt

written_2/travel_guides/berlitz2/Canada-WhereToGo.txt:If you want a view of the whole 
city and the North Saskatchewan River on your way home, stop off at Vista 33, the 
observation level of the telephone building.
```

> Only searches for "vista" with upper-case v

With the -i option, we can make grep search for a certain string without being particular about case. If we ran the previous command with -i, the output would be the combination of searching for both "vista" and "Vista"

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -i "vista" 
written_2/travel_guides/berlitz2/Canada-WhereToGo.txt 
written_2/travel_guides/berlitz2/Costa-WhereToGo.txt

written_2/travel_guides/berlitz2/Canada-WhereToGo.txt:If you want a view of the whole 
city and the North Saskatchewan River on your way home, stop off at Vista 33, the 
observation level of the telephone building.

written_2/travel_guides/berlitz2/Costa-WhereToGo.txt:There are only two major sights. 
Abd-er-Rahman III’s massive Alcazaba looms large on the hilltop above the city. 
Although an earthquake caused extensive damage in 1522, the crenellated ocher outer 
walls and a section of the turreted ramparts stand firm, providing wide-ranging 
vistas over the city and the sea. And the forbidding, fortified cathedral that stands 
just inland from the waterfront Paseo de Almería was built during the 16th century, 
when Barbary pirates were terrorizing the coast (see page 18).
```

### Example 2

We can try something similar, but this time by passing in multiple words to search for. In this case, we search for both the words "moor" and "ranches", first passing in a capital version of the latter. 

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -l "moor\|Ranches" 
written_2/travel_guides/berlitz2/Barcelona-
History.txt written_2/travel_guides/berlitz1/WhatToMallorca.txt 
written_2/travel_guides/berlitz2/Portugal-WhatToDo.txt

written_2/travel_guides/berlitz1/WhatToMallorca.txt
```

> Only shows one file when searching for "moor" and "Ranches"

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -l -i "moor\|Ranches" 
written_2/travel_guides/berlitz2/Barcelona-
History.txt written_2/travel_guides/berlitz1/WhatToMallorca.txt 
written_2/travel_guides/berlitz2/Portugal-WhatToDo.txt

written_2/travel_guides/berlitz2/Barcelona-History.txt
written_2/travel_guides/berlitz1/WhatToMallorca.txt
written_2/travel_guides/berlitz2/Portugal-WhatToDo.txt
```

> Shows all three files when ignoring capitalization

*Note that in the above examples, we also used -l to print only the file names*

Clearly, -i is an extremely useful tool to add onto grep. It is very common to want to search for a word without caring about upper or lower case, making -i a very popular option. 


## -r

One of the main limitations of grep is that you have to pass in a specific file to search in. The -r option overcomes this. With -r, we can pass in a directory, and grep will recursively search for all the subfiles within that directory (and its subdirectories), and return every file with the correct expression.

### Example 1

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -rl "pants" written_2/

written_2//non-fiction/OUP/Berk/ch2.txt
written_2//non-fiction/OUP/Berk/ch1.txt
written_2//non-fiction/OUP/Berk/ch7.txt
written_2//non-fiction/OUP/Abernathy/ch2.txt
written_2//non-fiction/OUP/Abernathy/ch1.txt
written_2//non-fiction/OUP/Abernathy/ch8.txt
written_2//non-fiction/OUP/Abernathy/ch9.txt
written_2//non-fiction/OUP/Abernathy/ch15.txt
written_2//non-fiction/OUP/Rybczynski/ch1.txt
written_2//non-fiction/OUP/Kauffman/ch9.txt
written_2//non-fiction/OUP/Fletcher/ch9.txt
written_2//non-fiction/OUP/Castro/chP.txt
written_2//non-fiction/OUP/Castro/chV.txt
written_2//non-fiction/OUP/Castro/chM.txt
written_2//non-fiction/OUP/Castro/chZ.txt
written_2//travel_guides/berlitz1/WhatToGreek.txt
written_2//travel_guides/berlitz1/WhatToIndia.txt
written_2//travel_guides/berlitz1/WhereToHongKong.txt
written_2//travel_guides/berlitz2/Canada-WhereToGo.txt
written_2//travel_guides/berlitz2/Budapest-WhatToDo.txt
written_2//travel_guides/berlitz2/Vallarta-WhatToDo.txt
written_2//travel_guides/berlitz2/CstaBlanca-WhereToGo.txt
written_2//travel_guides/berlitz2/Crete-WhatToDo.txt
written_2//travel_guides/berlitz2/Nepal-WhatToDo.txt
```

> All the files under written_2 that contain the word "pants"


```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep "pants" written_2

grep: written_2: Is a directory
```

> Error produced when just passing in a directory name


### Example 2

We can put together all the options we've learned so far in this example. Previously, we searched two files for the words "vista" and "Vista". Now, we can find all the files that contain either version of the string by combining `-r`, `-i`, and `-l`.

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -r -i -l "vista" written_2/

written_2//non-fiction/OUP/Castro/chB.txt
written_2//travel_guides/berlitz1/IntroDublin.txt
written_2//travel_guides/berlitz1/IntroMadeira.txt
written_2//travel_guides/berlitz1/WhereToGreek.txt
written_2//travel_guides/berlitz1/WhereToLakeDistrict.txt
written_2//travel_guides/berlitz1/WhereToIsrael.txt
written_2//travel_guides/berlitz1/WhereToFrance.txt
written_2//travel_guides/berlitz1/WhereToMadeira.txt
written_2//travel_guides/berlitz1/WhereToIbiza.txt
written_2//travel_guides/berlitz1/WhereToLosAngeles.txt
written_2//travel_guides/berlitz1/WhereToJerusalem.txt
written_2//travel_guides/berlitz1/IntroLakeDistrict.txt
written_2//travel_guides/berlitz2/Costa-WhereToGo.txt
written_2//travel_guides/berlitz2/Portugal-WhereToGo.txt
written_2//travel_guides/berlitz2/Bahamas-WhereToGo.txt
written_2//travel_guides/berlitz2/Crete-WhereToGo.txt
written_2//travel_guides/berlitz2/Canada-WhereToGo.txt
written_2//travel_guides/berlitz2/Athens-WhereToGo.txt
written_2//travel_guides/berlitz2/China-WhereToGo.txt
written_2//travel_guides/berlitz2/CstaBlanca-WhereToGo.txt
written_2//travel_guides/berlitz2/PuertoRico-WhereToGo.txt
written_2//travel_guides/berlitz2/CanaryIslands-WhereToGo.txt
written_2//travel_guides/berlitz2/Algarve-WhereToGo.txt
written_2//travel_guides/berlitz2/Nepal-WhatToDo.txt
written_2//travel_guides/berlitz2/Vallarta-WhereToGo.txt
```

> A list of all files containing both "vista" and "Vista"

In fact, we can just put them all together as `-ril` to get the same result.

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -ril "vista" written_2/

written_2//non-fiction/OUP/Castro/chB.txt
written_2//travel_guides/berlitz1/IntroDublin.txt
written_2//travel_guides/berlitz1/IntroMadeira.txt
written_2//travel_guides/berlitz1/WhereToGreek.txt
written_2//travel_guides/berlitz1/WhereToLakeDistrict.txt
written_2//travel_guides/berlitz1/WhereToIsrael.txt
written_2//travel_guides/berlitz1/WhereToFrance.txt
written_2//travel_guides/berlitz1/WhereToMadeira.txt
written_2//travel_guides/berlitz1/WhereToIbiza.txt
written_2//travel_guides/berlitz1/WhereToLosAngeles.txt
written_2//travel_guides/berlitz1/WhereToJerusalem.txt
written_2//travel_guides/berlitz1/IntroLakeDistrict.txt
written_2//travel_guides/berlitz2/Costa-WhereToGo.txt
written_2//travel_guides/berlitz2/Portugal-WhereToGo.txt
written_2//travel_guides/berlitz2/Bahamas-WhereToGo.txt
written_2//travel_guides/berlitz2/Crete-WhereToGo.txt
written_2//travel_guides/berlitz2/Canada-WhereToGo.txt
written_2//travel_guides/berlitz2/Athens-WhereToGo.txt
written_2//travel_guides/berlitz2/China-WhereToGo.txt
written_2//travel_guides/berlitz2/CstaBlanca-WhereToGo.txt
written_2//travel_guides/berlitz2/PuertoRico-WhereToGo.txt
written_2//travel_guides/berlitz2/CanaryIslands-WhereToGo.txt
written_2//travel_guides/berlitz2/Algarve-WhereToGo.txt
written_2//travel_guides/berlitz2/Nepal-WhatToDo.txt
written_2//travel_guides/berlitz2/Vallarta-WhereToGo.txt
```


## -w

Curently, grep searches for a string in a file by finding any valid expression that contains it. However, what if we just want a specific word? By using -w, we force grep to pattern match to certain words. Essentially, it will separate words by blank spaces and search based on that.

### Example 1

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep "John" written_2//non-fiction/
OUP/Castro/chB.txt written_2//non-fiction/OUP/Castro/chV.txt written_2//
non-fiction/OUP/Castro/chM.txt

written_2//non-fiction/OUP/Castro/chB.txt:Bourke, Captain John Gregory (1846–1896)
written_2//non-fiction/OUP/Castro/chB.txt:Luke, Mark, John and Matthew
written_2//non-fiction/OUP/Castro/chV.txt:References Alarcon 1989; Brundage 1979; 
Demarest and Taylor 1956; Elizondo 1957; -Johnston...
written_2//non-fiction/OUP/Castro/chM.txt:There have been over twenty-one published 
versions of the Joaquín Murrieta legend. The first and for many years the one 
considered most -authentic was by John ...
written_2//non-fiction/OUP/Castro/chM.txt:Joseph Henry Jackson’s work traces the 
history of the legend, claiming all the information came from the John Rollin Ridge ...
```

> Searching for "John" in three files

We can see that in file `chV.txt`, the word John is actually found in the name Johnston. To find the files with just the word "John" and no variation that simply contains that string, we use -w.

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -w "John" written_2//non-fiction/OUP
/Castro/chB.txt written_2//non-fiction/OUP/Castro/chV.txt written_2//non-fiction/OUP/
Castro/chM.txt

written_2//non-fiction/OUP/Castro/chB.txt:Bourke, Captain John Gregory (1846–1896)
written_2//non-fiction/OUP/Castro/chB.txt:Luke, Mark, John and Matthew
written_2//non-fiction/OUP/Castro/chM.txt:There have been over twenty-one published 
versions of the Joaquín Murrieta legend. The first and for many years the one 
considered most -authentic was by John ...
written_2//non-fiction/OUP/Castro/chM.txt:Joseph Henry Jackson’s work traces the 
history of the legend, claiming all the information came from the John Rollin Ridge ...
```

> Only returns the results that contain the exact word "John"

### Example 2

A more visible example of this is when we put the results of grep into a file and look at the word count of each file. 

```
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -rl "social" written_2 > social-
count.txt
Neelams-MacBook-Pro:docsearch neelamgurnani$ grep -rlw "social" written_2 > social-
w-count.txt
Neelams-MacBook-Pro:docsearch neelamgurnani$ wc social-count.txt
      94      94    4708 social-count.txt
Neelams-MacBook-Pro:docsearch neelamgurnani$ wc social-w-count.txt
      82      82    4080 social-w-count.txt
```

> Combining r, l, and w modifiers. 

Just searching for the expression "social" in all possible forms, we get 94 results; searching for the word "social", we end up with 82. Perhaps other forms are commonly used that contain the phrase, such as "socially".
