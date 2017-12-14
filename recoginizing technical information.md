# Regular Expressions and Words Used for technical information recognization

In our paper, we propose six features to characterize the existence of various technical information in the description of bug reports. These features include `has_stack`, `has_step`, `has_code`, `has_patch`, `has_testcase` and `has_screenshot`. They characterize whether a bug report contains **stack traces**, **steps to reproduce**, **code samples**, **patches**, **testcase** and **screenshot**, respectively. Reporters can directly report **stack traces**, **steps to reproduce**, **code samples**, **patches** and **testcase** in the textual description of bug reports. Reporters can also attach files in the description of bug reports. Attachments of bug reports contain **stack traces**, **patches**, **testcases** and **screenshots**. We need to recognize these technical information in the form of both *text and attachment*.

Here, for each type of technical information, we propose the corresponding regular expressions and words. Notice that the regular expressions are in Python format using **re** package.

###Stack Trace

We recognize three types of stack traces in textual format -- namely Java stack traces, Gdb stack traces and JavaScript stack traces. Regular expressions for recognizing these types of stack traces are as follows:

**Java Stack Traces:** 
* r'^\!SUBENTRY .*'
* r'^\!ENTRY .*'
* r'^\!MESSAGE .*'
* r'^\!STACK .*'
* r'^[\s]*at[\s]+.*[\n]?\([\w]+\.java(:[\d]+)?\)'
* r'^[\s]*([\w]+\.)+[\w]+(Exception|Error)(:[\s]+(.*\n)*.*)?' 

**Gdb Stack Traces:**
* r'#[\d]+[\s]+0x[0-9a-f]{16}[\s]+in[\s]+[\S]+'
* r'Thread[\s]+[\d]+[\s]+\(process[\s]+[\d]+\):\n#[\d]+'
