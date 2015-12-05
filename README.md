# cxx-regex-preprocessor
Pre-preprocessor for c/c++ source and header files expands definitions into source code.

# this is only a placeholder and has no released implementation YET.

To use these you have seveal options:
1. preprocess and output to an intermediate source file, then compile
2. preprocess and output to temp file and use that during compile
3. use the compilers stdin as file or pipe flag with backquotes for the sourcefile

The regex preprocesser has a handful of options including preoptimization of the regex
and generated code, all of which are turned off by default to allow the compiler to 
make it's own decisions regarding optimization.

Typically cxx-regex-preprocessor usage is like this:

cxx-regex main.cpp   -- this preprocesses main.cpp and outputs to stdout, just use the "-o file" or add ">file" to capture the output.

flag/switch/command options include:
- --help  suage information
- --output  where to send output, defaults to stdout
- --spec=SPEC  what c++11 spec variation you are using

Also include quiet and the other usual suspecs.

To define a regex within the source code file:

##regex NAME REGEXP FLAGS

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

# expansion

```c++
#regex WORD_RE /[a-z]+/i
```
expands to:
```c++
#ifdef WORD_RE
#undef WORD_RE
#undef cxx11re_WORD_RE_source
#undef cxx11re_WORD_RE_test
#undef cxx11re_WORD_RE_search
#undef cxx11re_WORD_RE_replace
#undef cxx11re_WORD_RE_find
#undef cxx11re_WORD_RE_match
#endif
#define cxx11re_WORD_RE_source /[a-z]+/i
void* inline cxx11re_WORD_RE_find(char* subject, char** endp) \
{auto*p=subject,*b,*e; \
  if(!(p=strpbrk(p,"a..zA..Z"))) return 0; \
  e=(b=p)+strspn(b,"a..zA..Z"); \
  if(endp)endp=e; \
  return b;}
#define WORD_RE(a,b,c,d) cxx11re_WORD_RE_##a(b,c,d) \
```
then
```c++
WORD_RE(find endp endp)
```
would expand to:
```c++
cxx11re_WORD_RE_find(endp,endp)
```

# Regex support

# Replacer support


