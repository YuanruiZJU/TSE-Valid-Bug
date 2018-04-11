# Regular Expressions, Phrases and Words Used for technical information recognition

In our paper, we propose six features to characterize the existence of technical information in the description of bug reports. These features include `has-stack`, `has-step`, `has-code`, `has-patch`, `has-testcase` and `has-screenshot`. They characterize whether a bug report contains **stack traces**, **steps to reproduce**, **code samples**, **patches**, **test cases** and **screenshot**, respectively. Reporters can directly report **stack traces**, **steps to reproduce**, **code samples**, **patches** and **test cases** in the textual description of a bug report. Reporters can also attach files in the description of a bug report. Attachments of bug reports contain **stack traces**, **patches**, **test cases** and **screenshots**. We need to recognize the technical information in both the *text* and *attachment* of a bug report.

**Recognizing Attachments and Extracting their Description.** In Bugzilla, each attachment has an ID number and corresponds to a paragraph in the description of a bug report. The paragraph consists of two lines. The first line of the paragraph is in the form of "Created attachment #(attachment_id)", which can be easily identified. The second line of the paragraph is the description of the attachment. We use the following regular expression to recognize the paragraph that correspond to attachments:  

```r'Created attachment [0-9]+'```

Here, for each type of technical information, we propose the corresponding regular expressions and words. Notice that the regular expressions are in Python format using the **re** package. And we ignore case when matching in the description of a bug report.  

## Stack Traces

1. We recognize three types of stack traces in textual format -- namely Java stack traces, GDB stack traces and JavaScript stack traces. Regular expressions for recognizing these types of stack traces are as follows:
* Java Stack Traces: 
```
r'^\!SUBENTRY .*'
r'^\!ENTRY .*'
r'^\!MESSAGE .*'
r'^\!STACK .*'
r'^[\s]*at[\s]+.*[\n]?\([\w]+\.java(:[\d]+)?\)'
r'^[\s]*([\w]+\.)+[\w]+(Exception|Error)(:[\s]+(.*\n)*.*)?'
```

* GDB Stack Traces:
```
r'#[\d]+[\s]+0x[0-9a-f]{16}[\s]+in[\s]+[\S]+'
r'Thread[\s]+[\d]+[\s]+\(process[\s]+[\d]+\):\n#[\d]+'
```

* JavaScript Stack Trace:
```
r'^[\s]*[\S]+@[\S]+\.js:[\d]+'
```

2. We identify whether an attachment contains stack traces by directly checking whether the description of the attachment contains the word `"trace"`.

## Steps to Reproduce

We recognize steps to reproduce using the following regular expressions:
```
r'step[s]? to reproduce'
r'reproduce step[s]?'
r'step[s]? for reproduction'
r're-creation proc'
r'repro[\w]*[\s]+step'
r'to reproduce'
```

## Code Samples

We recognize code samples using the following regular expressions:
```
r'^[\s]*(public|private|protected).*class[\s]+[\w]+[\s]'
r'^[\s]*(public|private|protected).*\(.*\)[\n]?'
r'^[\s]*(if|for|while)[\s]*\(.*\)'
r'\{(.*\n)*.*\}'
r'import[\s]+.*;'
```

## Patches

1. We recognize patches in textual format using the following regular expression:
 ```
 r'[-]{3}[\s].*\n[\+]{3}[\s].*\n[@]{2}'
 ```
2. We identify whether an attachment contains patches by directly checking whether the description of the attachment contains the word `"patch"` or `"fix"`.

## Test Cases

1. We recognize test cases in textual format using the following regular expression:
```
r'test case[s]?:'
r'testcase[s]?:'
r'test case[s]?[\s]+\(.*\):'
r'testcase[s]?[\s]+\(.*\):'
r'^test case[s]?[\s]*\n'
r'^testcase[s]?[\s]*\n'
```
2. We identify whether an attachment contains test cases by checking whether the description of the attachment contains the following words:

```'test case', 'testcase', 'added test', 'test program', 'testing case'```

## Screenshots
We identify whether an attachment contains screenshots by checking whether the description of the attachment contains the following words or phrases:

```'window', 'view', 'picture', 'screenshot', 'visible', 'image', 'png', 'bmp', 'jpg', 'jpeg', 'where to', 'screen shot','yellow', 'rectangle'```
