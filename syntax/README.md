##Color defines
When you are preparing the syntax files, you must want to distinguish the colors of each type. There are a couple of built-in colors in EverEdit.

**Note:** The value of each color should be defined in Themes.

<table>
  <tr>
		<th>Value</th>
		<th>Description</th>
	</tr>
	<tr>
		<td><code>COLOR_DEFAULT</code></td>
		<td>Normal text without any syntax state</td>
	</tr>
	<tr>
		<td><code>COLOR_COMMENT1</code></td>
		<td>Single line comment, example:<code>//</code> in C/C++</td>
	</tr>
	<tr>
		<td><code>COLOR_COMMENT2</code></td>
		<td>Block line comment, example:<code>/*your comment*/</code> in C/C++</td>
	</tr>
	<tr>
		<td><code>COLOR_STRING1</code></td>
		<td>Single quoted string, example:<code>'your single quoted string'</code> in PHP</td>
	</tr>
	<tr>
		<td><code>COLOR_STRING2</code></td>
		<td>Double quoted string, example:<code>"your double quoted string"</code> in PHP</td>
	</tr>
	<tr>
		<td><code>COLOR_TAG</code></td>
		<td>Use for HTML/XML tag match, EverEdit could find the matched tag automatically</td>
	</tr>
	<tr>
		<td><code>COLOR_MACRO</code></td>
		<td>Macro define, example:<code>#define/Import/Range VALUE 100</code> in C/C++/C#/Java</br></td>
	</tr>
	<tr>
		<td><code>COLOR_URL</code></td>
		<td>URL, example:<code>http://www.everedit.net</code></td>
	</tr>
	<tr>
		<td><code>COLOR_EMAIL</code></td>
		<td>Email address, example:<code>support@everedit.net</code></td>
	</tr>
	<tr>
		<td><code>COLOR_NUMBER</code></td>
		<td>Numbers, example:<code>10,0x20,1.35</code></td>
	</tr>
	<tr>
		<td><code>COLOR_FOUND</code></td>
		<td>When you search something, EverEdit will use this state to draw the found text</td>
	</tr>
	<tr>
		<td><code>COLOR_PAIR</code></td>
		<td>Paired strings, expample:<code>()[]{}<>""''</code></td>
	</tr>
	<tr>
		<td><code>COLOR_FUNCTION</code></td>
		<td>Function name</td>
	</tr>
	<tr>
		<td><code>COLOR_VAR</code></td>
		<td>Variables</td>
	</tr>
	<tr>
		<td><code>COLOR_SUBLAN</code></td>
		<td>Sub-language, example:<code><script></script></code> in HTML</td>
	</tr>
	<tr>
		<td><code>COLOR_OPERATOR</code></td>
		<td>Operators, example:<code>+-*/ ++ --</code></td>
	</tr>
	<tr>
		<td><code>COLOR_WORD1</code></td>
		<td>Key word 1, built-in keywords. example:<code>int bool double do while</code> in C/C++/Java</td>
	</tr>
	<tr>
		<td><code>COLOR_WORD2</code></td>
		<td>Key word 2, framework keywords. example:<code>CString CMap CArray</code> in MFC</td>
	</tr>
	<tr>
		<td><code>COLOR_WORD3</code></td>
		<td>Key word 3</td>
	</tr>
	<tr>
		<td><code>COLOR_WORD4</code></td>
		<td>Key word 4</td>
	</tr>
	<tr>
		<td>
		<code>COLOR_HIGHLIGHT1</code>~<code>COLOR_HIGHLIGHT8</code>
		</td>
		<td>User defined highlight, used for custom marker</td>
	</tr>
	<tr>
		<td><code>COLOR_IGNORE</code></td>
		<td>The foreground color of text will be same as the background. It can only be displayed on selecting</td>
	</tr>
	<tr>
		<td><code>COLOR_CONCEAL</code></td>
		<td>The text will not be displayed expect activating or selecting this line</td>
	</tr>
</table>

##Parser
You should use this class to create syntax parsers, define some rules to describe your specific file type, and then EverEdit could draw the text according to your rules.

```c++
//properties
string Name;//get,set
SyntaxRegion  DefaultRegion;//read-only

//functions
bool  AddSnippet(string strPathName);
void  AddItem(SyntaxItem item);//Add a SyntaxItem to default region
void  AddWord(WordItem item);//Add a WordItem to default region
void  AddRegion(SyntaxRegion child);
SyntaxRegion  CopyRegion(SyntaxRegion copyFrom);
SyntaxItem  CreateItem(int state, string strMatch, bool bCase, bool bToRight=false);
WordItem  CreateWord(int state, string strMatch, bool bCase, string strDelimiters="");
SyntaxRegion  CreateRegion( int state, string strBegin, string strEnd, bool bCase, bool bToRight=false );
SyntaxRegion  CreateStringRegion( int state, string strChar, char chEscape, bool mline );

void  FoldText(string strText, bool bCase);
void  UnfoldText(string strText, bool bCase);
void  IndentText(string strText, bool bCase);
void  UnindentText(string strText, bool bCase);
void  SetPairs(string strText);
void  CommentBlock(string strOn, string strOff);
void  CommentLine(string strText);
```

###AddSnippet(string strPathName)
Add a snippet file to default region. **strPathName** is the file name which locates at **syntax folder**.

example: <code>cpp.AddSnippet "c.snippet"</code>

###AddRegion(SyntaxRegion child)
Add a child region to the default region. Child's parent will be changed to default region after calling this function.

example: <code>cpp.AddRegion myregion</code>

###CopyRegion(SyntaxRegion copyFrom)
One region should have a unique parent, while if you want to add a region to multiple ones, you should make a shadow  first.

<code>Set newRegion=cpp.CopyRegion rLineComment</code>

<code>yourRegion.AddRegion newRegion</code>

###CreateItem(int state, string strMatch, bool bCase, bool bToRight=false)

<code>state</code>:Color

<code>strMatch</code>:Regular expression

<code>bCase</code>:Case sensitive flag

<code>bToRight</code>:(Optional).If this SyntaxItem will be matched at EOL, the right edge will be painted with the background of this item.

###CreateWord(int state, string strMatch, bool bCase, string strDelimiters="")
Create a SyntaxWord. Words should be speraterd by spaces.

example: <code>cpp.CreateWord COLOR_WORD1, "int float double fo while my-data", "-"</code>

<code>state</code>:Color

<code>strMatch</code>:Words, example: <code>void int float while do class</code>

<code>bCase</code>:Case sensitive flag

<code>strDelimiters</code>:(Optional).The charactors in <code>strDelimiters</code> will be treated as a part of Word. example: <code>-#_</code>


###CreateRegion( int state, string strBegin, string strEnd, bool bCase, bool bToRight=false );
Create a SyntaxRegion.

example:<code>cpp.CreateRegion COLOR_COMMENT2, "+/*+", "+*/+", False</code>

<code>html.CreateRegion COLOR_TAG, "<\w+", "+>+", False</code>

<code>state</code>:Color

<code>strBegin</code>:The beginning of this region. If you use plus <code>+</code> at both end and beginning of strBegin, <code>strBegin</code> wil not be treated as a regular expression.

<code>strEnd</code>:The end of this region. If you use plus <code>+</code> at both end and beginning of strEnd, <code>strEnd</code> wil not be treated as a regular expression.

<code>bToRight</code>:(Optional)..If this SyntaxItem was matched at EOL, the right edge will be painted with the background of this item.

##SyntaxWord
SyntaxWord is a collection list which contains your keywords. We can use SyntaxWord to define keyword match, it’s faster than regular expression match. Besides, there is a useful property of SyntaxWord, AutoCase! The SyntaxWord with AutoCase property could automatic correct your input with right case.

```c++
//properties
bool AutoCase
string Name
```

##SyntaxItem
SyntaxItem was generally be used to describe a regular expression match pattern. A SyntaxItem can be added to multiple SyntaxRegion, but the Color/State was only one which belongs to its parent region. If you want to match a same string, but the Color/State was different, you should create it again.

```c++
//properties
string Name;

//functions
void Capture(int group, int state);
```

##SyntaxRegion
SyntaxRegion is a very important object in EverEdit’s parser. If you create a parser, EverEdit will contain a default region automatically; all other regions will be added to this default region. Region can be added recursively, but one region just has only one parent region. **Do not** try to add a same region to multiple parent regions.

```c++
//properties
string Name;
SyntaxRegion TransRegion;//set-only

//functions
bool AddSnippet( string strPathName );
void AddItem( SyntaxItem item );
void AddWord( WordItem item );
void AddRegion( SyntaxRegion child ;
void FoldText(string strText, bool bCase);
void UnfoldText(string strText, bool bCase);
void CommentBlock(string strOn, string strOff);
void CommentLine(string strText);
void SetPairs(string strText);
void IndentText(string strText, bool bCase);
void UnindentText(string strText, bool bCase);
```

##Create a simple C++ Parser


#To be continue!
