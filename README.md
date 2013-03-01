iptables-bash_completion
========================


Description
===========

Programmable completion specification (compspec) for the bash shell
to support the iptables and ip6tables programs (netfilter.org).


Programmable completion allows the user, while working in an interactive shell,
to retrieve and auto-complete commands, their options, filenames, etc.
Pressing [TAB] will complete on the current word, if only one match is found.
If multiple completions are possible, they will be listed by hitting [TAB] again.


Features
========

This completion specification follows the logic of iptables and will only show commands and options, 
when they are available for the current context (combination of commands and/or options).
Also some values entered by the user are checked for validity and completion will not continue afterwards.
All together providing some kind of interactive help.

- Show and complete commands and options.
- Show and complete matches, targets and builtin and/or user-defined chains.
- Dynamically retrieve, show and complete:
	set names, services, protocols, active interfaces, cpu numbers, routing realms,
	user and group names, NFLOG logging groups, tc classes, nfacct names.
- Show and complete various arguments for matches and targets (those which are in any way predictable).


Installation
============

Put it into ~/.bash_completion or /etc/bash_completion.d/.

Tip: To make tab completion more handsome put the following into either /etc/inputrc or ~/.inputrc:

     set show-all-if-ambiguous on

This will allow single tab completion as opposed to requiring a double tab.

     set page-completions off

This turns off the use of the internal pager when returning long completion lists.


Usage
=====

Type -[TAB] to start option completion.
Using a single dash (-) will show the short version of the main options.
If the current context also provides options of a match or target,
they will be shown with a double-dash (--) and therefore are easy
to distinguish from each other.

Typing --[TAB] will show only the long form of the available options.

Type [TAB] to complete on anything available at the current context.

Available targets (includes builtin and user-defined chains) and matches are selected by table.
The default table is 'filter'.

Also some targets or matches are not shown if it can be predicted by the protocol,
that the combination is not valid.

To complete named port ranges, enter the : (colon) after the first completed service (port) name,
hit [TAB] again to start completion on the second named port.
The list of services will start from the offset+1 of the first part of the range expression.
No matter if you specified the first part as a numeric value or a name from /etc/services.

Tip:
	To reduce the amount of services listed, specify a protocol before.

Numeric protocol specifications are recognized for the following protocols
(their options are loaded for completion afterwards):

icmp 1
tcp 6
udp 17
dccp 33
esp 50
ah 51
sctp 132



If a comma separated list is to be completed (i.e. state match),
no space will be appended to the currently completed word.
The cursor will be at the end of the current word.
Hit [TAB] and the comma will be appended.
Hit [TAB] again to see the possible completions.


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
