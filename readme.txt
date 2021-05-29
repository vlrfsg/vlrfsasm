
About vlrfsasm

1. Purpose
  This software, vlrfsasm, is for writing the machine language x86_64.
  Expected environment is Windows 10 on x64.

2. Installation and Uninstallation
  You can locate this software anywhere.
  This software does not leave anything except the destination file.
  If you would like to uninstall vlrfsasm, delete everything from the directory where this "readme.txt" is.

3. How to use vlrfsasm
  Create either a shortcut or batch file.
  Then edit them to execute vlrfsasm.exe with the path of source and destination files.
  "vlrfsasm <destination file> <source files>"
  

4. Example
  A Hello world example is in hello world folder.
  I recommend you check those files before you start on your own.

5. Grammar
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

6. License
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
