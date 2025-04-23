# Advanced Soup 🧑‍🍳

### Encoding
After compiling a collection (be it in csv, txt, or other format) of data, it is important to do a thourough check for encoding errors. Even if you declare utf-8 encoding in the save statement, you may still find characters like this in the output:

![encoding-error-1](_static/encoding-error-1.png)

or a more extreme example, where every character is 'corrupted':

![encoding-error-2](_static/encoding-error-2.png)


In most cases, this is either due to the file being opened with a non utf-8 encoding in your text editor - Visual Studio Code or a sheet editor like Excel, or due to an improper encoding declaration when saving the file in the script.

[Visual Studio Code – File encoding support](https://code.visualstudio.com/docs/editor/codebasics#_file-encoding-support)

[Microsoft Excel – Save a workbook to text format (CSV UTF‑8)](https://support.microsoft.com/en-us/office/save-a-workbook-to-text-format-txt-or-csv-3e9a9d6c-70da-4255-aa28-fcacf1f081e6)

Once you've determined that the file is correctly encoded, you may notice (or be alerted of) the presence of non-text unicode - invisible instructions to compose the text on page. Regex is especially helpful for finding and replacing nontext - but how do you know what to replace?

[Check for non-ASCII characters here, or in your text editor if supported](https://pages.cs.wisc.edu/~markm/ascii.html)

Then, determine the best course of action for replacing it - this likely means contacting your project manager and providing a detailed report on what you've found.

