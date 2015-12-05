# cxx-regex-macros
A regex macro expander, or pre-preprocessor for c/c++ source and header files expands definitions into source code.  Since the macros are expanded to simple inline code the overhead is extremely minimal, think precompiled regex's.

Common use under the standard MIT license.

### this is only a placeholder and has no released implementation YET.

# Install

Clone it, or download and unzip it to your favorite user (~/cxx-regex-macros) directory, cmake it, make it, install it (optional.)

1. cd ~/cxx-regex-macros  # or to whereever you clone'd/unzipped it.
2. cmake .                # don't forget the dot (period, fullstop, etc, etc, etc...)
3. make
4. make test              # optional
5. make install           # also optional, but then you have to setup your bin and LD path stuff
6. make dev-init          # only if you are debugging the pre-preprocessor itself

# Command line / shell usage

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
- --std=SPEC  what spec variation you are using [default: c++11]
- --no-inline turns off inline-ing for the expanded code.

Also includes quiet, verbose and the other usual flags.  Type: "cxx-regex --usage" for details.

# Source and code usage

To define a regex within source code:

  \#regex NAME REGEXP

If you wish to undefine them use "\#regexundef", which cleans up after itself. "\#undef" does NOT:

  \#regexundef NAME

To use instances simply include the identifier NAME with a regex COMMAND:

NAME(COMMAND ...)

Specifically:

usage | description
----- | -----
NAME(test SUBJECT [ENDP])  | test - returns 0 ot 1
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
#undef cxxre_WORD_RE_source
#undef cxxre_WORD_RE_flags
#undef cxxre_WORD_RE_test
#undef cxxre_WORD_RE_search
#undef cxxre_WORD_RE_replace
#undef cxxre_WORD_RE_find
#undef cxxre_WORD_RE_match
#endif
#define cxxre_WORD_RE_source [a-z]+
#define cxxre_WORD_RE_flags i
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
cxxre_WORD_RE_find(endp,endp)
```

# Regex support

# Replacer support

# Giving support
1. send me money (so I can work on this instead of paid projects OR as a paid project)
2. Clone it, mess around a bit, push request qualified new features, create detailed bugs/issues, work on documentation and/or translation, or just think of something you want to contribute (art work, how-to videos/tutorial, prayers, lunch, etc...)
