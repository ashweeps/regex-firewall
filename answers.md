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

---- Task 4

#Command:
grep -Ec ' [0-9]{7}$' firewall.log

#Result: 2343

# For this task, the regex checks the size field at the end of each event line. The space before `[0-9]` helps start the match at the last field. The class `[0-9]` matches digits, and `{7}` means exactly seven digits. The `$` anchor makes sure the seven digit number is at the end of the line, so the match is for the size field.

---- Task 5

#Command:
sed -E -n '/^[^#]/ s/^([^ ]+) [^ ]+ ([^ ]+) ([^ ]+) .*/\1 \2 \3/p' firewall.log | head -n 5

#Result:
2018-05-25 FORWARD TCP
2018-02-22 FORWARD UDP
2018-03-20 REJECT UDP
2018-11-08 REJECT TCP
2018-07-24 REJECT TCP

# In this task, sed changes each event line into a shorter report with only three fields: date, action, and protocol. The option `-E` allows extended regular expressions, so the groups can be written with normal parentheses. The option `-n` stops sed from printing every line automatically. The part `/^[^#]/` makes sed work only on event lines, because header lines start with `#`. In the substitution, the first group `([^ ]+)` captures the date and saves it as `\1`. The next `[^ ]+` skips the time field because it is not needed. The second group `([^ ]+)` captures the action and saves it as `\2`. The third group `([^ ]+)` captures the protocol and saves it as `\3`. The `.*` part matches the rest of the line after the protocol. The replacement `\1 \2 \3` rebuilds the line using only the captured fields. The final `p` prints the modified line, and `head -n 5` limits the result to the first five lines.
