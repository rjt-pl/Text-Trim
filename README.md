# NAME

Text::Trim - remove leading and/or trailing whitespace from strings

# VERSION

version 1.04

# SYNOPSIS

```perl
use Text::Trim;

$text = "\timportant data\n";
$data = trim $text;
# now $data contains "important data" and $text is unchanged

# or:
trim $text; # work in-place, $text now contains "important data"

@lines = <STDIN>;
rtrim @lines; # remove trailing whitespace from all lines

# Alternatively:
@lines = rtrim <STDIN>;

# Or even:
while (<STDIN>) {
    trim; # Change $_ in place
    # ...
}
```

# DESCRIPTION

This module provides functions for removing leading and/or trailing whitespace
from strings. It is basically a wrapper around some simple regexes with a
flexible context-based interface.

# EXPORTS

All functions are exported by default.

# CONTEXT HANDLING

## void context

Functions called in void context change their arguments in-place

```perl
trim(@strings); # All strings in @strings are trimmed in-place

ltrim($text);   # remove leading whitespace on $text

rtrim;          # remove trailing whitespace on $_
```

No changes are made to arguments in non-void contexts.

## list context

Values passed in are changed and returned without affecting the originals.

```perl
@result = trim(@strings);    # @strings is unchanged

@result = rtrim;             # @result contains rtrimmed $_

($result) = ltrim(@strings); # like $result = ltrim($strings[0]);
```

## scalar context

As list context but multiple arguments are stringified before being returned.
Single arguments are unaffected.  This means that under these circumstances,
the value of `$"` (`$LIST_SEPARATOR`) is used to join the values. If you
don't want this, make sure you only use single arguments when calling in
scalar context.

```perl
@strings = ("\thello\n", "\tthere\n");
$trimmed = trim(@strings);
# $trimmed = "hello there"

local $" = ', ';
$trimmed = trim(@strings);
# Now $trimmed = "hello, there"

$trimmed = rtrim;
# $trimmed = $_ minus trailing whitespace
```

## Undefined values

If any of the functions are called with undefined values, the behaviour is in
general to pass them through unchanged. When stringifying a list (calling in
scalar context with multiple arguments) undefined elements are excluded, but
if all elements are undefined then the return value is also undefined.

```perl
$foo = trim(undef);        # $foo is undefined
$foo = trim(undef, undef); # $foo is undefined
@foo = trim(undef, undef); # @foo contains 2 undefined values
trim(@foo)                 # @foo still contains 2 undefined values
$foo = trim('', undef);    # $foo is ''
```

# FUNCTIONS

## trim

Removes leading and trailing whitespace from all arguments, or `$_` if none
are provided.

## rtrim 

Like `trim()` but removes only trailing (right) whitespace.

## ltrim

Like `trim()` but removes only leading (left) whitespace.

# UNICODE

Because this module is implemented using Perl regular expressions, it is capable
of recognising and removing unicode whitespace characters (such as non-breaking
spaces) from scalars with the utf8 flag on. See [Encode](https://metacpan.org/pod/Encode) for details about the
utf8 flag.

Note that this only applies in the case of perl versions after 5.8.0 or so.

# SEE ALSO

Brent B. Powers' [String::Strip](https://metacpan.org/pod/String::Strip) performs a similar function in XS.

# AUTHORS

**Matt Lawrence** <mattlaw@cpan.org> - Original author and maintainer

**Ryan Thompson** <rjt@cpan.org> - Co-maintainer, miscellaneous fixes

# SUPPORT

 - [Issue Tracker](https://github.com/rjt-pl/Text-Trim/issues): Bug reports and feature requests
 - [GitHub Repository](https://github.com/rjt-pl/Text-Trim.git)

# ACKNOWLEDGEMENTS

Terrence Brannon <metaperl@gmail.com> for bringing my attention to
[String::Strip](https://metacpan.org/pod/String::Strip) and suggesting documentation changes.

# LICENSE

This program is free software; you can redistribute it
and/or modify it under the same terms as Perl itself.

[http://dev.perl.org/licenses/artistic.html](http://dev.perl.org/licenses/artistic.html)
