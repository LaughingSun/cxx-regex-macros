# cxx11-regex-preprocessor
Pre-preprocessor for c/c++ source and header files expands definitions into source code.

# this is only a placeholder and has no released implementation YET.

To define a regex within the source code file:

#regex NAME REGEXP FLAGS

To use instances simply include the identifier NAME with a regex COMMAND:

NAME(COMMAND ...)

Specifically:

usage | description
----- | -----
NAME(test SUBJECT)  | test - returns 0 ot 1
NAME(search SUBJECT [ENDP]) | search - returns index of match
NAME(replace SUBJECT REPLACER [ENDP]) | replace - returns subject result
NAME(find SUBJECT [ENDP]) | find - returns match
NAME(match SUBJECT [ENDP]) | match - returns array of matches

tag      | description
-------- | -------
REGEX    | legal regex or extended regex, depending on flags (see regex support)
NAME     | any legal preprocessor macro syntax
SUBJECT  | data to be tested
COMMAND  | test, search, replace, find or match
ENDP     | offset or end pointer containing the end of the match
SUBJECT  | the constant or variable to process must be an array or pointer
REPLACER | replacement text


# examples

Simple example:

```c++
#regex WORD_RE /[a-z]+/i

char text[] = "It is such a perfect day, because I spent it with you."
  , *endp = text, *p;
while { p = WORD_RE(find endp endp) )
  printf( "Found a word, specifically: \"%s\n\n", p );
}
```

# Regex support

# Replacer support


