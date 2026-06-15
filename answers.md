# answers.md

----Task 1

#Command:
grep -Ec '^[^#]' firewall.log

#Result: 100000

# This command counts the real firewall events and ignores the header lines. The ^ anchor checks the start of each line, and [^#] means the first character must not be #. The option -c prints only the number of matching lines.

---- Task 2

#Command:
grep -Ec '^[^ ]+ [^ ]+ (DROP|REJECT) ' firewall.log

#Result: 60156
# This counts the events where the action field is DROP or REJECT. The regex starts with ^ to begin at the start of the line, then [^ ]+ [^ ]+ skips the date and time fields. The group (DROP|REJECT) matches only one of those two actions, and the spaces around it make sure the match happens in the action field.

---- Task 3

#Command:

grep -Ec '^[^ ]+ [^ ]+ [^ ]+ [^ ]+ 11\.' firewall.log
#Result: 33217
# The goal here is to find source IP addresses that begin with 11. The first four `[^ ]+` parts move through the date, time, action, and protocol fields. After that, the pattern reaches the source IP field. The part `11\.` matches the number 11 followed by a real dot. The dot needs `\` because a normal dot in regex means any character.
