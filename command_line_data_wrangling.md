## A Command

* Every command your run has:
	* Stream 0, an input stream: stdin(Standard Input)
	* Stream 1, an output stream: stdout (Standard Output)
	* Stream 2, stderr
	* All three attach to the terminal by default.
* Attach one commands stdout to another command's stdin with a pipe:
	* cmdA | cmdB
* Attach a file to a command's stdin with a less-than:
	* cmd < inputFile
* Attach a commands stdout to a file with a greater-than:
	* cmd > outputFile

## Redirecting
* stderr often stays attached to the terminal
	* cmdA |& cmdB
* You can use the output of a command:
	* IN place of an argument: cmdA $(cmdB)
	* In place of a file: cmdA -f <(cmdB)
* You can append to a file instead of overwriting it:
	* Overwrite cmd > outputFile
	* Append: cmd >> outputFile
* TO write a file but still see the output in your terminal you can use tee:
	* Overwrite : cmd | tee outputFile
	* Append: cmd | tee -a outputFile

## Plain Text
* Many tools output plain text
* Usually there's one item per line
* Sometimes there's more to one field per line, seperated by whitespace
* Extract or exclude lines with grep:
	* Extract cmd | grep pattern
	* Exclude cmd | grep -v pattern
* Extract or remove fields with awk:
	* Extract third field cmd | awk '{print $3}'
	* Extract last field: cmd | awk '{print $NF}'
	* Remove fifth field : cmd | awk '{$5 = ""; print $0}'
You can use *cut* to extract fields too, but it doesn't handle mixed whitespace (tabs and spaces) as well as awk

## Dupes and stats and dupes and stats
* It's common to have duplicate lines: domains, URLs IPs etc
* You can remove duplicates in a couple of ways:
	* with sort: cmd | sort -u
	* With anew: cmd | anew
	* Using anew leaves the order of your data alone
	* Write unique lines to a file: cmd | anew outputFile
* Sorting by frequency is useful for wordlists etc:
	* With counts: cmd | sort | unic -c | sort -nr
	* Without counts: cmd | sort | uniq -c | sort -nr | awk '{print $NF}'

## Regular Expressions
* Regular Expressions are descriptions of patterns in text
	* Used to match, extract, and replace parts of text
	* Often cited as something many Peokple don't understand
* There is a lot to learn, but some basics are:
	* Any character:.
	* Classes [a-z][0-9][0-9A-Za-z_/]
	* Negated classes: [â-z[^0-9]
	* Zero or one: ?
	* Zero or more"*
	* One or more: +
	* Two to seven: {2,7}
	* At least three: {3,}
	* Groups: (one|two|three)
	* Start of line: ^
	* End of line: $
## Grep
* Grep works on stdin by default, and outputs any matching line
	* Match subdomains of example.com : cat domains.txt | grep "\.example\.com$'
* Use -o to extract the text that matches; -E for extended regular expressions
	* Extract TLDs from domains: cat domains.text | grep -oE '\.[a-zA-Z]+$'
* Search recursively with -r
	* Search current directory for GCP keys: grep -rE 'Alza[a-zA-Z0-(]+'*
* Exclude lines with -v; be case-insensitive with -i
	* Exclude blog subdomains; cat domains.txt | grep -vi blog
Store and use your complex patterns with gf:)
	* https://github.com/tomnomnom/gf

## SED 
* Sed is the Stream Editor - use it for match and replace
	* Basic usage : cmd | sed -r 's/search/replace/'
	* You can use something else instead of the slashes too: cmd | sed -r 's#search#replace#'
* YOu can refer to groups with \1, \2, \3 etc
* Add a g (for global) to the end to replace all matches:
* Use anchors (^and $) to match the start and end of lines

## Find
* The find command lists files and directiories recursively, with filters
* You can filter by tupe:
	* Files in the current directory: find . -type f
	* Directories in /etc: find /etc/ -type d
* By name:
	* Files ending with .txt: find . -type f -name '*.txt'
	* Anything containing 'conf': find .-name '*conf*'
* By size:
	* Emty files: find . -size 0
	* Bigger than 200k: find . -size +200k
* By when they changed
	* In the last 5 minutes: find . -mmin '-5'
	* More than 30 minutes ago: find . -mmin '+30'
* And a ton more things...
	* man find

## Xargs
* Run a command using each line of input as an argument
* Use -n to specify how many lines to pass each time
* Use -P to specify how many commands to run in parallel
* Use -l and a placeholder (e.g. {}) to control where the argument goes
* When things get complex, consider while loops 
