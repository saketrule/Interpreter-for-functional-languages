# The_Interpreter

# Table Of Contents
- [Overview](#overview)
- [Overview of Functional Language Constructs](#overview-of-functional-language-constructs)
- [Pretty Printer](#pretty-printer)
- [Lexer](#lexer)
- [Parser](#parser)
- [Template Instantiation Machine](#template-instantiation-machine)

===========================================================================================================================================
## Overview 

This implementation takes some program written in simple Core language and executes it. The Core language forms the basis for buildling Interpreters for high level functional languages such as Haskell whose constructs form the 'front end' of the Interpreter. Our initial focus is on building the back end which consists of constructs in the Core language. 

#### The Interpreter for Haskell would be built in four steps.

- The first step includes understanding syntax and sematics of haskell and knowing how it's different constructs would be handled by  the Interpreter. `CoreProgram` and `CoreExpression` will form the first of the two data types we declare for accepting core programs and expressions as input.
- Secondly, we would be designing a pretty printer to make the output readable and elegant.
- Thirdly, the Lexer would be made which would convert the input Program String into a list of Tokens.
- Fourthly, this list of tokens would be parsed to convert it into a format accepted by the Machine or as defined in the Interpreter.
- Lastly, the parsed tokens would then be evaluated by a graph reducer based on template instantiation.


=======================================================================================================================================
## Overview Of Functional Language Constructs

- The Program is considered to be set of Supercombinator definitions.Each supercombinator is an set of one or more variables evaluating to an expression. Supercombinators can define functions taking any number of arguements. The production rules look like -> 

`program -> sc1 ; sc2 ; .... scn`

`sc -> var var1 var2 ... varn = expr`
- Constructs 
 - **Let**
  
  There are recursive and non recursive let bindings.
  
  ```haskell
  let <...bindigs....> in <expr>
  ```
  ### Example:
  ```haskell
  > let x = 5 in x*x
  === Final value  = 25 ===
  ```
 - **Function Application**
  ```haskell
  function <args>
  ```
  ###Examle:
  ```haskell
  square x = x*x
  ```
 - **Data Constructors** 
  ```haskell
  Pack{tag,arity}
  ```
  Tag represents a specific constructor and arity represents number of arguements taken by the constructor.
  
  ### Example:
  ```haskell
  Data Colour = Red | Green | Blue 
  ```
  This would be represented as:
  ```haskell
  Red = {1,0}
  Green  = {2.0}
  Blue = {3,0}
  ```
 - **Case expressions**
  ```haskell
  case <expr> of
  ```
  ### Example:
  ```haskell
  isGreen colour  = case colour of 
                          <1> -> False;
                          <2> -> True;
                          <3> -> False
  ```
 - **Comments**
  ```haskell
  # A comment begins with a # in core language and continues till the end of line. (No multiline comments)
  ```
 - **Lambda abstractions**
  ```haskell
  square = \x. x*x
  ```
  
== Note == 

Lamda abstractions and case expressions are not evaluated by this machine as they require certain different implementations.

=======================================================================================================================================
## Pretty Printer

The pretty printer makes the ouput more readable. We will be developing our own printer called ``pprint``. The implementation of this part has been skipped to move on to implementing Instantiation machine as early as possible.

=======================================================================================================================================
## Lexer

- The Lexer essentially converts an input string into a list of Tokens. Tokens are nothing but indivdual units of String which cannot be further divided. So `let` ,` =` , `25` ,etc are essentially tokens where `let` is keyword in core language, `=` represents the equal to sign and `25` is just an Integer.   
- So we define a set of keywords and operators to be considered while lexing as:
 
 ```haskell
 keywords :: [String]
 keywords = ["let","letrec","case","in","of","Pack"]
 
 binop :: [String]
 binop = ["=","<",">","<=",">=","-","+","/","*","|","&"]
 ```
 ### Example
 
 ```haskell
 square x = x*x
 main = square 2
 ```
 This will we tokenized as
 
 ```haskell
 ["square","x","=","x","*","x","main","=","square","2"]
 ```
- All the whitespace is ignored while lexing.
- Comments will be ignored while lexing which are identified as beginning with `#`.
- Variables are String beginning with an alphabet followed by alphanumeric characters or underscores.
- Thus lexing converts input program into a list of tokens which is then fed to the parser.

=======================================================================================================================================
## Parser

=======================================================================================================================================
## Template Instantiation Machine

===========================================================================================================================================
