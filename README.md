[![Build Status](https://travis-ci.org/beneills/commonsense-gem.svg?branch=master)](https://travis-ci.org/beneills/commonsense-gem)

# Commonsense

This is the specification for the _commonsense_ text format used for resisting [authorship analysis](https://en.wikipedia.org/wiki/Stylometry).  Basically, it should be more difficult to attribute authorship to a conforming text through stylistic means.  This is essential for anonymous publishing.  The canonical implementation of a validator for this spec may be found [here](https://github.com/beneills/commonsense-gem).

We aim to prevent the current-day identification of an author from published text, __and also their future identification__.

## Quick Validation

    $ gem install commonsense
    $ commonsense thomas_paine.txt

## Examples

    Meeting at public square. Power to every person.


    writing idea

    This day I am feeble because all political material is a record but tomorrow I am able to put writing in public as a secret.

## Specification (v1.0)

The reference implementation of this specification may be found [here](https://github.com/beneills/commonsense-gem).  In case of conflict/ambiguity, the implementation should take precedence.

__Note:__ all regular expressions are [Ruby regexps](http://ruby-doc.org/core/Regexp.html) and must match fully (i.e. as if with beginning/end anchors: ^ and $)

### Definitions

+ A _candidate text_ is a UTF-8 encoded text stream.
+ A _nice character_ is _/[a-zA-Z0-9 \.\n]/_
+ A _line_ is a substring of the _candidate text_, maximal subject to not containing the _\n_ character
+ A _sentence character_ is _/[a-zA-Z0-9 ]/_
+ A _word character_ is _/[a-zA-Z0-9]/_
+ A _word_ is a maximal substring of _word characters_
+ The _word list_ is the list of words returned by the [_words_ method of the reference implementation](https://github.com/beneills/commonsense-gem/blob/master/lib/commonsense/basic_english.rb)
+ The _capitalized word list_ is derived from the _word list_, with each word capitalized

### Criteria

A _candidate text_ conforms to the _commonsense_ spec if and only if it meets all the following criteria:

1. __Characters__: The text contains only _nice characters_
2. __Whitespace__: Each line has no beginning/trailing spaces and contains no consecutive space characters
3. __Lines__: Each line is either empty, a _heading_ or a _sentence list_
4. __Headings__: A _heading_ is _/S+/_, where S is any _sentence character_
5. __Sentences__: A _sentence_ is _/S+\./_, where S is any _sentence character_
6. __Sentence Lists__: _sentence lists_ are consecutive _sentences_, separated by a single space character
7. __First Words__: The first _word_ of each sentence should be in the _capitalized word list_
8. __Other Words__: All words not matching (7) should be in the _word list_

## Attack Vectors

Possible vulnerabilities include (with example passing validation):

1. __Semantic__: author betrays themself in meaning of text, e.g. revealing location _("I am in the south")_
2. __Format Artefacts__: common unique formatting between texts, e.g. use of punctuation _("I. Protest. Government.")_
3. __Stylistic__: text style properties, e.g. dialectic _("I like protest government. I like seem political.")_
4. __Metadata__: data outside our control, e.g. publication timestamps _("I protest government.")_

### Mitigation

1. Not in the project scope.
2. We do a pretty good job mitigating against formatting, by completely preventing things such as double-spaced sentences and trailing whitespace.
3. Our severe restrictions on formatting and characters help to a large extent.  Vertical spacing between paragraphs and sentence length are outside our control.
4. Not in the project scope.

## Todo

+ expand word list
+ gain experience of using specification in real-world setting
+ revise (possibly antiquated) wordlist
+ allow something like "conforms to commonsense vX.Z" at end of text
+ investigate stylometric methods
+ run analyses on validated texts

## Further Reading

+ https://en.wikipedia.org/wiki/Stylometry
+ https://books.google.gr/books?id=EBAUdyBN_6kC&pg=PA149


## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/beneills/commonsense-spec
