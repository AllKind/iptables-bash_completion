iptables-bash_completion
==


Description
===========

Programmable completion specification (compspec) for the bash shell to support the iptables and ip6tables programs (netfilter.org).


Programmable completion allows the user, while working in an interactive shell, to retrieve and auto-complete commands,
their options, filenames, etc.
Pressing [TAB] will complete on the current word, if only one match is found.
If multiple completions are possible, they will be listed by hitting [TAB] again.


Features
========

This completion specification follows the logic of iptables and will only show commands and options, 
when they are available for the current context (combination of commands and/or options).
Also some values entered by the user are check for validity and completion will not continue afterwards.
All together providing some kind of interactive help.

- Show and complete commands and options.
- Show and complete matches, targets and builtin and/or user-defined chains.
- Dynamicylly retrieve, show and complete:
	set names, services, protocols, active interfaces, cpu numbers, routing realms, user and group names, NFLOG logging groups, tc classes.
- Show and complete various arguments for matches and targets (those which are in any way predictable).


Installation
============

Put it into ~/.bash_completion or /etc/bash_completion.d/.

Tip: To make tab completion more handsome put the following into either /etc/inputrc or ~/.inputrc:

     set show-all-if-ambiguous on

This will allow single tab completion as opposed to requiring a double tab.

     set page-completions off

This turns off the use of the internal pager when returning long completion lists.


Compatibility
=============

Tested with iptables v1.4.16.3.

Compatibility for future iptables versions cannot be promised, as new options may appear, 
which of course are currently unpredictable.

Bash 3.x and upwards are supported.

The bash_completion (v2.0+) package is highly recommended, though not mandatory.

http://bash-completion.alioth.debian.org/

Some things might not be that reliable without it.
Also the colon (if there) is removed from COMP_WORDBREAKS.
This alteration is globally, which might affect other completions,
if they do not take care of it themselves.


Availability
============

https://github.com/AllKind/iptables-bash_completion

https://sourceforge.net/projects/ipt-bashcompl/
