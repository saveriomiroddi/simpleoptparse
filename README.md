[![Build Status][BS img]](https://travis-ci.org/saveriomiroddi/simpleoptparse)

Simple Option Parser
====================

SOP is a library which acts as frontend to the standard Option Parser library (`optparse`), giving a very convenient format for specifying the arguments, along with automatic help.

SOP is very useful for people who frequently write small scripts (eg. devops) and want to handle options parsing in a compact and automated way.

This is a definition example:

    result = SimpleOptParse.decode_argv(
      ['-s', '--only-scheduled-days',     'Only print scheduled days'                           ],
      ['-d', '--print-defaults TEMPLATE', 'Print the default activities from the named template'],
      'schedule',
      '[weeks]',
      long_help: 'This is the long help! It can span multiple lines.'
    )

which:

- optionally accepts the `-s`/`--only-scheduled-days` switch, interpreting it as boolean,
- optionally accepts the `-d`/`--print-defaults` switch, interpreting it as string,
- requires the `schedule` argument,
- optionally accepts the `weeks` argument,
- automatically adds the `-h` and `--help` switches,
- prints all the options and the long help if the help is invoked,
- prints the help and exits if invalid parameters are passed (eg. too many).

This is a sample result:

    {
      only_scheduled_days: true,
      print_defaults:      'my_defaults',
      schedule:            'schedule.txt',
      weeks:               '3',
    }

This is the corresponding help:

    ARGS: <schedule> [<weeks>]

    Usage: tmpfile [options]
        -s, --only-scheduled-days        Only print scheduled days
        -d, --print-defaults TEMPLATE    Print the default activities from the named template
        -h, --help                       Help

    << Here starts the provided long help... >>

Help
----

**Important:** This library doesn't raise an error on invalid definitions - their behavior is undefined.

Switches:

    ['-x', '--xxx [VALUENAME]'[, description]]

The key will be the long (or, if not present, the short one) version name, as symbol (ie. `:xxx`).
If `VALUENAME` is not specified, the switch is considered a boolean, and evaluates to true/false.

Arguments:

    'aaa'
    '*aaa'
    '[bbb]'/[*bbb]'

The key is the name, as symbol (ie. :aaa). Can be a simple string not in an array.
The star (`*`) indicates varargs; when there are brackets, indicates that the args are optional.
Brackets define optional arguments.
Optional arguments must follow mandatory ones.
Regular arguments and varargs can't be used together.

Switches:

    :long_help
    :input:     for testing purposes; defaults to ARGV
    :output:    for testing purposes; defaults to $stdout. IMPORTANT: if this is not $stdout, the
                :decode_argv will not call :exit, instead, it will return.

The switches `-h` and `--help` are added automatically.

[BS img]: https://travis-ci.org/saveriomiroddi/simpleoptparse.svg?branch=master
