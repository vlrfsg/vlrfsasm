
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
  Then edit them to execute vlrfsasm.exe with the path of source and destination files.
  Text source file must be encoded in Utf-8 and suffixed with ".txt".
  Other file extensions indicate binary source files.
  
  >  vlrfsasm <destination file> <source file>
  

4. Example
  An example is in folder "example".
  I recommend you check it before you start on your own.

5. Grammar
  Vlrfsasm uses s-expression.
  >  (function argument0 argument1 ...)
  Each argument is function, parameter, local name, literal or global name.
  Defining functions is an exception in syntax.
  >  {function parameter0 parameter1 ...; localName0 localName1 ...; (content)}
  Repeating function is defined with splitting width and condition for next iteration.
  Results of all iterations are concatenated.
  >  {loopFunction splitWidth parameter0 ...; ; (content); (condition)}
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
  Concatenate: sum of arguments
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
  Multiply: same with argument A
  B must be within 64 bits or an error occurs.
  >  (* A B)
  Divide: same with argument A
  B must be within 64 bits or an error occurs.
  If B is 0, another error occurs.
  >  (/ A B)
  Modulo: same with argument A
  B must be within 64 bits or an error occurs.
  If B is 0, another error occurs.
  >  (% A B)
  If: same with chosen argument
  If every bit of A is 0, C is chosen.
  Otherwise B is chosen.
  >  (? A B C)
  Resize: argument B
  >  (: A B)
  Get size: 64 bits
  >  (# A)
  Label: No result
  >  (@ @A)
  Address: 64 bits
  >  (` @A)
  Error: No result
  If this function is executed, utf-16 encoded argument A is printed out and vlrfsasm stops.
  >  (; A)

7. Label
  Labels embedded in (.) are evaluated after each step.
  Vlfsasm repeats converting with value of labels from previous step.
  If no value have changed, it stops.

8. Result
  Finally, vlrfsasm creates destination file and writes (.) from the last step in it.
  

/*5. Grammar
  Vlrfsasm has 14 commands.
  Every expression begins a command character, takes 1 to 3 parameters, and ends with a space.
  Parameters are separated by one colon(:).
  Write commands use little endian.
  [Size] is interpreted as 2**[Size]; 0,1,2,3 are 1,2,4,8 respectively.
  Though they are not distinguished below, writing label values will be written AFTER all * commands are processed.
  
    .[Value]:[Size] #Write [Value] at current position and shift the position right next to there.
    ,[Value]:[Size] #Calculate bitwise OR of [Value] and the memory back for [Size] from current position and then write it there.
    -[Value]:[Size] #Subtract [Value] from the memory back for [Size] from current position and then write it there.
    /[R]:[R/M]:[M]  #Write ([R]<<4 | [R/M] | [M]>>2) as 1 byte and shift the position by 1 byte.
    +[M]:[I]:[S]    #Write ([R]<<4 | [M] | [E]>>2) as 1 byte and shift the position by 1 byte.
    *[Name]         #Save current position as label [Name].
    '[Name]:[Size]  #Write at current position, the address of label [Name], relative from the end of wrinting position and shift current position there.
    ?[Name]:[Size]  #Write at current position the absolute address of label [Name] and shift the position right next to there.
    <[Name]:[Size]  #Subtract the address of label [Name] relative from current position from the memory back for [Size] from current position and then write it there.
    @[Address]      #Set [Address] to current position.
    _[FileSize]     #Set the size of destination file.
    =[Name]:[Value] #Define a constant.
    "[Str]:[Size]:[Null] #Write string [Str] as utf-16 in [Size], shifting current position. Add a null if [null] is null.
    >[Size]         #Shift current position till it matches alignment of [Size].
*/

8. License
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
