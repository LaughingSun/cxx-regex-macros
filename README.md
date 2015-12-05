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
exands to:
```c++
#ifdef WORD_RE
#undef WORD_RE
#undef cxx11_WORD_RE_source
#undef cxx11_WORD_RE_test
#undef cxx11_WORD_RE_search
#undef cxx11_WORD_RE_replace
#undef cxx11_WORD_RE_find
#undef cxx11_WORD_RE_match
#endif
#define cxx11_WORD_RE_source /[a-z]+/i
void* inline cxx11_WORD_RE_find(char* subject, char** endp) \
{auto*p=subject,*b,*e; \
  if(!(p=strpbrk(p,"a..zA..Z"))) return 0; \
  e=(b=p)+strspn(b,"a..zA..Z"); \
  if(endp)endp=e; \
  return b;}
# endif
#define WORD_RE(a,b,c,d) \
# if #a == "test"
   cxx11_WORD_RE_test(a,b,c)
# elif #a == "search"
   cxx11_WORD_RE_search(a,b,c)
# elif #a == "replace"
   cxx11_WORD_RE_replace(a,b,c)
# elif #a == "find"
   cxx11_WORD_RE_find(a,b,c)
# elif #a == "match"
   cxx11_WORD_RE_match(a,b,c)
# else
#  error Invalid regex command #a
# endif
```

# Regex support

# Replacer support


