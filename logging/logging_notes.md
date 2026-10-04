

Test execution logging

LogName:
- time stamp (test start/end)
- test name / test id

Log folder:
- sub-folder named by timestamp+test run ID
- configuration loading

Log maintenance:
- TBD (last 10 executions)
- alternate logs storage

Logs content
- timestamp of each log entries (every line)
- define log level
- device configuration used
- device state prior to interaction
- device state confirmation after each interaction
- equipment rump down 

Log content parsing:
- exceptions
- errors
- DUT connection
- retries
resource allocation

Logs delivery:
- summarize logs with the second part of the team
- Define logic for marking of exact braking point 
- auto create test report

Use the following mermaid diagrams:
- pie
- sequenceDiagrams
- stateDiagrams
- graph LR
- graph TB
- flow chart TD
- gitGraph
- class diagram
- treeView-beta

Code block:
- for simple code use indentation of 4 spaces
- for json indentation of 4 spaces
- for yaml indentation of 2 spaces
- use moderate color scheme in syntax highligting
- use moderate syntax highlighting where possible
- all code samples have tipical/standard codeblock grey background