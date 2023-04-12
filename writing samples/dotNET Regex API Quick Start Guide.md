<details>
<summary><i>Background</i></summary>

.NET's regex API is powerful, but not intuitive if you haven't worked with it before. So, for newcomers to use the API effectively, they'll consistently need to refer to the API's documentation. However, Microsoft's official documentation for this API is verbose and difficult to parse, with essential information hidden amongst rarely-needed details.

I wrote this quick start guide to provide a concise, easily-scannable guide to the most essential features of .NET's regex API. The intended audience for this document is the intermediate-level developer who is familiar with regex, but not with regex in C#.

</details>
</br>

# C# .NET Regex API Quick Start Guide

This document will explain how you can get started using regular expressions (regex) in C# using .NET 7 or above.

Only basic use cases will be covered here. For more advanced use cases not covered by this document, refer to Microsoft’s full documentation [here](https://learn.microsoft.com/en-us/dotnet/api/system.text.regularexpressions.regex?view=net-8.0).

## Contents:
- [Setup](#setup)
- [Define your regex](#define-your-regex)
- [Methods](#methods)

## Setup <a name="setup"></a>

Import the System.Text.RegularExpressions package with the following statement:

```
using System.Text.RegularExpressions;
```

## Define your regex <a name="define-your-regex"></a>

You will implement your regex as a **string of symbols with an @ before the opening “**. Here’s what that looks like:

```
string pattern = @"your pattern here";
```

Define your regex pattern in C# by combining the symbols in the following table:

<table>
  <tr>
   <td><strong>Symbol</strong>
   </td>
   <td><strong>Element</strong>
   </td>
   <td><strong>Examples</strong>
   </td>
  </tr>
  <tr>
   <td><em>character</em>
   </td>
   <td>exact character
   </td>
   <td><code>@"<strong>5</strong>"</code> matches <code>"5"</code></br>
<code>@"<strong>5</strong>"</code> does not match <code>"3"</code>
   </td>
  </tr>
  <tr>
   <td>[<em>characters</em>]
   </td>
   <td>in group of characters
   </td>
   <td><code>@"<strong>[abc]</strong>"</code> matches <code>"b"</code></br>
<code>@"<strong>[abc]</strong>"</code> does not match <code>"d"</code>
   </td>
  </tr>
  <tr>
   <td>[^<em>characters</em>]
   </td>
   <td>not in group of characters
   </td>
   <td><code>@"<strong>[^abc]</strong>"</code> matches <code>"d"</code></br>
<code>@"<strong>[^abc]</strong>"</code> does not match <code>"b"</code>
   </td>
  </tr>
  <tr>
   <td>[<em>character - character</em>]
   </td>
   <td>in range of characters
   </td>
   <td><code>@"<strong>[a-g]</strong>"</code> matches <code>"f"</code></br>
<code>@"<strong>[a-g]</strong>"</code> does not match <code>"h"</code>
   </td>
  </tr>
  <tr>
   <td>\s
   </td>
   <td>whitespace character
   </td>
   <td><code>@"<strong>\s</strong>"</code> matches <code>" "</code></br>
<code>@"<strong>\s</strong>"</code> does not match <code>"h"</code>
   </td>
  </tr>
  <tr>
   <td>\S
   </td>
   <td>non-whitespace character
   </td>
   <td><code>@"<strong>\S</strong>"</code> matches <code>"h"</code></br>
<code>@"<strong>\S</strong>"</code> does not match <code>" "</code>
   </td>
  </tr>
  <tr>
   <td>\d
   </td>
   <td>digit
   </td>
   <td><code>@"<strong>\d</strong>"</code> matches <code>"5"</code></br>
<code>@"<strong>\d</strong>"</code> does not match <code>"a"</code>
   </td>
  </tr>
  <tr>
   <td>.
   </td>
   <td>any character
   </td>
   <td><code>@"<strong>.</strong>"</code> matches <code>"5"</code>, <code>"b"</code>, <code>" "</code>, <code>"?"</code>, etc 
   </td>
  </tr>
  <tr>
   <td>^
   </td>
   <td>beginning of string
   </td>
   <td><code>@"<strong>^</strong>5"</code> matches <code>"53"</code></br>
<code>@"<strong>^</strong>5"</code> does not match <code>"35"</code>
   </td>
  </tr>
  <tr>
   <td>$
   </td>
   <td>end of string
   </td>
   <td><code>@"5<strong>$</strong>"</code> matches <code>"35"</code></br>
<code>@"5<strong>$</strong>"</code> does not match <code>"53"</code>
   </td>
  </tr>
  <tr>
   <td>{<em>n</em>}
   </td>
   <td>previous element occurs <em>n</em> times
   </td>
   <td><code>@"\d<strong>{3}</strong>"</code> matches <code>"531"</code></br>
<code>@"\d<strong>{3}</strong>"</code> does not match <code>"53a"</code>
   </td>
  </tr>
  <tr>
   <td>{<em>n</em>,}
   </td>
   <td>previous element occurs <em>n</em> or more times
   </td>
   <td><code>@"\d<strong>{3,}</strong>"</code> matches <code>"5312"</code> and <code>"531"</code></br>
<code>@"\d<strong>{3,}</strong>"</code> does not match <code>"53"</code>
   </td>
  </tr>
  <tr>
   <td>{<em>n</em>,<em>m</em>}
   </td>
   <td>previous element occurs at least <em>n</em> times, but no more than <em>m</em> times
   </td>
   <td><code>@"^\d<strong>{3,5}</strong>$"</code> matches <code>"53124"</code> and <code>"531"</code></br>
<code>@"^\d<strong>{3,5}</strong>$"</code> does not match <code>"53"</code> or <code>"531246"</code>
   </td>
  </tr>
  <tr>
   <td>*
   </td>
   <td>previous element occurs 0 or more times
   </td>
   <td><code>@"^<strong>a*</strong>b$"</code> matches <code>"aab"</code> and <code>"b"</code></br>
<code>@"^<strong>a*</strong>b$"</code> does not match <code>"ba"</code>
   </td>
  </tr>
  <tr>
   <td>+
   </td>
   <td>previous element occurs 1 or more times
   </td>
   <td><code>@"^<strong>a+</strong>b$"</code> matches <code>"aab"</code> and <code>"aaaab"</code></br>
<code>@"^<strong>a+</strong>b$"</code> does not match <code>"b"</code> or <code>"ba"</code>
   </td>
  </tr>
  <tr>
   <td>?
   </td>
   <td>previous element occurs 0 or 1 times
   </td>
   <td><code>@"^<strong>a?</strong>b$"</code> matches <code>"ab"</code> and <code>"b"</code></br>
<code>@"^<strong>a?</strong>b$"</code> does not match <code>"aab"</code> or <code>"ba"</code>
   </td>
  </tr>
</table>


The above table only lists the most commonly used symbols. For a full list of all C# regex symbols, refer to Microsoft’s full documentation [here](https://learn.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference).


## Methods <a name="methods"></a>

After defining your regex pattern as a string, you can compare it to other strings using the Regex class’s static methods. The most common .NET regex static methods are the following:


<table>
  <tr>
   <td><strong>Method</strong>
   </td>
   <td><strong>Use case</strong>
   </td>
   <td><strong>Example</strong>
   </td>
  </tr>
  <tr>
   <td>IsMatch
   </td>
   <td>Check whether a string matches your regex pattern.
   </td>
   <td>
    <code>string trueInput = "3";</code></br>
    <code>string falseInput = "ab";</code></br>
    <code>string pattern = @"^\d$";</code></br>
    </br>
    <code>bool trueMatch = Regex.IsMatch(trueInput, pattern); // true</code></br>
    <code>bool falseMatch = Regex.IsMatch(falseInput, pattern); // false</code>
   </td>
  </tr>
  <tr>
   <td>Replace
   </td>
   <td>In an input string, replace strings that match your regex pattern with a different string.
   </td>
   <td>
   <code>string input = "abcdefg";</code></br>
   <code>string pattern = @"[bdf]";</code></br>
   <code>string replaceWith = "!";</code></br>
   </br>
   <code>string replaced = Regex.Replace(input, pattern, replaceWith); // "a!c!e!g"</code>
   </td>
  </tr>
  <tr>
   <td>Count
   </td>
   <td>In an input string, count the number of times your regex pattern is matched.
   </td>
   <td><code>string input = "abcdefg";</code>
<p>
<code>string pattern = @"[bdf]";</code>
<p>
<code>int count = Regex.Count(input, pattern); // 3</code>
   </td>
  </tr>
  <tr>
   <td>Split
   </td>
   <td>Split an input string into an array of substrings delimited by your regex pattern.
   </td>
   <td>
    <code>string input = "abcdefg";</code></br>
    <code>string pattern = @"[ce]";</code></br>
    </br>
    <code>string[] substrings = Regex.Split(input, pattern); // ["ab","d","fg"]</code>
   </td>
  </tr>
</table>

