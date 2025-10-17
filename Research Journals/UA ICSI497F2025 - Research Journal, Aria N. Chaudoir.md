# Fall 2025 Semester GOALS:

* Interpolated Strings

  * C# supports these, but Java doesn’t. The idea is that instead of this:
    var foo = “hello” + myLocation + “ my name is “ + me.name;
    You can do this:
    var foo = $“hello {myLocation}  my name is {me.name}”;
    This is not a lot of work, but we have to think through some of the underlying implementation questions
    (for example – right now, we don’t have an implementation of “toString()” for numbers.

* Optional<type> → type?
  Tran uses Optional all over the place (which is good). But it is … annoying to type it all the time.
  So I would like to implement a shortcut – putting a question mark after a type makes it an optional.
  String? ← Optional<String>
  This might be the easiest thing we do.
* The big one – dynamic memory
  Right now, in Tran, new is mostly implemented as a static instance. That is – we don’t do something like malloc().
  We need to add that. While we are at it, two other (related) features: Reference counting and ENUM

  * Reference counting
    when an object is dynamically allocated, we add a counter to it. Each pointer to the object (reference) increments
    the counter by 1. When you change a pointer to no longer point at the object, we decrement the counter. When we
    decrement the counter to 0, the object is no longer in use and is deallocated (like free() in C).

* ENUM
  Since we are automagically adding a field, let’s add one more – an ENUM, essentially, which tells us what type the
  object is. Why? Well, when we have an interface, we need to know so that we can call the right version of the code!
  Plus, we can do all sorts of things with this later – we could add a lookup table for each object and get the size,
  the offsets to fields, the methods, etc. In C# and Java, that is called reflection.
* OS functionality
  Finally – if we have time (and we probably won’t).
  The way we set up AIR and Tran, anything that is not native Tran code (like print(), for example) is handled by sending
  a message (if you took 412 with me, this will be a familiar concept). The Tran implementation of Print() sends a message
  containing what to print. There is a portion of the interpreter that receives the message and actually prints. We can
  add to and expand that for other things that need access to the underlying operating system (like file system access).

# Week of 08/25/2025 (7 Hours Total)

## 08/25/25 (.5 Hours)

### 1-1:30PM

Jetbrains Rider IDE downloaded.
I Still need to download an addon called TOML as Python Community Edition Addon is dependent on it

#### NEAR FUTURE:

* Read Documentation
* Read the Shank wiki
* Read the Shank and/or Tran source code
* Look at the open issues and think about what you would like to implement
* More Reading

  * LLVM documentation
    Start with: https://llvm.org/docs/tutorial/
    C# Version: https://ubiquitydotnet.github.io/Llvm.NET/articles/Samples/Kaleidoscope.html
    Joe Duffy's blog, starting with the links in this post: https://joeduffyblog.com/2015/11/03/blogging-about-midori/

* research primary syntactic and functional differences between Java and C#



## 08/30/25 (4 Hours)

### 12-1pm

I investigated the addon TOML and discovered it to be a file formatting system for improved human readability. I then reinstalled the Python Community Edition addon as TOML presence was not noticed by the addon.

### 5:30-6:55 PM

I reviewed and consolidated the information I received from Prof. Phipps into a form that is easier to
navigate in the form of this Journals pretext (Fall 2025 Semester Goals). I also wrote entries for the
first day I truly initiated the project on my end. I also contacted Phipps to ask when we can expect
to have a meeting and for ways to reach my project peers.

### 7:27 - ## 08:46

Comparing C# to Java syntactically. I also started investigating the keywords for C#

#### NEAR FUTURE:

* Read Documentation
* Read the Shank wikiwikiwikiwiki
* Read the Shank and/or Tran source code
* Look at the open issues and think about what you would like to implement

#### More Reading

* LLVM documentdocumentationdocumentdocumentationationation
* Start with: https://llvm.org/docs/tutorial/
* C# Version: https://ubiquitydotnet.github.io/Llvm.NET/articles/Samples/Kaleidoscope.html
* Joe Duffy's blog, starting with the links in this post: https://joeduffyblog.com/2015/11/03/blogging-about-midori/
* Investigate the means of implementation for the optional feature.



## 08/31/25 (2.5 Hours)

12:24 - 2:45pm
Finished Keyword investigation of C#



# Week of 09/01/2025 (15.25 Hours Total)

## 09/02/25 (4.75 Hours)

### 1:10 -  2:55 pm

Dove into Project code trying to understand it.
Future Aria here now somewhat understands what was happening with the codebase. I didn't realize that the
code was being passed off from lex/parser to not an interpreter but semantic analysis and then eventually AIR

### 3:30 - 4:30 pm

Meeting with Phipps,  ←←←←←←←←←←←←←←←← does this count? ←←←←←←←←←←←←←←←←←←



### 7:55 -10:58 pm

I started by transferring this journal into a non-changing format (.txt) as txt is one of the acceptable formats
for Professor Phipps's researchresearch journals (as I discovered today)
I then went through most of the documentation and started realizing that there were some inconsistencies. Only after a good while of searching did I realize that this was likely due to earlier version documentation not beong updated. This leaves me a little lost when it comes to knowing which is most up to date and what to use as a overarching guide.
I should ask pbipps to see if he knows more.
I also should ask if he wants me to replace the tran file for conditionals or if he'd rather I implement it directly in the langauge via the lexer and parser. (Sounds like it could cause unanticipated errors particularly with my shallow familiarity with the overall codebase).
Finally I should ask what the dependencies are for tran and shank (also are they both considered different projects. what is there relation?). I found some documentation noting it but it was from an area where the documentations relevancy seemed questionable.

## 09/05/25 (5.5 Hours)

### 4:15 - 6:21pm

I started reading through the Lexer File to ensure that an insertion into the code would not break in unexpected ways

### 7:15 -  8:13pm

Finished investigating Lexer and investigating Parser and Visitor modulo next (who does or can call the lexer and parser. What Assumptions can be made)

### 8:58 - 10:30pm

Investigating Parser.

### 11:10 - 12:14

I continued Investigating the Parser. I need to rest to fully comprhend what the logic is doing but I believe I found where I need to look to implement the optional feature.
The Following is the Files (and structures / methods) that will need to be modified to implement the Optional feature directly into the compiler:

#### Lexer

* Symbols
  { "?", Token.TokenTypes.QMARK }
  Token.TokenTypes
  QMARK,

#### Parser

* Variable Declaration(s)
* ResolveType
* Boolean? Term? Factor? Will Optionals interact with any values directly based on context (i.e. in boolean context, is present, or in arithmetic context if it is a number retrieve value, if string concatenate, etc...)
  AST/
  OptionalNode.cs
  public class OptionalNode : ExpressionNode{}
  Visitors?

Need updated Variable Declaration EBNF. It seems the current EBNF description over the Variable Declaration is out of date

## 09/06/25 (5 Hours)

### 12:17 - 12:48 PM

I am composing an email to Phipps to get a clearer picture of the context as well as a better understanding of how he want's the optional feature implemented

#### Current Optional Class includes:

* Boolean isPresent() {property, accessor}
* Boolean isEmpty() {property, accessor}
* T item  {property, mutator. Also changes the isPresent and isEmpty (where appropriate))}
* construct() {empty and isPresent and isEmpty reflect that}
* construct (T val) {contains item and isPresent and isEmpty reflect that}
* shared of(T val) : Optional|T| retVal {creates a new optional filled containing val and returns it}
* shared Empty() : Optional|T| retVal {creates a new empty optional and returns it}
* GetOrDie() : T retval {returns item if present. Otherwise it ends the program (System.Exit(1))}
* get(\[T:] handler) {returns item if present}

#### I am wondering if he wants me to keep the functionality the same of if he'd like to change or simplify it.

* Does he want me to incorporate it into semantic analysis, removing optional when an object's presence is guarenteed for an instance?
* Does he know what in semantic analysis would break with the implementation
* Is anything in the Interpreter folder still in use?

### 1:01 - 3:00

I'm almost done collecting all the questions I have. I am also trying to dowload the Clang/LLVM and was struggling due to the introduction page linked in the gitHub being unsafe for my firewall. I then realized that I was on an old version

### 3:29 - 5:25

LLVM and CLANG
I am trying to figure out If I have the proper setup for the download of the Clang/LLVM. It describes the file with aarch64 and msvc.

AARCH64 refers to ARM64 archietecture. As it turns out, my laptop works off of a different archietecture and is incompatible. I tried looking through the llvm documentation for perhaps other versions but as I dug, their llvm.org website was referenced frequently. I have tried connecting to this site with no luck and I am unsure why. I tried turning off my firewall even and I had no luck. I suspect that either the website is down, or my router / modem may be enforcing their own set of safety protocols.

MSVC refers to microsoft visual c++. This is a c++ compiler. I do not know if that means the binary file was compiled by this compiler, or if it means that the compiler is included in the download. It is referring to Visual Studio's Compiler

KEYS Despite knowing that the AARCH64 incompatibility likely meant the file was likely incompatible with my system I noticed that at the top of the page, the files I was trying to download were merely marked as Windows x64. This lead me to believe that perhaps it will work anyway. However, after downloading the .sig file and comparing it with the release keys on their gitHub I noticed that a signature was invalid. According to the gitHub that means my file's integrity is comprimised.

The documentation for LLVM seems to indicate I could compile the code myself which I will attempt now. The data being extracted from the .zip is rather large 1.6GB so far, I'm anticipating up to 1.5 - 2GB based off of other gitHub download options. 1.72 total. I have 40 minutes to wait.

### 8:05 - 8:36 pm

After installation I am looking through the documentation to figure out how to compile the code. From what I found I need Visual Studio as opposed to Visual Studio Code. I am going to install this now. I had to figure out how to make room on my laptop. This is also going to take a while



# Week of 09/08/2025 (7.25 Hours Total)

## 09/11/25 (7.25 Hours)

### 1:59 PM - 2:55 pm (1 HOUR)

I am implementing the changes I sent to Phipps in an email to be approved. So far I have implemented changes for 1a, 1b, Working on 2.

Questions for Meeting:
Variable Declaration - in my project for your class, every variable's type was assigned by it's own typing s.t. parameters were entered:
typeName variableName {',' typeName variableName}

I could be mistaken, but is this the new format, allowing multiple variables of the same type to be declared with one type identifier s.t. parameters look like:
\[typeName variableName {',' variableName}] {',' typeName variableName {',' variableName}}

### 3:30 - 4:30 pm (1 HOUR)

Phipps meeting

### 6:24 - 11:44 pm (5.25 Hours)

As it turns out, I was almost right. There is multi-instance 1 type declarations in tran, just not in parameters

Optionals types to test:
Optionals of primative types
Optionals of Objects without generic subtypes (especially if generics in generics)
Optionals of Objects with generic subtypes
Optionals of Lambdas
Optionals of Loops (since object and ensure it breaks?)

Ask Phipps

* about the previous comment "This is for the case: ..." (obviously get full comment and add context (in get type)). It looks like there may have been a third option but lambdas were dealt with previously. Now it could be that the lambda section was added later and that comment means nothing or it could be something else. I'd like to avoid breaking the code so I am checking if it is something else even if all the tests I run pass.
* about possible syntax for method calls and headers involving pipes. I imagine they are not found outside the parameters and returns but best to be certain.

Review list: -----
In Parser, Methods confirmed implemented for Optional are:
ResolveLambdaType
ResolveType
ResolveTypeConstructor

and by extension (and review) these methods as well:
PeekOverType()

confirmed unrelated:



methods left to investigate:
Parser
Tran
RequiredNewLine
Class
Member
Property
VariableDeclaration
Statements
Statement
MethodDeclaration
MethodBody
Constructor
VariableDeclarations
Interface
MethodHeader
If
BoolExpTerm
GetComparedNode
BoolExpFactor
Disambiguate
VariableReference
Loop
Expression
Term
Factor
ParseBaseExpression
ParseNumber
ParseMethodCall
ParseVariableReference
ParseStringLiteral
ParseCharLiteral
ParseParenthesizedExpression
ParseNewExpression
ParseConstructorParameters
ApplyDotOperator
ApplyComparisonOperator
MethodCallExpression
MethodCall
VariableUsage
GenericTypes
ResolveTypeWrapper
ResolveTypeSimple
ResolveTypeCommit
LambdaDeclaration
ShortMethod
MethodExposure
TestDeclaration
ParseAttribures



# Week of 09/15/25 (10.75)

## 09/19/25 (0.5 Hours)

### 10:30 - 11:00 AM

Meeting with Phipps

#### NOTE:

Phipps mentioned that a great way to test this code would be to make a class for the standard library in Tran that would do the testing for me

## 09/20/25 (8.75 Hours)

### 5:27 PM - 2:15 AM

#### suspected locations to find optional:

declaration / assignment declaration of variable in code block

* as type on right side of assignment (new object)
  declaration of variable as class / object field or property
  generic optionals in class declarations? (extended???)
  declaration of variable in method side of parameters and returns
  declatarion of variables in for loops
  as type in pass in side of paramters (say for passing in a new object)

#### REVIEW LIST for optional syntax:

Check if limited condtional check for var. decl in disambiguate forms infinite loop
Implemented - Lexer, Parser(ResolveLambdaType, ResolveType, ResolveTypeConstructor, PeekOverType,
Confirmed Functional: - ResolveTypeSimple(depends on resolve type), ResolveTypeCommit(depends on resolve type), Tran, VariableDeclaration, VariableDeclarations, RequiredNewLine, VariableReference, LambdaDeclaration, ShortMethod, Statement, Statements, Loop, If, Disambiguate, MethodHeader, Constructor, MethodBody, MethodDeclaration, MethodCallExpression, ParseBaseExpression, ParseNumber, ParseMethodCall, ParseVariableReference, ParseStringLiteral, ParseCharLiteral, ParseParenthesizedExpression, ParseNewExpression, Expression, Term, Factor, ParseConstructorParameters, ApplyDotOperator, ApplyComparisonOperatorIfPresent, BoolExpTerm, BoolExpFactor, ParseAttribures, GenericTypes, MethodExposure, Class, Member, Property, GetComparedNode, MethodCall, Interface, VariableUsage,
ResolveTypeSimple, ResolveTypeCommit, TestDeclaration, ResolveTypeWrapper.

#### Ask Phipps:

I think it would be neat if you could place a "?" after an optional variable reference as a shorthand for calling the accessor for the property isPresent. The potential problem with this is then we would have to ensure that this expression wouldn't be confused with a ternary operator (if we use the syntax I'm familiar with) should it be included Tran. It may not be a problem, perhaps there is some glaring context to easily distinguish them but it's not coming immediately to mind.
Oh wait... the potential inclusion of the ternary operator may be moot anyway. If I can't distinguish a ternary operator from a regular optional declaration without forcing the ternary operator to be a 1 line expression. That would severely impede the utility of the ternary operator. The parser seems to allow assignment for any variable declarations including the parameter list of a method and a lambda definition. A ternary operator and optional declaration could both exist in a method header's parameter list as well as any statement block.

Lets use EBNF to see if we can always distinguish the two in these contexts
TerOp   = BoolExpTerm                                                                                               "?" {If | Loop | MethodCall | Assignment} ":" {If | Loop | MethodCall | Assignment}
OptDecl = (WORD || ( WORD "|" Type \[{ "," Type }] "|" ) || ("\[" \[Type \[{ "," Type }] ":" \[Type \[{ "," Type }] "]")) "?" WORD {"," Type WORD}\["=" Expression]

* When looking before the "?" both the TernOp and the OpDecl could be a single WORD token in each case. Therefore if we wanted to distinguish each case using just the predecessing WORD tokens we would need to do some level of semantic analysis to find if the WORD token is a type or a variable reference. That sounds feasible but out of scope for the parser. I can ask Phipps if it is needed.
* Following the "?"
  an optDecl must have a WORD followed by either a COMMA, EQUALS, or NEWLINE Token
  a TernOp must can be any statement followed by a ":" then another statement. So far as I can tell, the symbol ":" is used in 3 circumstances for Tran at the moment.
  1. following the keywords "accessor" \& "mutator"
  The keyword predecessors cannot occur except following the start of a new block after a property is declared meaning that specific situation won't cause problems.
  2. before the returns list in a method call
  a terinary operator would have it's ":" within the enclosing parenthesis whereas the ":" preceeding the return list of a method header would be outside the parentheses
  3. between the parameters and returns for the right side of a lambda definition header.
  "\[" \[Type WORD]{"," Type WORD} ":" \[Type WORD]{"," Type WORD} "]". Assuming we have the shared predecessor to "?" for both options {WORD}, preceeding the "?" symbol, we almost get a format that wouldn't allow us to distinguish between the expressions. However, because Optional's must be followed by a WORD token, and then (in this context) a COMMA, ":", "=" or "]", there is no overlapping syntax since
  "\["WORD "?" WORD ":"
  must be followed by a variable declaration for the optional type declaration i.e. (WORD WORD) | (WORD"|"\[Type]{","Type}"|") | Lambda. As the Ternary operator cannot match the syntax for a variable declaration following it's ":" we can safetly declare that Terenary Operators can be distinguished from Optional Variable Declarations using the context surrounding the ":" symbol in lambda declarations.
  As non of the present scenarios in Tran including the symbol "?" and ":" would have indistinguishable syntax with the Ternary Operator, implementing it into the language is a feasible choice.

HOWEVER, I started this because I was thinking that it would be nice to shorted the syntax of optionals s.t. optVarRef? → optVarRef.isPresent
This would be problematic since ternary operators could be (and the new optional syntax would be) used as BoolExpressions. As expressions do not terminate with a NEWLINE token, we need to learn which expressions concern us and where they terminate according to their syntax. For BoolExprs, operating and comparison symbols are included. If we don't have any of these operators following the next potential pieces of the expression then they are excluded from the expression. This includes NumberLiterals, VariableReferences, "true","false", StringLiteral, CharacterLiteral, MethodCallExpression, "("Expression")". Even with all this in mind, I do not see a way in which an optionalVar? will be mixed up with a ternary operator since a ternary operator will always have a ":" following after the next statement (which could certainly encompass any expression optional could be a part of as expressions are parts of statements.) Therefore if a ":" is found following the first statement following the "?" symbol, we can be certain it is a ternary operator.

This may have been something Phipps would have realized but doing a loose kinda proof via process of elimination really helped cement in my mind that we could do the new syntax I am proposing as well as the ternary operator without any overlapping syntax.
Attributes... are those equivalent to java's ENUMS
I noticed that several places in parser have potentially full Variable Declarations (type name and value) where I wasn't expecting them, such as the parameters and returns for method headers, as well as for the lambda's. I don't think it's a bad thing necessarily it is simply never mentioned in the wiki so I thought I'd bring it up so I could either add it to the wiki or make the parser more accurate to the intended syntax. I also spent some time considering what I want to do in terms of a stdlib class for testing the implimentation of the optional syntax shortcut I added.

Today I spent a lot of time making sure that my optional wouldn't break the program but at the same time I spent a lot of time looking through the wiki and code to learn a few more details about the capabilities of this language as well as it's intended syntax. I also caught a few non-breaking errors that would have thrown off the syntax. I also adjusted several comments to reflect the current state of the project. Hopefully this makes it easier on other developers working on this in the future.



#### Next:

I put TODOs in the document
I need to do testing on some level.
Check to see if you can find evidence of generics extending interfaces. If not, ask Phipps if this is in the works or if it is intentional

## 09/21/2025 (1.5 hours)

### 8:30pm - 9:54pm

I started by investigating the java documentation to see if there were any important features I felt should be included in the standard library. I am doing this to find a class I can use to ensure that the code I wrote is working correctly.
Off the top of my head there are a few things I can think of:

* stack.tran {I don't use a lot of stacks. I'll ask phipps if it is worth it}
* queue.tran {same as stack}
* ArrayList.tran???
* set.tran
* Hash.tran
* stringOperator.tran
  {convert digits to char equivalents and vice-versa}
  {toUppercase, toLowercase, compareIgnoreCase, compare, isEmpty, length, toCharArray, indexOf, endsIn, startsWith, substring(start?, end?), }
* Math.tran
  {add ScientificNumber subclass as a different object class. Should handle any operations performed on it. Should also be able to convert}
  {change between different base values for numbers, i.e. 70 → (46 base 16) → (106 base ## 08) → (1000110 base 2)}
  {matrix addition / subtraction}
  {det(matrix)}
* LinkedList.tran {add common sorting functions}
* What about streams? I remember you saying something about them but I figured I should ask. They seem like they would be rather helpful for all iterable structures. They may even be exceptionally important in organizing a non-hierarchical file system.
* print.tran
  shared printListHorizontal(List|x|, \[x:string]) {allows you to print a list structure with each element in the list formatted occuring to the lambda passed in}
  shared ListToStringHorizontal(List|x|, \[x:string]) : string result {allows for the same functionality of above, but returns a string rather than printing to console}
  shared printListVertical(List|x|, \[x:string],) {allows you to print a list structure with each element in the list formatted occuring to the lambda passed in}
  shared ListToStringVertical(List|x|, \[x:string],) : string result {allows for the same functionality of above, but returns a string rather than printing to console}

# Week of 09/22/25 (7.5 Hours total)

## 09/26/25 (1.5 Hours)

### 10:30 - 11:30 pm

research meeting

### 12:40 - 1:05 pm

I spent some time formatting my .txt research journal into an .md format for easier readability

## 09/27/2025 (6 Hours)

### 8:01pm -2:00am

Phipps likes my idea of using optVariableName? (IDENTIFIER QMARK) as a shorthand for optVariableName.isPresent

#### Assessing TODO1

Before testing the question mark syntax I implemented, I'm returning to the TODO comments I left for potentially problematic code segments I
found. Just now I found a method called VariableDeclarations that references a global variable a lot to get a type for several in-line variable
declarations. I found it problematic because afterwards it doesn't set this global varaible to null meaning it is somewhat fragile
in nature. I should check 2 main things. 1. If I change it so that this variable is set to null at the end of the VariableDeclarations
(this is a seperate method that calls VaraibleDeclaration) will it fix this problem or only part of it. My concern is where this is called.
If, for example VariableDeclarations is only called in parameters and returns but not for code blocks, then this global referencing type could
still be problematic.
What I need to Check:

* Where is VariableDeclarations Called (check Globals, Parameters, Returns, and Code Blocks) used

  * Where is VariableDeclaration Called. Is it only VariableDeclarations?

* Is the global variable LastVariableNode used anywhere outside the VariableDeclaration method

  * Will setting it to null break anything?
  * Could it get set to null in the midst of a VariableDeclaration calling another method?
  * Is this already making the code fragile? (what if someone calls another method that has it's own in-line variable declarations and the parser
    overwrites the variable and the following declared inline variable after the method call gets assigned the wrong type)

I suspect that encapsulation could help with this issue, but I'm not yet sure how to approach this without potentially breaking the
codebase... if this effects someone else's project (wasn't someone working on generics). If it is not already the case, VariableDeclarations
should be called in all Variable Declaration environments and it should pass in the type of LastVariableNode to avoid any variables being assigned
to a type different from what the user intended.

#### Results:

Well, I am reminded of a complexity that is still new to me. Properties. This complicates everything. I think what I said still holds true,
but it needs to be managed differently. First, I should ask phipps if he want's to allow inline properties, and if not if he wants to allow
inline globals. If not, well that complicates things. If so, that also complicates things.
I think this could be resolved by passing an otherwise defaulted boolean value to VariableDeclaration called isGlobal. When it is true,
VariableDeclaration can avoid looking for inline variables (should Phipps not want inline globals).

VariableDeclarationNode referenced only by VariableDeclaration and VariableDeclarations(I set it to null at the end of this method)
VariableDelcaration called on by VariableDeclarations
VariableDeclarations used for:

* Parameters of constructor declaration
* Parameters and Returns of methodheader (declaration)
* Parameters and Returns of LambdaDeclaration

##### Problematic:

VariableDeclaration called on by Member(i.e. globals) and several times by MethodBody (seems to use the same logic as VariableDeclarations but is used seperately)

#### Assessing TODO2:

I was mixing up the terms Expression and Statement in my head, all good here

#### Testing Optional Now

1. To start I am going to ensure that my changes haven't broken any existing code in the standard library and tests.

I was not exactly successful. All of the tests went through except the BenchMarkTests.cs (NBOdy method). I tried figuring out why, but it got stuck in an infinite loop during the translation section. I won't even begin to claim that I understand what happened. I tried debugging it but I don't really understand what is happening with all the translation parts of the program and trying to would take a lot of time. I could be mistaken but I think this may have been the case before I even touched the code so I am guessing this is an ongoing issue being worked on by someone else.

2\. Now I will try changing some of the optional declarations to the new format in the standard library. If it breaks anything I will see it in the currently passing tests since the standard library is processed with every execution.

a. I started by changing an Array2D in the parameter for MatrixMultiply1() in Math.tran

I checked in the debugger and it worked as intended!

b. I changed another Array2D|WholeNumber| in the same method but this time in the method body. It is also immediately calling a method, Empty(), on this optional object, i.e. Array2D|WholeNumber|?.Empty()

This also worked but I got confused for a while. I kept looking at the "left" field of the MethodCallStatementNode and finding that the Type was contained within several layers of objects and was listed as Optional|Array2D|WholeNumber|| at the bottom layer (with no Array2D as a Type Parameter). It was only after realizing that this was after SemanticAnalysis that I thought to check if that was what was causing it. Turns out, that was what was causing this and checking before it with debugging did give me my expected results. It's moments like this that remind me I need to step back more often.
c. I tested it in LinkedList.tran as a field.
It worked as intended
d. I still need to test it in a constructor, interface, return, and regular method body variable declaration and assignment
I am doing this in a separate test file.
!!! I was not able to finish this testing due to the fact that my very small hard drive ran out of space again. I spent a good deal of time fixing this issue but forgot to stop counting the active working time when I did. I think I spent a good hour on this so I'll just remove it from the time.

Note to self:

I'm reaffirming that, as in 311, Loops and If blocks cannot contain variable declarations

#### AskPhipps

* I found a part of the code that seems fragile because it uses a global variable to hold the type for inline variable declarations. When digging through
  a bit of the complexity it occured to me that Properties could really complicate this issue. **My Question is**  : Would you prefer to have or
  not have inline declarations in the global variables and and if you want inline declarations in the global scope, would you want inline properties?
  If so, would these properties only be able to have basic accessors and mutators? I'm assuming so, otherwise they would all have to be defined seperately
  (if we went by order this would still be hard to read as a programmer). Alternativelyt we could have the same definition for the accessors and/or mutators
  for the properties declared together (this is reminiscent of generics to me). I feel this may be too niche a use
  case if we don't introduce it well in the documentation. It also may be too niche a use, causing more confusion to those learning the syntax that it is worth.

#### Next
I think I may still need to set the Optional Type isGeneric to true

# Week of 09/29/25 (12.75 Hours Total)

## 10/03/25 (1 Hour)

### 10:30 - 11:30 am 
Phipps meeting

* I discussed how he wants properties implemented and he said that he would be alright with several inline properties with accessors and mutators.
However, any inline properties would only have basic accessors and mutators.
* Also any declared inline variables/properties will all be of the same type.

## 10/05/25 (11.75 Hours )
### 12:00 am - 2:12 am
<p>Soo, there are a whole lot of commits that were just merged into the main branch. I am now looking through all of these to see where 
it affects me for a lot of my code is intertwined with the code that was changed. So far, I am thinking that I should probably just
abondon my current branch and retain the files I changed in said branch (Commented with the changes I made) in a seperate folder. Then
I can see where and if I should implement the way I did previously.<p/>
I should note that I did find the bug I mentioned in the previous entry and it seems it was addressed but I am going to review it to see if the way
it changed affects me.

#### Investigate in Spare Time:
My goodness there has to be a more effecient way to have all the comparisons up at once. I essentially took notes on where I changed the code with enough
context to find it in the new base. Now I can see the main branch changes and compare to see how they will affect eachother.
#### Ask Phipps
* I'm curious as to what you are using attributes for in Tran. It is not defined in the Tran Language Definition of the Wiki so I am guessing it is either new or pooly documented. I looked up attributes for CS in general but google gave me a couple of different definitions. I'm mostly asking because I believe they are untyped but I would prefer to be certain.

I'll finish this when I have more rest. With the updates to the LLVM I probably need to reperform some of my tests

### 16:42 - 21:34
I'm going to reimpliment my intended changes ensuring that they do not mess up the recently modifier parser file

Should definently check:
* <s>LambdaType</s>
* <s>LambdaDeclaration</s>
* <s>Type</s>
* <s>genericType</s>
* <s>Parameters</s>
* <s>Returns</s>
* <s>variableDeclaration</s>
  * <s>Globals / Properties (Members) </s>
  * <s>MethodBody variables  </s>

### 22:07 - 2:49
I implemented the optional shorthand but I still need to check/merge the remaining changes I previously made. That was simple, all the bug fixes I was doing were rendered moot from the slight restructuring of Parser.

Now I need to check if it works as intended. I cannot get my tests to run properly. I investigated further and noticed that I got a lot of errors consistently (Array out of bounds) and I would also get stack overflow exceptions.

I am not certain why, but I did find what I believe to be a bug. This bug entered NEWLINE tokens in even for NEWLINE characters that had nothing on them. Maybe this should have been dealt with in the parser, but I didn't see it investigated (then again maybe I looked right past it, probably). I'm still not sure what is causing the errors.

#### NEXT
Implement the shorthand for retrieving isPresent from Optional types.
Make sure my Lexing NEWLINE eater thing didn't mess up the parser

# Week of 10/06/25 (2.5 Hours Total)
## 10/12/25 (2.5 Hours)
### 12:25 - 14:52
So, after submitting my journal to Phipps I got a response that was only the link to (https://github.com/mphipps1/ShankCompiler/issues/31)... seems a bit cryptic to me but I shall pursue it. FUTURE ARIA HERE: I reviewed my earlier notes and I specifically asked this question (face palm). So... my interprestation as it being cryptic is just my foregetfulness.

<s>
Okay so the link points to an issue for his shank compiler, specifically for attributes. This is a somewhat new concept to me because though I have seen them in others' code before I have not used them nor have I really understood them. As such, before today I spent some time figuring out what they do and it seems they are used primarily as a means to communicate with the parser telling it how to process certain sections of code. Okay but what specifically does that have to do with my current issues? My ideas are:
1. Double check that I implemented Parser with metadata tagging correctly.
2. Perhaps he is giving me another thing to work on once I finish my current project.
3. Perhaps there is another piece of the code I am unaware of that would be messed up by my shorthand implementation if it is not properly addressed.
</s>

Okay So I found that in the ParserWithMetaDataLinking file, there was a function defined for all the functions in Parser, except the following:
* Tran()
* RequireNewLine
* MemberDeclarations
* GenerateDefaultAccessor
* GenerateDefaultMutator
* Statements
* MethodBody
* FunctionVariableDeclarations
* GetCompareNode
* ParseConstructorParameters
* ApplyDotOperatorIfPresent
* ApplyComparisonOperatorIfPresent
* VariableUsage
* GenericTypes
* LambdaType
* Optional
* ShortMethod
* ParseAttributes

So what do these fxns have in common.
I think it is that they do not have a return value.
That applies to Tran, GenerateDefaultAccessor, GenerateDefaultMutator...

That is not it. Perhaps it is functions with no return or a return for (lists of) nodes that have already been generated, such as listed nodes?
##### That applies to:
Tran, RequireNewLine, MemberDeclarations, GenerateDefaultAccessor, GenerateDefaultMutator, Statements, MethodBody, FunctionVariableDeclarations, ParseConstructorParameters, ShortMethod, 

##### But not:
* <s>GetCompareNode(called by ApplyComparisonOperatorIfPresent and Expression) </s>
  *  This makes sense for BoolExpFactor for it packages the return for getCompareNode in an optional and is included in ParserW...ing
  * ApplyComparisonOperator doesn't make sense till you realize this is like the same thing through two method calls
* <s>ApplyDotOperatorIfPresent(called by Factor) </s>
  * This tests if '.' is present and calls a method that calls other methods if there is something there to be called. i.e. all the nodes generated are done outside these two functions.
* <s>ApplyComparisonOperatorIfPresent (called by Expression) </s>
  * This Calls a function that generates it's own node
* <s>VariableUsage (called by ApplyDotOperatorIfPresent) /* seems to handle repeated use of dot operator? */ </s>
  * After further investigation this is returning kind of a chain (if present) of method / variable references seperated by the '.'. That is kind of a list and those are handled inside functions that are in the ParserW....ing
* <s>GenericTypes(called by Class and Interface) /* returns a list of simple (i.e. no generics of generics) generic types for classes/interfaces */</s>
  * This doesn't generate any nodes, just a string list so that is probably why it isn't in ParserWi...ing
* <s>LambdaType </s>
  * A lot like GetCompareNode where the node creation is counted as having happened in a function further up the call line (Type in this instance)
* Optional /* I forgot to even check if I should implement it here */
  * This should be handled the same way LambdaType is
* ParseAttributes
  * The program doesn't really interact with attributes directlty so this makes sense

My suspicion is that the functions that require implementation in the ParserWithMetaDataLinking are the functions that attach / form a new Node in the AST.

I'm about to go but best I can tell, the reason my test is failing is NOT because I failed to properly implement it into ParserWithMetaDataLinking. I shouldn't need to touch that best I can tell. It may be related to a typo I may not have noticed due to a slight misunderstanding of the syntax of tran. Afterall the current error reads "After a class declaration, there should be a member, method or constructor". Is my member declaration mistyped? I need to check that.