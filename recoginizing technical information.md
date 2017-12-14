# Regular Expressions and Words Used for technical information recognization

In our paper, we propose six features to characterize the existence of various technical information in the description of bug reports. These features include `has_stack`, `has_step`, `has_code`, `has_patch`, `has_testcase` and `has_screenshot`. They characterize whether a bug report contains **stack traces**, **steps to reproduce**, **code samples**, **patches**, **testcase** and **screenshot**, respectively. Reporters can directly report **stack traces**, **steps to reproduce**, **code samples**, **patches** and **testcase** in the textual description of bug reports. Reporters can also attach files in the description of bug reports. Attachments of bug reports contain **stack traces**, **patches**, **testcases** and **screenshots**. We need to recognize these technical information in the form of both *text and attachment*.

**Recognizing Attachments and Extracting their description.** In Bugzilla, each attachment has an ID number and it is corresponding to a paragraph in the description of a bug report. The paragraph consists of two lines. The first line of the paragraph is in the form of "Created attachment #(attachment_id)", which can be easily identified. The second line of the paragraph is the description of the attachment. We use the following regular expression to recognize the paragraphs corresponding to attachments:  

```r'Created attachment [0-9]+'```

Here, for each type of technical information, we propose the corresponding regular expressions and words. Notice that the regular expressions are in Python format using **re** package.

## Stack Traces

1. We recognize three types of stack traces in textual format -- namely Java stack traces, Gdb stack traces and JavaScript stack traces. Regular expressions for recognizing these types of stack traces are as follows:

**Java Stack Traces:** 
* `r'^\!SUBENTRY .*'`
* `r'^\!ENTRY .*'`
* `r'^\!MESSAGE .*'`
* `r'^\!STACK .*'`
* `r'^[\s]*at[\s]+.*[\n]?\([\w]+\.java(:[\d]+)?\)'`
* `r'^[\s]*([\w]+\.)+[\w]+(Exception|Error)(:[\s]+(.*\n)*.*)?'`

**Gdb Stack Traces:**
* `r'#[\d]+[\s]+0x[0-9a-f]{16}[\s]+in[\s]+[\S]+'`
* `r'Thread[\s]+[\d]+[\s]+\(process[\s]+[\d]+\):\n#[\d]+'`

**JavaScript Stack Trace:**
* `r'^[\s]*[\S]+@[\S]+\.js:[\d]+'`

2. We identify whether an attachment contains stack traces by directly checking whether the description of the attachment contains the word `"trace"`.

### Steps to Reproduce

We recognize steps to reproduce using the following regular expressions:
* `r'step[s]? to reproduce'`
* `r'reproduce step[s]?'`
* `r'step[s]? for reproduction'`
* `r're-creation proc'`
* `r'repro[\w]*[\s]+step'`
* `r'to reproduce'`

### Code Samples

We recognize code samples using the following regular expressions:
* `r'^[\s]*(public|private|protected).*class[\s]+[\w]+[\s]'`
* `r'^[\s]*(public|private|protected).*\(.*\)[\n]?'`
* `r'^[\s]*(if|for|while)[\s]*\(.*\)'`
* `r'\{(.*\n)*.*\}'`
* `r'import[\s]+.*;'`

### Patches

1. We recognize patches in textual format using the following regular expression:
* `r'[-]{3}[\s].*\n[\+]{3}[\s].*\n[@]{2}'`
2. We identify whether an attachment contains patches by directly checking whether the description of the attachment contains the word `"patch"` or `"fix"`.

### 
