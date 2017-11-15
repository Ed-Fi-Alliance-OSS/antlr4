# ANTLR 4.6 JavaScript runtime, patched by Ed-Fi for MetaEd
This runtime is hosted on the Ed-Fi npm registry as '@edfi/antlr4'.

The ANTLR 4.6 JavaScript runtime has a simple bug causing the lexer to crash under error conditions
caused by invalid keywords, something common to MetaEd IDE usage as end users are typing. This
was originally patched and published to the Ed-Fi npm registry on 11/15/2017, and MetaEd-js
updated to use this patched version.

The ANTLR 4.7.x runtimes fixed this bug, however ANTLR 4.7 generates JavaScript code with bugs
that also cause crashes. These were too complicated to attempt to fix after every language change.

JavaScript ANTLR versions after 4.6 have a different behavior for lexer rules intended to consume
those characters that do not match any keywords. We use a "." lexer rule at the end of the lexer grammar
to catch the first character of these partial keywords, which works fine for error reporting in 4.6.
However, newer ANTLR versions do not report these as errors, but instead see them as valid "undefined" lexer
rules. This seems like a bug. Maybe the 4.6 behavior is also a bug. Who knows.

Attempts to convert these "undefined" rules into errors were unsuccessful, as were attempts to modify the
lexer grammar to see partial keywords as an "UNKNOWN"-style rule using variations of a ".+?" rule,
which apparently is a common solution on non-JavaScript ANTLR runtimes. However, even if this strategy
had worked, it would require embedding JavaScript into the lexer grammar in order to fire an error message on
such a rule. All roads were leading to complex JavaScript code embedding to make partial-keyword
error reporting work, so upgrading to a new version of ANTLR was abandoned in favor of taking on 4.6 maintenance.
