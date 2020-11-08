# args
A piece of code that demostrates some ideas of cmd-arguments processing.

## Basic ideas and considerations
### Arguments processing
Command-line arguments is a common way to provide information to programm at invocation time.  
I believe that any part of the program that uses command-line arguments should declare this dependency explicitly. That's why I prefer to declare the arguments and their **semantics** with interface/contract and connect other parts of the programm to the implementation of such interface.  
Concrete implementations of "Arguments"-interface represents sources of values for other parts of the program.  

Unix-style arguments includes **options** and **positional arguments**.  
Options represent the named properties and should at least have the short form: single dash ('-') symbol followed by single letter - or the long form: double dash ('--') followed by one or multiple words separated with dashes. Option may also have both forms.  
All options divides to **flags** and **regular options**:
1. Flags - named options that represent some boolean property, for example `-f` or `--force`. Presence of the flag represents its value is `true` and absence represents `false`.  
2. Regular options - named property that is followed by the value, for example `-p /var/run/app.pid` or `--pidfile /var/run/app.pid`.  

Positional arguments represents values those semantics is determined by their position in command-line string, for example `divide 8 2` have the `8` as dividend and the `2` as divider. The real semantis for positional arguments is always hidden from user and may be revealed only with help of documentation. This facts concludes that any programm that uses positional arguments and don't have any documentation is **unusable** because you can't be sure about arguments' semantics.  

It's common practice to stick together multiple short-format options under the single dash, for example `tar -xzf`, while it's still allowed to have separate short-format options or long-format options as well, e.g. `rsync -avu -zb --exclude '*~' samba:samba/ .`  

Some authors uses equal-sign-separated format, e.g. `--pidfile=/var/run/app.pid`, and I totally believe that this approach is preferable, because it's easier to parse and syntax is more explicit.  
Some approaches allow options to have list-like values with implicit format, e.g.
```
bin --files /var/run/app1.pid /var/run/app2.pid
```
I totally disagree, because such format leads to undetermined argument parsing and hides syntax rules - you just can't know where the list for the option ends and where the positional arguments begin.  

Some options and arguments may be incompatible when used together. I don't know for now how to handle such situations. Maybe it's reasonable to use some kind of "CompatibleSet", I'm not sure.

### Help / usage info
Help / usage must be designed as an independent task. Programmer should always have an opportunity to write the help message statically, because anyway author knows better how software should be used and what correct argument combinations exist.  
Any attempt to take away this opportunity from author/programmer leads to restrictions on help message format and content thus reduces programmers freedom of choice.  
But I clearly realize that static help message may become outdated during the application livecycle.  
That's why the final decision what approuach to use: static free-form vs configured-preformatted - is programmer's responsibility.
