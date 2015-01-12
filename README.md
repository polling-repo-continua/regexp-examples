# regexp-examples
[![Gem Version](https://badge.fury.io/rb/regexp-examples.svg)](http://badge.fury.io/rb/regexp-examples)

Extends the Regexp class with the method: Regexp#examples

This method generates a list of (some\*) strings that will match the given regular expression

\* If the regex has an infinite number of possible srings that match it, such as `/a*b+c{2,}/`,
or a huge number of possible matches, such as `/.\w/`, then only a subset of these will be listed.

## Usage

```ruby
/a*/.examples #=> [''. 'a', 'aa']
/b+/.examples #=> ['b', 'bb']
/this|is|awesome/.examples #=> ['this', 'is', 'awesome']
/foo-.{1,}-bar/.examples #=> ['foo-a-bar', 'foo-b-bar', 'foo-c-bar', 'foo-d-bar', 'foo-e-bar',
  # 'foo-aa-bar', 'foo-bb-bar', 'foo-cc-bar', 'foo-dd-bar', 'foo-ee-bar', 'foo-aaa-bar', 'foo-bbb-bar',
  # 'foo-ccc-bar', 'foo-ddd-bar', 'foo-eee-bar']
/https?:\/\/(www\.)?github\.com/.examples #=> ['http://github.com',
  # 'http://www.github.com', 'https://github.com', 'https://www.github.com']
/(I(N(C(E(P(T(I(O(N)))))))))*/.examples #=> ["", "INCEPTION", "INCEPTIONINCEPTION"]
/what about (backreferences\?) \1/.examples #=> ['what about backreferences? backreferences?']
```

## Supported syntax

* All forms of repeaters (quantifiers), e.g. `/a*/`, `/a+/`, `/a?/`, `/a{1,4}/`, `/a{3,}/`, `a{,2}`
* Boolean "Or" groups, e.g. `/a|b|c/`
* Character sets (inluding ranges and negation!), e.g. `/[abc]/`, `/[A-Z0-9]/`, `/[^a-z]/`
* Escaped characters, e.g. `/\n/`, `/\w/`, `/\D/` (and so on...)
* Capture groups, including named groups and backreferences(!!), e.g. `/(this|that) \1/` `/(?<name>foo) \k<name>/`
* Non-capture groups, e.g. `/(?:foo)/`
* **Arbitrarily complex combinations of all the above!**

## Not-Yet-Supported syntax

I plan to add the following features to the gem (in order of most -> least likely), but have not yet got round to it:

* Throw exceptions if illegal syntax (see below) is used
* POSIX bracket expressions, e.g. `/[[:alnum:]]/`, `/[[:space:]]/`
* Options, e.g. `/pattern/i`, `/foo.*bar/m`
* Unicode characters, e.g. `/\p{L}/`, `/\p{Arabic}/`

## Impossible features ("illegal syntax")

The following features in the regex language can never be properly implemented into this gem because, put simply, they are not technically "regular"!
If you'd like to understand this in more detail, there are many good blog posts out on the internet. The [wikipedia entry](http://en.wikipedia.org/wiki/Regular_expression)'s not bad either.

* Lookarounds, e.g. `/foo(?=bar)/`, `/(?<!foo)bar/`
* Anchors, e.g. `/\bword\b/`, `/line1\n^line2/` (although a special case could perhaps be made to allow `\A`, `^`, `\z` and `$` at the beginning/end of the pattern)

(Note: Backreferences are not really "regular" either, but I got these to work with a bit of hackery!)

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'regexp-examples'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install regexp-examples

## Contributing

1. Fork it ( https://github.com/[my-github-username]/regexp-examples/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
6. Don't forget to add tests!!
