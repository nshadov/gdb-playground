# GDB Cheat Sheet

## Basics

`run` - start a program
`start` - add breakpoint to main and start

`breakpoint` - set breakpoint
`next` - next instruction
`step` - step instruction
`continue` - continue running program

`shell ls` - shell commands
`print i` - print variable "i"

`bt` - backtrace



## Built in Python interpreter (`python`)

`python print("test")` - single line python
`python
import os
print("PID is %d", os.getpid())
` - multi line python
`python gdb.execute('next')` - run gdb 'next' command from python
`python bp = gdb.Breakpoint('code.c:8')` - create breakpoint "bp" object
`python var_i = gdb.parse_and_eval('i')` - create python variable "var_i" that is
string from evaluated C code (no live update)
`python print(help(gdb))`

### Python Pretty Print

`set print pretty on` - default GDB pretty printer
`call person_prettyprint(&person_struct)` - run code function

- create "pretty_print.py" in Python with "MyPythonPrinterFunction"
- add registration in that file:
```
import gdb.printing
pp = gdb.printing.RegexpCollectionPrettyPrinter('person')
pp.add_printer('person', '^person$', 'MyPythonPrinterFunction')
gdb.printing.register_pretty_printer(gdb.current_objectfile(), pp)
```
- `source pretty_print.py`


## TUI (start `gdb -tui`)

`CTRL+X A` - switch to TUI
`CTRL+X 2` - split view with disassembly/registers at the same time

## Debugging Backwards it time

### Initial check (maybe it's directly readable from coredump)

* `gdb -c core.123 code` - open coredump file
* `print $pc` - print Program Counter
* `print x $pc` - where did it crashed (probably outside code)
* `bt` - back trace

### Record execution (code paths)

* `gdb code` - start debugging
* `record` - record code execution paths
* `continue` - let it work on it's own

* break main - create breakpoint 2
* break exit - create breakpoint 3
* create list of commands to execute when hit breakpoint 2 (main) - start recording
```
command 2
record
continue
end
```
* create list of commands to execute when hit breakpoint 3 (exit) - rerun
```
command 3
run
end
```

* `reverse-stepi` - **step one machine code instruction**

### Watch memory location

* `watch *(long**) 0x7fffffffff1234` - create watchpoint for memory address (ret address for main())
* `watch -p *(long**)$sp` - same as above, but expression
* `reverse-continue`

## References & Attribution

* [CppCon 2016: Greg Law “GDB - A Lot More Than You Knew"](https://www.youtube.com/watch?v=-n9Fkq1e6sg)
