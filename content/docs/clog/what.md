---
title: "Clog - What is it?"
---

# What is Clog?

Clog is a filter that colorizes log files.
Have you ever done something like this:

```
$ tail -f /var/log/something.log
```

Then left that running in a terminal while you debug or run some program.
Were you able to spot details as they scrolled by? Depending on the volume of data scrolling by, it can be difficult to spot important information.

Clog is a filter, that you run like this:

```
$ tail -f /var/log/something.log | clog something
```

The \'something\' argument to clog is a section, which refers to a section in `~/.clogrc` with a distinct set of rules.
Those rules list patterns to look for, and actions to take when the pattern is matched by a line.
A typical example might be that you want to highlight any line that contains the pattern \'severe\'. This entry in `~/.clogrc` achieves this:

```
something rule /severe/ --> bold line
```

This rule is in the \'something\' section, the pattern is \'severe\', and the action taken is to embolden the whole line.
Pattern \'severe\' should probably be more restrictive, because it will also match words like \'persevered\', but that is optional.

Full 16- and 256-color support is included, for the colorization of the log entries.
See [Color Specification](/docs/clog/color) for full details.

Instead of coloring the whole line, specifying \'match\' instead will only color the parts of the line that match.

The format of the rules is:

```
<section> rule /<pattern>/ --> <color> <action>
```

The section is simply a way to allow multiple rules sets, so that one `.clogrc` file can serve multiple uses.
The pattern is any supported Standard C Library regular expression.
Action must be one of \'line\', \'match\', \'suppress\' or \'blank\'.

Rules are processed in order, from top to bottom.
This means that a rule defined lower in the rc file gets to apply it\'s color later, and therefore \'on top of\' that of an earlier rule.

Note that there is a default section, called \'default\'. Putting rules in the default section means that no section need be specified on the command line.
Multiple sections may be specified, and the rules are combined.
