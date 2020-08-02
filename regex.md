Regular Expressions
-------------------

#### _Optional Topic_



### Regular Expressions

* Regex, or Regexp for short
  - Dart calls them [RegExp](https://api.dart.dev/stable/2.9.0/dart-core/RegExp-class.html)
* patterns of text
  - e.g. `*` represents _"whatever"_.txt when deleting all text files
  
```bash
$ ls -lh
total 12K
-rw-r--r-- 1 michael michael   41 Aug  2 23:49 debt.txt
-rw-r--r-- 1 michael michael  470 Aug  3 00:32 demo.dart
-rw-r--r-- 1 michael michael 2.5K Aug  2 22:55 foo.dart
-rw-r--r-- 1 michael michael    0 Aug  3 00:55 miyaw.txt

$ rm -v *.txt
removed 'debt.txt'
removed 'miyaw.txt'
```



### `*.txt` is a pattern

* some text, followed by a dot, then a **t**, **x**, and then a **t**
  - `foolish.txt` matches the pattern
  - `grr.latxt` doesn't match
  - `picture.jpg` doesn't match
  - `assignment.txt` matches
  - `lab.dart.txt` matches
* regular expressions are patterns on steriods
  - what makes these expressions regular?
    + term comes from mathematical concept of the same name
    + if you shift to BSCS 😁, it's discussed in-depth in Automata Theory
    + kidding...don't shift, take masters in CS instead 😁



### Regex crash course

* It's a mini-langauge
  - syntax is similar in **most** languages
  - PCRE-syntax (Perl-compatible regular expressions)
  - can reuse 90% of what you'll learn here in other languages
* Dart-specific info:
  - `String`s and `RegExp`s implement [`Pattern`](https://api.dart.dev/stable/2.9.0/dart-core/Pattern-class.html)
  - in some cases, `'simple'` will do instead of `RegExp('simple')`
* Crash-course, for more details, see [this site](https://www.regular-expressions.info/)
  - it doesn't have Dart-specific info, be resourceful!



### The most basic pattern

* character literals, in sequence
* cat
  + read: **c**, followed by **a**, followed by **t**
    - thank me later!
  + matches 
    - **cat** <span style="color: green">✔</span>
    - **cat**astrophe <span style="color: green">✔</span>
    - con**cat**enate <span style="color: green">✔</span>
  + won't match
    - **C**atanduanes <span style="color: red">❌</span> (regexes case-senstive by default)
    - cartwheel <span style="color: red">❌</span>
    - basic attitude <span style="color: red">❌</span>



### In Dart code

```dart
void main() {
  final regex = RegExp('cat');

  showMatch(regex, 'cat');
  showMatch(regex, 'catastrophe');
  showMatch(regex, 'intoxicate');
  showMatch(regex, 'Catanduanes');
  showMatch(regex, 'basic attendance');
}

void showMatch(RegExp regex, String input) {
  print("$input: ${regex.hasMatch(input)}");
}
```

<pre style="font-size: 0.55em">
cat: true
catastrophe: true
intoxicate: true
Catanduanes: false
basic attendance: false
</pre>



### Scam: 1+1 isn't working

```dart
void main() {
  final regex = RegExp('1+1');

  showMatch(regex, '1+1');
  showMatch(regex, '111111111');
}

void showMatch(RegExp regex, String input) {/* ... */}
```

<pre>
1+1: false
111111111: true
</pre>

* the plus (\+) symbol is a metacharacter <!-- .element style="font-size: 0.9em" -->
  + metadata is data about the data
  + metagame is game about the game
  + \+ means _"one or more"_ (one or more 1s followed by a 1)



### Metacharacters should be escaped to erase special meaning

```dart
void main() {
  final regex = RegExp('1\\+1');

  showMatch(regex, '1+1');
  showMatch(regex, '111111111');
}
```

<pre>
1+1: true
111111111: false
</pre>

* But why two backslashes (`\`)?
  - `\+` is not an escape sequence, and gets interpreted as `+`
  - `print('\+\+');` just prints `++`
  - `\\` is the escape sequence for a single backslash



### Easier life with `r"aw strings"`

* strings prefixed with `r` are raw
  - `\`s don't escape anything
    + `print('c:\notes-in-SE')` yields **c:\notes-in-SE**
  - usually the best practice for languages that has this feature

```dart
void main() {
  final regex = RegExp(r'1\+1');

  showMatch(regex, '1+1');
  showMatch(regex, '111111111');
}
```

<pre>
1+1: true
111111111: false
</pre>



### There are 14 metacharacters

<div style="display: flex">
<ul>
  <li>[ opening square bracket</li>
  <li>{ opening curly bracket</li>
  <li>( opening round bracket</li>
  <li>. the dot</li>
  <li>+ the plus sign</li>
  <li>^ the caret</li>
  <li>| the vertical pipe</li>
</ul>
<ul style="margin-left: 64px">
  <li>] closing square bracket</li>
  <li>} closing curly bracket</li>
  <li>) closing round bracket</li>
  <li>* the asterisk</li>
  <li>? the question mark</li>
  <li>$ the dollar</li>
  <li>\ the backslash</li>
</ul>
</div>



### The two square brackets

* represents a character class
  - everything inside the bracket is an **OR**
  - it represents just **one character**
  - `[aeiou]` means **an** a, e, i, o, or u
  - in the example **g**, followed by **r**, then an **[a OR e]**, then **y**

```dart
void main() {
  showMatch(RegExp(r'gr[ae]y'), 'my gray shirt');
  showMatch(RegExp(r'gr[ae]y'), 'my grey cat');
  showMatch(RegExp(r'gr[ae]y'), 'so graeyish!');
}
```

<pre>
my gray shirt: true
my grey cat: true
so graeyish!: false
</pre>