Exception Handling
------------------



### Make sure you answer the discussion first

Learn how to read others' code:

<div style="display: flex; align-items: center;">
  <img src="images/bill-gates.jpg" alt="Bill Gates">
  <blockquote>
    You’ve got to be <b>WILLING TO READ other people’s code</b>, then write your own, then have other people review your code.  <i style="font-size: 0.75em; color: darkblue">~ Bill Gates</i>
  </blockquote>
</div>



### The basic `try-catch` statement

This basic syntax is almost identical to  other languages

```dart [2 | 3 | 4 | 7-8 | 9-14]
try {
  someSafeCode();
  someMoreSafeCode();
  someDANGEROUSCode();
  codeSkippedWhenExceptionOccurs();
  moreCodeSkippedWhenExceptionOccurs();
} catch (exception) { // catch all types of errors
  // Do something with `exception` object
} finally {
  // clean up some resources like files
  // no matter what, even if something goes wrong in catch
  // ensures this block of code is not bypassed by
  // return, continue, break, or other jumping code
}
```



### Handling specific errors

Dart's syntax on handling specific exception types is unique, and pretty too!

```dart [2 | 3 | 4 | 7-8 | 9-10 | 11-13]
try {
  someSafeCode();
  someMoreSafeCode();
  someDANGEROUSCode();
  codeSkippedWhenExceptionOccurs();
  moreCodeSkippedWhenExceptionOccurs();
} on SpecificKindOfException {
  // No object given, 
} on AnotherSpecificException catch (exception)
  // do something w/ AnotherSpecificException object
} finally {
  // clean up
}
```



### Bug Report #1

Crashes on non-numeric head count

<pre style="overflow:hidden">
FREE MASKS FOR THOSE IN NEED, REGISTER HERE
Enter -1 to stop registration.
Enter name of household head: Mike Coo
How many members in your household? two
Unhandled exception:
FormatException: Invalid radix-10 number (at character 1)
two
^

#0      int._throwFormatException (dart:core-patch/integers_patch.dart:133:5)
#1      int._parseRadix (dart:core-patch/integers_patch.dart:144:16)
#2      int._parse (dart:core-patch/integers_patch.dart:102:12)
#3      int.parse (dart:core-patch/integers_patch.dart:65:12)
#4      main (file:///home/michael/Documents/summer2020-se1223/error-handling/foo.dart:24:27)
#5      _startIsolate.<anonymous closure> (dart:isolate-patch/isolate_patch.dart:301:19)
#6      _RawReceivePortImpl._handleMessage (dart:isolate-patch/isolate_patch.dart:168:12)
</pre>


### Preventing the crash

```dart [23-30 | 31-34]
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
  final households = <Household>[];
 
  print('Enter -1 to stop registration.');
  while (true) {
    stdout.write('Enter name of household head: ');
    final head = stdin.readLineSync();
 
    if (head == '!') break;
 
    try {
      stdout.write('How many members in your household? ');
      final headCount = int.parse(stdin.readLineSync());
      totalPersonCount += headCount;
  
      households.add(Household(head, headCount));
      stdout.write("Recipient's registration ref. #: ");
      print('RR #${households.length}');
    } on FormatException catch (exception) {
      print('${exception.source} is not a number.');
      print('Registration failed for ${head}.');
    }
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
  }
}
```

<pre style="font-size: 0.47em">
Enter name of household head: Mike Coo
How many members in your household? two
two is not a number.
Registration failed for Mike Coo.
Enter name of household head: 
</pre>



### Bug Report #2

Crashes when registration ends without any household signing up

<pre style="overflow: hidden">
FREE MASKS FOR THOSE IN NEED, REGISTER HERE
Enter -1 to stop registration.
Enter name of household head: !

*** REGISTRATION HAS BEEN ENDED ***
How many masks do we have so far? 320
Unhandled exception:
IntegerDivisionByZeroException
#0      int.~/ (dart:core-patch/integers.dart:24:7)
#1      main (file:///home/michael/Documents/summer2020-se1223/error-handling/foo.dart:40:37)
#2      _startIsolate.<anonymous closure> (dart:isolate-patch/isolate_patch.dart:301:19)
#3      _RawReceivePortImpl._handleMessage (dart:isolate-patch/isolate_patch.dart:168:12)
</pre>



### adsf

```dart [42-51]
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
  final households = <Household>[];
 
  print('Enter -1 to stop registration.');
  while (true) {
    stdout.write('Enter name of household head: ');
    final head = stdin.readLineSync();
 
    if (head == '!') break;
 
    try {
      stdout.write('How many members in your household? ');
      final headCount = int.parse(stdin.readLineSync());
      totalPersonCount += headCount;
  
      households.add(Household(head, headCount));
      stdout.write("Recipient's registration ref. #: ");
      print('RR #${households.length}');
    } on FormatException catch (exception) {
      print('${exception.source} is not a number.');
      print('Registration failed for ${head}.');
    }
  }
 
  print('\n*** REGISTRATION HAS BEEN ENDED ***');
  stdout.write('How many masks do we have so far? ');
  var totalMasks = 0;
  var masksPerPerson = 0;

  try {
    totalMasks = int.parse(stdin.readLineSync());
    masksPerPerson = totalMasks ~/ totalPersonCount;  
    stdout.write('Each household member will get ');
    print('$masksPerPerson pieces.\n');
  } on FormatException {
    print('Please enter a number for total mask count.');
  } on IntegerDivisionByZeroException {
    print('Just donate the masks, nobody wants it.');
  }
 
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
  }
}
```



### No sign ups, sad...

<pre>
FREE MASKS FOR THOSE IN NEED, REGISTER HERE
Enter -1 to stop registration.
Enter name of household head: !

*** REGISTRATION HAS BEEN ENDED ***
How many masks do we have so far? 500
Just donate the masks, nobody wants it.
GET YOUR MASKS, PLS. STAY 1M FROM EACH OTHER
</pre>

It's not a perfect fix, but hey, no crashes!




### Bug Report #3

Crashes for unrecognized reference numbers

<pre style="overflow: hidden">
ow many members in your household? 1
Recipient's registration ref. #: RR #3
Enter name of household head: !

*** REGISTRATION HAS BEEN ENDED ***
How many masks do we have so far? 490
Each household member will get 61 pieces.

GET YOUR MASKS, PLS. STAY 1M FROM EACH OTHER
Enter RR#: 5429
Unhandled exception:
RangeError (index): Invalid value: Not in range 0..2, inclusive: 5428
#0      List.[] (dart:core-patch/growable_array.dart:146:60)
#1      main (file:///home/michael/Documents/summer2020-se1223/error-handling/foo.dart:60:33)
#2      _startIsolate.<anonymous closure> (dart:isolate-patch/isolate_patch.dart:301:19)
#3      _RawReceivePortImpl._handleMessage (dart:isolate-patch/isolate_patch.dart:168:12)
</pre>