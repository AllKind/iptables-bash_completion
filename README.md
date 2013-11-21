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
Also some values entered by the user are checked for validity and completion will not continue
after an invalid input. All together providing some kind of interactive help.

- Show and complete commands and options.
- Show and complete matches, targets and builtin and/or user-defined chains.
- Dynamically retrieve, show and complete:
	set names, services, protocols, active interfaces, cpu numbers, routing realms,
	user and group names, NFLOG logging groups, tc classes, nfacct names,
	nfct timeout policy names, osf match genre names.
- Show and complete hostnames, ip/network/mac addresses (dynamically and from file).
- Show and complete various arguments for matches and targets (those which are in any way predictable).
  All options and extensions coming with iptables version 1.4.18 are supported.
- Complete on variables and command substitution.


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

ip[6]tables [TAB] to get you started.

The environment variable **_IPT_OPTS_ON_START** controls
what options are shown at the beginning.
Setting it to 'actions' (the default) will list only actions (-A,-I, etc.)
and -t, -m, -j, -h, -v (respectively the long forms if requested).
Any other value will show all options at start.

Generally typing -[TAB]  starts option completion.
Using a single dash (-) will show the short version of the main options.
If the current context also provides options of a match or target,
they will be shown with a double-dash (--) and therefore are easy
to distinguish from each other.

Typing --[TAB] will show only the long form of the available options.

Type [TAB] to complete on anything available at the current context,
i.e interface-, host-, port-, set-names, etc,.

---

Available targets (includes builtin and user-defined chains) and matches are selected by table.
The default table is 'filter'.

Also some targets or matches are not shown if it can be predicted by the protocol,
that the combination is not valid.

---

To complete named port ranges, enter the : (colon) after the first completed service (port) name,
hit [TAB] again to start completion on the second named port.
The list of services will start from the offset+1 of the first part of the range expression.
No matter if you specified the first part as a numeric value or a name from /etc/services.

	Tip: To reduce the amount of services listed, specify a protocol before.

---

Numeric protocol specifications are recognized for the following protocols
(their options are loaded for completion afterwards):

	icmp 1
	tcp 6
	udp 17
	dccp 33
	esp 50
	ah 51
	sctp 132
	mh 135


---

The environment variable **HOSTFILE** controls how hostname completion is performed.
Taking the description from the bash man-page:

	Contains the name of a file in the same format as /etc/hosts that 
	should be read when the shell needs to complete a hostname.
	The list of possible hostname completions may be changed while the shell is running
	the next time hostname completion is attempted after the value is changed,
	bash adds the contents of the new file to the existing list.
	If HOSTFILE is set, but has no value, or does not name a readable file, bash
	attempts to read /etc/hosts to obtain the list of possible hostname completions.
	When HOSTFILE is unset, the hostname list is cleared.


If the bash-completion package is available hostname completion is extended
the following way (description from bash-completion source):

	Helper function for completing _known_hosts.
	This function performs host completion based on ssh config and known_hosts
	files, as well as hostnames reported by avahi-browse if
	COMP_KNOWN_HOSTS_WITH_AVAHI is set to a non-empty value.
	Also hosts from HOSTFILE (compgen -A hostname) are added, unless
	COMP_KNOWN_HOSTS_WITH_HOSTFILE is set to an empty value.


Also if the  *bash-completion* package is not available, **COMP_KNOWN_HOSTS_WITH_HOSTFILE**
is recognized the same way.

If the environment variable **_IPT_COMP_IP_LOCAL** is set to a non-empty value,
ip addresses of the local system are added to the list of possible completions.

If the environment variable **_IPT_COMP_NETWORKS** is set to a non-empty value,
additionally network addresses are taken from /etc/networks
and get added to the list of possible completions.

Also a list of ip addresses can be supplied using the
environment variable **_IPT_IPLIST_FILE**. Which should point to a file containing
an ip address per line. They can be ipv4 and/or ipv6. If iptables is invoked,
only ipv4 addresses are shown, if ip6tables is invoked only ipv6 addresses are shown.

---

The iprange match only completes on addresses taken from the file specified with **_IPT_IPLIST_FILE**.

---

Mac addresses are retrieved depending on the value of the environment variable **_IPT_MAC_COMPL_MODE**.
If it is set to 'file', the variable **_IPT_MACLIST_FILE** is queried for a filename,
which contains a list of mac addresses.
The file should contain one mac address per line.
Empty lines and comments (also after the address) are supported.
Setting it to 'system', mac addresses are fetched from arp cache,
/etc/ethers and the output of `ip link show`.
Setting it to 'both' (the default, if unset), both methods are used.
If _IPT_MAC_COMPL_MODE has any other value, no mac address completion is performed.

---

If a comma separated list is to be completed (i.e. state match),
no space will be appended to the currently completed word.
The cursor will be at the end of the current word.
Hit [TAB] and the comma will be appended.
Hit [TAB] again to see the possible completions.

---

To retrieve the list of genres for the **osf** match the environment variable
**_IPT_OSF_PF_OS** has to point to the pf.os file.

---

If the environment variable **_IPT_VALIDATE_INPUT** is set to a non empty value
validation of users input is disabled.

---

If the environment variable **_DEBUG_NF_COMPLETION** is defined (any value)
debugging information is displayed.


Compatibility
=============

bash v4+ is required.

Tested with iptables v1.4.16.3, v1.4.18 kernel 3.12 on debian squeeze.

Compatibility for future iptables versions cannot be promised, as new options may appear, 
which of course are currently unpredictable.
Old iptables version, which do not have the -S (--list-rules) option are not supported.

The bash-completion (v2.0+) package is highly recommended, though not mandatory.

http://bash-completion.alioth.debian.org/

Some things might not be that reliable or feature rich without it.
Also the colon (if there) is removed from COMP_WORDBREAKS.
This alteration is globally, which might affect other completions,
if they do not take care of it themselves.

The iproute program (ip) is needed to display information about the local system.

Other programs used are:

* ipset - to list the sets
* arp - to retrieve mac addresses
* tc - to get the list of tc classes
* nfacct - to show netfilter accounting names
* nfct - to show netfilters CT target timeout policy names


Availability
============

https://github.com/AllKind/iptables-bash_completion

https://sourceforge.net/projects/ipt-bashcompl/


Bugs
====

Please report bugs!
Either through sf.net, or email.

