# [x]it! Specification

> This specification document is currently a working draft (RFC).

[x]it! is a plain-text file format for todos and check lists.

The name is supposed to be pronounced like the English word “exit”.

## License

Per [Creative Commons CC0 1.0 Universal](http://creativecommons.org/publicdomain/zero/1.0/), to the extent possible under law, the editors have waived all copyright and related or neighbouring rights to this work. In addition, as of March 2022, the editors have made this specification available under the [Open Web Foundation Agreement 1.0](https://www.openwebfoundation.org/the-agreements/the-owf-1-0-agreements-granted-claims/owfa-1-0).

## Preface

The keywords “MUST”, “MUST NOT”, “SHOULD”, “SHOULD NOT”, and “MAY”
in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## File Format

The file extension SHOULD be `.xit`.

The file encoding MUST be UTF-8.

Newlines MUST be encoded with either `\n` or `\r\n`.
The newline style SHOULD be consistent within a file.

There SHOULD be a newline at the end of the file.

### Item

An *item* is an entry in the file.

It MUST start at the beginning of a line with a *checkbox*.
There MAY follow a *priority* and/or a *description*,
which MUST appear in this order.

*Checkbox*, *priority*, and *description* MUST be separated by one space character (` `) from each other.
(Additional space characters MAY appear.)

The *item* MUST NOT contain any blank lines.
(I.e. lines that only consist of whitespace characters.)

### Checkbox

A *checkbox* represents the *status* of the *item*.

It MUST be a sequence of 3 characters,
where the first character MUST be an opening square bracket (`[`),
and the third character MUST be a closing square bracket (`]`).

The second character determines the *status* of the *item*,
and MUST be either of:

- ` ` (space) for *open*
- `x` (the letter x) for *checked*
- `@` (at) for *ongoing*
- `~` (tilde) for *obsolete*

> #### Example
>
> ```
> [ ] This is an open item
> [x] This is a checked item
> [@] This is an ongoing item
> [~] This is an obsolete item
> ```

### Priority

The *priority* indicates how important the *item* is.

It MUST contain any number of exclamation marks (`!`) and dots (`.`).
The dots MUST appear either before or after the exclamation mark(s),
but not in between them.

The number of exclamation marks MUST be interpreted as equivalent to the level of importance.

The dots are only for visual padding and MUST NOT be attributed any meaning to.

> #### Example
>
> ```
> [ ] ! This is important
> [ ] !! This is more important
> 
> [ ] ..! This is important
> [ ] !!. This is more important
> ```

### Description

The *description* is user-provided text that gives meaning to the *item*.

It MUST start on the same line that the *item* starts on.
It MAY be continued on the subsequent line(s),
where each of them MUST be indented (preceded) by a sequence of four space characters (`    `).

The *description* MAY contain the following tokens:
- One *due date*
- Any number of *tags*

These tokens MUST be surrounded by whitespace or punctuation,
except for such punctuation which the tokens themselves can consist of.

Potential additional *due dates* MUST be disregarded.

> #### Example
>
> ```
> [ ] This description is one line
> [ ] This description continues ...
>     ... on the next line
> ```

### Due Date

A *due date* determines how timely an *item* is.

It MUST be commenced with the character sequence `-> `
(a hypen, a greater-than sign, and a space),
and then followed by a date pattern.

The date pattern MUST be either of:

- `yyyy-mm-dd` or `yyyy/mm/dd` to reference a calendar day
- `yyyy-mm` or `yyyy/mm` to reference a month period
- `yyyy` to reference a year period
- `yyyy-Www` or `yyyy/Www` to reference a week period
- `yyyy-Qq` or `yyyy/Qq` to reference a quarter period

`y` MUST be a digit that denotes the year;
`m` the month;
`d` the day;
`w` the week number (according to ISO8601);
`q` the quarter number.
`W` and `Q` MUST appear literally.

The periodic patterns MUST be treated as equivalent
to the last calendar day within the respective time frame.

The *due date* value MUST be representable by the gregorian calendar.

> #### Example
>
> ```
> [ ] This shall be done until -> 2022-03-31
> [ ] -> 2022-03 Do this throughout March
> [ ] Do this -> 2022-Q2 within the second quarter
> ```

### Tag

*Tags* are annotations for categorising or filtering the data.

A *tag* MUST start with the hash character (`#`),
followed by one or more characters for the *name* of the *tag*.
There MAY follow an equals sign (`=`)
along with one or more characters for the *value* of the *tag*.

*Tag names* MUST be treated as case-insensitive.

The *tag value* MAY be surrounded by a pair of matching quote characters (either `"`, or `'`).

*Tag names* and unquoted *tag values* MUST only contain
letters, digits, the underscore character (`_`),
and/or the hyphen character (`-`).

Quoted *tag values* MAY contain any character,
except for the respective quote itself
or a newline.
If there is no closing quote on the same line,
the *tag value* MUST be treated as absent.

> #### Example
>
> ```
> [ ] This item has a #tag
> [ ] This #item has #multiple #tags!
> [ ] Tags can #have=values
> [ ] Values #can="be quoted"
> ```

### Group

A *group* is any consecutive number of *items*
(without blank line in between),
that MAY be preceded by one *title*.

> #### Example
>
> ```
> [ ] This item and the next one ...
> [ ] ... are grouped
> 
> [ ] This item is its own group
> ```

### Title

The *title* is a single line of text
that MUST NOT start with a whitespace character
or the opening square bracket character `[`.

> #### Example
> 
> ```
> My TODO list
> [ ] Item 1
> [ ] Item 2
> ```
