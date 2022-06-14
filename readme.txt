
About vlrfsasm

1. Purpose
  This software, vlrfsasm, converts text files to a binary file.
  Expected environment is Windows 10 on x64.

2. Installation and Uninstallation
  You can locate this software anywhere.
  This software does not leave anything except the destination file.
  If you would like to uninstall vlrfsasm, delete everything from the directory where this "readme.txt" is.

3. How to use vlrfsasm
  Create either a shortcut or batch file.
  Then edit it to execute vlrfsasm.exe with the path of source and destination files.
  Text source file must be encoded in Utf-8 and suffixed with ".vlra".
  Other file extensions indicate binary source files.
  
  >  vlrfsasm <destination file> <log file> <source file>

4. Example [Under preparation - Coming in the next release]
  An example is in the "example" folder.
  I recommend you to check it before you start on your own.

5. Grammar
  Vlrfsasm uses s-expression.
  >  (function argument0 argument1 ...)
  Each argument is function, parameter, local name, literal or global name.
  Defining functions is an exception in syntax.
  >  {function parameter0 parameter1 ...; localName0 localName1 ...; (content)}
  Repeating function is defined with splitting width and condition for next iteration.
  Results of all iterations are concatenated.
  >  {loopFunction parameter0 splitWidth0 ...; ; (content); (condition)}
  Literal is either 64-bit unsigned number or utf-16BE string without Null.
  Number with prefix % is binary and with $ is hexdecimal number.
  Number can contains symbols to distinguish each digits clearly.
  String is characters between two double-quotes.
  Escape pairs of characters are \", \\, \r and \n.
  Length of string varies on its content and is always multiple of 16.
  >  214 %11010110 $D6
  >  $0022-0061-005C-0022 "\"a\\\""
  Global name must be with prefix @.
  >  @baseAddress
  Comment can be single line or multiple lines.
  >  [comment]

6. Built-in functions
  Function name: size of result
  Arguments shorter than result are extended automatically with 0 on left.

  Concatenate: sum of size of arguments
  >  (' A B ...)

  Add: size of argument A
  >  (+ A B ...)

  Not: same with argument
  >  (! A)

  And: size of argument A
  >  (& A B ...)

  Inclusive or: size of argument A
  >  (| A B ...)

  Exclusive or: size of argument A
  >  (^ A B ...)

  Shift left: (size of argument A) + argument B
  >  (< A B)

  Shift right: max of 0 and ((size of argument A) - argument B)
  >  (> A B)

  If: same with chosen argument
    If A contains no bit set to 1, C is chosen.
    Otherwise B is chosen.
  >  (? A B C)

  Resize: argument B
  >  (: A B)

  Get size: 64 bits
  >  (# A)

  Label: 0 bit
  >  (@ @A)

  Address: 64 bits
  >  (` @A)

  Error: No result
  If this function gets executed, argument A is printed out and vlrfsasm stops. A must be encoded in utf16LE.
  >  (~ A)

  Get range: argument C
    Calculate (: (> A B) C) but faster.
  >  (, A B C)

7. Label
  Labels embedded in (.) are evaluated after each step.
  Vlfsasm repeats converting source file to (.) with value of labels from previous step.
  If no value changes, it stops.

8. Result
  Finally, vlrfsasm creates destination file and writes (.) from the last step in it.

9. License
  Copyright 2021 vlrfsg

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.

10. History
  2022/6/14 v1.2
    [New] Increased value size limit to 4MB
    [New] Mask first three directories in path if its second character is colon (:)
    [New] Function (,)
    [Fix] Complexity of a loop from O(N**2) to O(N) for search and to O(NlogN) for conversion

  2022/3/23 v1.1
    [Fix] Unintended actions around functions (:), (<) and (>)

  2022/1/27 v1.0
    [Fix] All builtin functions are now stable
    [New] The datasheet set of assembly instructions for PIC16F1827

  2021/12/17 v0.7.1
    [Fix] Several functions in common.vlra
    [Fix] Accept files with size just 65536 Bytes

  2021/11/24 v0.7
    [Fix] Fix unintended behaviour of builtin function < (Seriously)

  2021/10/29 v0.6
    [Fix] Fix unintended behaviour of builtin function <

  2021/10/7 v0.5
    [New] Log file exportation
    [New] Year display
    [Change] Extension of source file from ".txt" to ".vlra"

  2021/10/3 v0.4
    [Fix] Wrong stack display for error 7
    [Fix] Time display; issue #2

  2021/9/28 v0.3
    [New] Stable loop
    [New] Examples
    
  2021/9/8 v0.2
    [New] The first release.
