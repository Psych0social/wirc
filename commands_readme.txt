wIRC 1.2.1 (or higher) Command tips.
By Dave Côté 17 04 2022.
---------------------------------------

In this document, the arguments of the function are presented in square brackets <> but no square brackets must be present in the code.
all of these commands can be used in runtime (chat window) using '/' slash as initiator
and can also be used in a script file (doesn't need the '/' but does support it)
There are also possibilities for invoking nested commands. You can find out more at the end of the document.

ALIAS				DESCRIPTION
=======				=======================
load <filename>			Load a script file in memory and execute it's content.
				<filename> must be located in the wirc\script\ folder (can be tricked).
				Script remain loaded into wirc memory until the application ends
				but it can be updated by recalling the load function.
				Once a file is loaded, it's filename is considered as a function name
				and can be called from anywhere including with '/'.

return <val>			Return <val> value, used by nested script call ($function)
				(look at basetest script for example)

use <filename>			The same as 'load' but will not trigger the function
				Use it to load or update code, usefull to make sure a nested function is ready to work

set <var> <value>		Sets a variable <var> with the value <value>
				Value can contain any caracter exept ; when used in a script

unset <var>			Will clear the content of a variable <var> (sets it to NULL)

msg <nick/#chan> <message>	Send a message
				message can contain any caracter exept ; when used in a script

echo <-a,-s,nick,#> <message>   Print a message on the assignated window
				-a is the Active window
				-s is the status window
				if the first word is a nickname of someone the user have an active query the message will be printed in this window
				if the first word is a channel the user have an active query the message will be printed in this window
				elsewhere the status window is used
				echo does not print carriage and line return, use must use the variable $CRLF to 'end a line'

timer <interval> <command>	create a timed execution (in dev, kinda buggy)
				<interval> is in ms from 1 to 32000)
				<command>

ctcp <nickname> <what>		send a ctcp request to <nickname>
				<what> can be PING VERSION or whatever

me <action>			this, i thnk, is part oh the mirc sub-protocol.
				must be in private chat of channel chat to use

query <nickname>		open a chat window with <nickname>
				will open a window even if the user's not present

quit <reason>			disconnect from server and give a quit reason
				wirc add watermark in the so called reason for application visibility purpose.
				when leaving without this the reason will simply be 'wirc'

nick <newnick>			change user nickname to new value
				subject to server rules.

ignore <nick/address>		Ignore a user, not implemented yet.


admin				server command
cnotice				server command
cprivmsg			server command
die				server command
help				server command
list				server command
luser				server command
names				server command
oper				server command
time				server command
users				server command
userhost			server command
userip				server command
who				server command
whois				server command
whowas				server command
wallops				server command
topic <channel> <topic>		server command
notice <nick/#chan> <message>	server command
server				server command
service 			server command
mode <nick/chan> <mode> <chan>	server command



Nested Summoning
=================================
A nested command is a command that is somewhat treated as a variable
which means that its value will override the invocation name when it is executed
the syntax is the same as for calling a variable but with the addition of parentheses.
ex: $nest()
Some wirc specific commands can be used and can also be created from a script
give the loaded script uses the 'return' command to share a result.
a short example can be found with the included 'basetest' and 'returntest' script

Here is a short list of calls provided by wirc

Summon Name and Argument		Function
============================         ==============
$calc(X + Y)				Do simple math, allowed operator are '+' '-' '*' '/'
					Multiple invocation is possible 
					ex: 	echo $calc(1 + $calc(1 + $calc(1 + 1)));
						<return 4>
					Space characters between the operator and the number are required
					The use if variable is possible inside nested invoke
					ex:	set testVar 1;
						echo $calc($testVar + 1);
						<return 2>

$round(number,decimals)			Round a number to a given amount of decimals
					ex:	echo $round(2.321321321,2);
						<return 2.32>
					Nested invocation or variable can also fit in it
					ex	set testVar 3.3333;
						echo $round($calc($testVar + 0.333),2)
						<return 3.67>