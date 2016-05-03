
pandocfiltering 0.1
===================

*pandocfiltering* provides constants and functions to be used in tandem with the [pandocfilters] module.  It was written to support the pandoc-[fignos]/[eqnos]/[tablenos] filters but may be of interest to others.  It is available for installation from [pypi].

I am pleased to receive bug reports and feature requests on the project's [Issues tracker].

[pandocfilters]: https://github.com/jgm/pandocfilters
[fignos]: https://github.com/tomduck/pandoc-fignos 
[eqnos]: https://github.com/tomduck/pandoc-eqnos 
[tablenos]: https://github.com/tomduck/pandoc-tablenos
[pypi]: https://pypi.python.org/pypi
[Issues tracker]: https://github.com/tomduck/pandoc-fignos/issues


Constants
---------

### PANDOCVERSION  ###

Pandoc [does not provide] version information in its json syntax tree.  This is needed by filters to provide support for multiple pandoc versions.

[does not provide]: https://github.com/jgm/pandoc/issues/2640


### STDIN/STDOUT/STDERR ###

Pandoc uses UTF-8 for both input and output; so must we.  Python's strings and sys.stdin/stdout/stderr behaviour differ between versions 3 and 4.  Use these instead.


Functions
---------

### quotify(x) ###

Replaces `Quoted` elements with quoted strings.

Pandoc uses the `Quoted` element in its json when `--smart` is enabled.  Output to TeX/pdf automatically triggers `--smart`.

`stringify()` ignores `Quoted` elements.  Use `quotify()` first to replace `Quoted` elements in `x` with quoted strings.  You should provide a deep copy of `x` so that the document is left untouched.

Returns `x`.


### dollarfy(x) ###

Replaces Math elements with a $-enclosed string.

`stringify()` passes through TeX math.  Use `dollarfy(x)` first to replace `Math` elements with math strings set in dollars.  You should provide a deep copy of `x` so that the document is left untouched.

Returns `x`.


### pandocify(s) ###

Returns a representation of the string `s` using pandoc elements.
Like `stringify()`, all formatting is ignored.


### extract_attrs(value, n) ###

Extracts attributes from a `value` list.  `n` is the index where the attributes start.  Extracted elements are set to `None` in the value list.  Returns the attributes in pandoc format.

### repair_refs(value, pandocversion=PANDOCVERSION) ###

Repairs broken references.  Using `-f markdown+autolink_bare_uris` splits braced references like `{@label:id}` at the ':' into `Link` and `Str` elements.  This function replaces the mess with `Cite` and `Str` elements we normally expect.  The updated value is returned.


Decorators
----------

### @filter_null ###

Wraps `func(value, ...)`.  Removes None values from the value list after modified by `func()`.  The filtering is done *in place*.  Returns the results from `func()`.
