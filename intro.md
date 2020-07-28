Error Handling
--------------

<small>
  <span style="color: darkblue;">&lt;</span><span style="color: goldenrod;">/&gt;</span>
  <a href="https://github.com/psse-cpu/se-1223-error-handling-slides">
    https://github.com/psse-cpu/se-1223-error-handling-slides
  </a>
</small>

<h4 style="margin-top: 192px; font-size: 0.85em;">
  <span class="course-code">SE 1223</span>
  <span class="course-title">Software Development II</span>
</h4>

<div style="font-size: 0.75em; margin-top: 16px;">
  <b>Richard Michael Coo</b> |

  <a href="https://github.com/myknbani">
    <img style="vertical-align: middle" src="images/github-32px.png" alt="github logo">
  </a>
  <a href="https://twitter.com/myknbani">
    <img style="vertical-align: middle" src="images/twitter-32px.png" alt="twitterlogo">
  </a>
  <span style="vertical-align: middle">@myknbani</span>
</div>



### Outline

* handling exceptions
* custom exception classes
* antipatterns:
  - Exception as Control Flow
  - Error hiding / Exception swallowing
* error handling without try-catch
* regular expressions
  - detecting badly-formatted strings
  - outside error handling
    + capturing groups 
    + find and replace



### Have you encountered this?

![stopped](images/stopped.jpg)

Or maybe your game crashing for some strange reason?  Or some weird error message popup?



### Exceptions

* These errors are formally called **exceptions**
  - they are anomalous or exceptional conditions requiring special processing - during the execution
    of a program.
* Examples:
  - entering "very tall" for `int height`
  - your Flash drive got full while saving that 4K movie
  - submitting your Canvas assignment and the Internet connection suddenly vanishes
  - **DIVIDING AN INTEGER BY ZERO**
  - **THE FAMOUS NULL POINTER EXCEPTION**
    + designed as NoSuchMethodError in Dart



### Exceptions Showcase

Bong Go's Free Masks 😷😷😷

```dart [3-9 | 12-14 | 16-29 | 32-40 | 42-47 | 49-58]
import 'dart:io';
 
class Household {
  String headName;
  int headCount;
  bool masksReceived = false;
 
  Household(this.headName, this.headCount);
}
 
void main() {
  print('FREE MASKS FOR THOSE IN NEED, REGISTER HERE');
  var totalPersonCount = 0;
  final households = &lt;Household&gt;[];
 
  print('Enter -1 to stop registration.');
  while (true) {
    stdout.write('Enter name of household head: ');
    final sendTo = stdin.readLineSync();
 
    if (sendTo == '!') break;
 
    stdout.write('How many members in your household? ');
    final headCount = int.parse(stdin.readLineSync());
    totalPersonCount += headCount;
 
    households.add(Household(sendTo, headCount));
    stdout.write("Recipient's registration ref. #: ");
    print('RR #${households.length}');
  }
 
  print('\n*** REGISTRATION HAS BEEN ENDED ***');
  stdout.write('How many masks do we have so far? ');
  final totalMasks = int.parse(stdin.readLineSync());
  final masksPerPerson = totalMasks ~/ totalPersonCount;
  stdout.write('Each household member will get ');
  print('$masksPerPerson pieces.\n');
 
  print('GET YOUR MASKS, PLS. STAY 1M FROM EACH OTHER');
  var claimCount = 0;
 
  while (claimCount < households.length) {
    stdout.write('Enter RR#: ');
    final refNumber = int.parse(stdin.readLineSync());
 
    final household = households[refNumber - 1];
    print('Request ID for ${household.headName}.');
 
    if (!household.masksReceived) {
      final give = masksPerPerson * household.headCount;
      print('Give $give pcs. to recipient.');
      print('--- Masks donated by Sen. Bong Go ---');
      household.masksReceived = true;
      claimCount++;
    } else {
      print('*** DOUBLE CLAIMING IS PUNISHABLE BY LAW');
    }
  } // end while
} // end main
```



#### Running the code in a perfect world

```text [2-15 | 17-19 | 22-25 | 26-29 | 30-33 | 34-36 | 37-40]
FREE MASKS FOR THOSE IN NEED, REGISTER HERE
Enter -1 to stop registration.
Enter name of household head: Mike Coo
How many members in your household? 5
Recipient's registration ref. #: RR #1
Enter name of household head: Fifi Marie Coo
How many members in your household? 3
Recipient's registration ref. #: RR #2
Enter name of household head: Tenten
How many members in your household? 1
Recipient's registration ref. #: RR #3
Enter name of household head: Uzumaki Boruto
How many members in your household? 4
Recipient's registration ref. #: RR #4
Enter name of household head: !

*** REGISTRATION HAS BEEN ENDED ***
How many masks do we have so far? 490
Each household member will get 37 pieces.

GET YOUR MASKS, PLS. STAY 1M FROM EACH OTHER
Enter RR#: 3
Request ID for Tenten.
Give 37 pcs. to recipient.
--- Masks donated by Sen. Bong Go ---
Enter RR#: 2
Request ID for Fifi Marie Coo.
Give 111 pcs. to recipient.
--- Masks donated by Sen. Bong Go ---
Enter RR#: 4
Request ID for Uzumaki Boruto.
Give 148 pcs. to recipient.
--- Masks donated by Sen. Bong Go ---
Enter RR#: 2
Request ID for Fifi Marie Coo.
*** DOUBLE CLAIMING IS PUNISHABLE BY LAW
Enter RR#: 1
Request ID for Mike Coo.
Give 185 pcs. to recipient.
--- Masks donated by Sen. Bong Go ---
```

* Our world is not perfect, what can go wrong here? 
  - Discuss your findings [here](https://canvas.instructure.com/courses/2109863/discussion_topics/9443000).