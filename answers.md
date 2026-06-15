# answers.md

----Task 1

#Command:
grep -Ec '^[^#]' firewall.log
#Result: 100000

# This command counts the real firewall events and ignores the header lines. The ^ anchor checks the start of each line, and [^#] means the first character must not be #. The option -c prints only the number of matching lines.
