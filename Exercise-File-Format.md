# Lambda Calculator - Exercise File Format

Exercise files created by the instructor are saved as plain-text files, usually with a .txt file extension. You can edit this files with, for instance, the Notepad program on Windows. When using Microsoft Word or another office suite, be sure to save the file as a Text Only-formatted file. When using special or international (non-ASCII) characters, files should be saved in the UTF-8 Unicode character encoding.

An example exercise file is given below.

```
# Lines that begin with a hash are ignored.

Homework 1

constants of type e : a b-c
constants of type <e,t> : P-Q
variables of type e : x-z
variables of type <et> : X Y

exercise semantic types
points per exercise 10
title Semantic Types
directions Give the semantic type of the following lambda-expressions.

c
P(c)
Q(x) V ~Q(x)
Lx.P(x) & Q(x)

exercise lambda conversion
points per exercise 15
title Lambda Conversion
directions After checking that the type of the function and the type of the
directions argument(s) match, simplify the following expressions performing
directions one lambda-conversion at a time.

[Lx.P(x) & Q(x)] (a)
[LxLy.R(a,y) & Q(x)] (a) (b)
[Lx.a] (b)
[LxLx.P(x) -> R(x,c)] (a) (b)
```

## File Format Basics

Blank lines in execise files are ignored, and lines that begin with a hash (#) are also ignored. You can use hashes to store notes in the file that won't be read by the Lambda program, but of course keep in mind that students can view such notes if they open the exercise file in a text editor themselves.

The first (non-ignored) line of every exercise file is the title of the exercise to be shown to the student, and for the instructor's use later when reviewing the students' work. In the example above, this is Homework 1.

Following this line, a line is either a directive to the Lambda program, or it is an exercise.

## Directives

Directives are lines that begin with specially recognized commands. These are explained below.

***

`constants of type ___ : ___`

`variables of type ___ : ___`

These directives specify the typing convention for variables and constants found in the exercises. If none of these directives are present in an exercise file, the default type assignments shown below are used. The first time this directive is used in an exercise file, the defaults are cleared, and the new type assignments apply for all exercises following the directive.

A type can be either:

* an atomic type, which can be any single letter,
* a complex (function) type specified as <T1,T2>, for types Ti, or
* a cartesian-product type, specified as T1\*T2\*T3....
* One-place predicates are typed as complex types. Two-place (or more) predicates are typed as functions from a cartesian-product type to another type, i.e. as <T1\*T2\*T3,T4>. Commas may be omitted from complex types when the types on either side are both atomic.

Examples:

```
constants of type e : a b-c
constants of type <e,t> : P-Q
constants of type <e*e,t> : R S T
variables of type e : x-z
variables of type <et> : X Y
variables of type <'a,'b> : B
```

These directives will always override previous uses of the directives, so you can change the typing conventions throughout the exercise file by issuing this directive again.

The default typing conventions are:

```
constants of type e : a-e
constants of type <e,t> : P-Q
constants of type <e*e,t> : R-S
variables of type e : u-z
variables of type <et> : U-Z
```

***

`points per exercise ___`

This directive specified a numeric point value assigned to each exercise that follows the directive, or just until another `points per exercise ___` directive is found. The points value can be any rational number. It is up to the instructor to ensure that the total points add up to 100 if that is desired. Example:

```
points per exercise 15
```

***

`exercise ___`

This directive begins a group of exercises and specifies the type of exercise in the group. The exercise type must be either `semantic types`, `lambda conversion`, or `tree`. These exercises are described below. The `exercise ___` directive must be followed by `title ___` and `directions ___` directives before any exercises are entered. Examples:

```
exercise lambda conversion
exercise semantic types
exercise tree
```

***

`title ___`

Specifies the title for the group of exercises. This directive must be specified after an `exercise ___` directive and before the actual exercises. Example:

```
title Lambda Conversion
```

***

`directions ___`

Specifies the directions to provide to the student for a group of exercises. This directive must be specified after an `exercise ___` directive and before the actual exercises. Directions can be split across multiple lines by repeating the `directions ___` directive. Example:

```
directions After checking that the type of the function and the type of the
directions argument(s) match, simplify the following expressions performing
directions one lambda-conversion at a time.
```

You can include predicate logic expressions inside the directions. By writing the expressions according to the directions below, and surrounding them in curly braces, they will appear to the student rendered nicely with the appropriate symbols for lambdas, etc.

```
directions The expression {Lx.Iy.P(y,x)} has type <e,e>.
```

Greek letters may be included in two ways. 1) By preceding the name of the letter with a backslash, as in LaTeX. For instance, \alpha for lowercase alpha or \Omega for a capital omega. (Not all Greek letters are supported.) 2) By putting the special character directly within the text file (i.e. with Word's Insert Symbol), and saving the file in UTF-8 Unicode encoding.

***

`instructions ___`

Specifies instructions for the following exercise only. This directive must be specified immediately before the exercise to which it applies. Instructions can be split across multiple lines and can include expressions in the same way as with the `directions` directive, described above. Example:

```
instructions This exercise is a trick question!
Lx[a] (b)
```

***

`multiple letter identifiers`
`single letter identifiers`

When the `single letter identifiers` directive is set, certain abbreviations are accepted when reading predicate logic expressions. For instance, "Pabc" may be used to abbreviate P(a,b,c). When `multiple letter identifiers` is used, "Pabc" is treated as a single identifier. If neither directive is specified, `multiple letter identifiers` is assumed.

***

`define ___ : ___`

Defines lexical entries for use in tree exercises provided to the student. The student will be able to enter other lexical entries as needed. Multiple words can be given the same lexical entry by separating the words with comma. A word may be given multiple lexical entries by repeating the `define` line multiple times. A single lexicon is used for all exercises in the file. Example:

```
define Sue : s
define introduce, introduces : LxLyLz.introduces(z,x,y)
```

***

`use rule ___`

Indicates which composition rules are allowed to be used in tree exercises, including function application, intensional function application, non-branching nodes (copying the denotation of the single child node to the parent node), predicate modification, and lambda abstraction, based on the Heim & Kratzer textbook. As of version 2.4.0, function composition can also be used to combine nodes. Example:

```
use rule function application
use rule non-branching node
use rule predicate modification
use rule lambda abstraction
use rule intensional function application
use rule function composition
```

***

## Exercises

Lines that are not recognized as directives are assumed to be exercises, of the type specified in a preceding `exercise ___` directive.

### Semantic Types Exercises

For this exercise type, the instructor enters an expression in predicate logic. The student's task is to provide the semantic type of the expression. For example, if the exercise is:

```
λx.P(x) ∧ Q(x)
```

The student is expected to answer:

```
<e,t>
```

Or in an equivalent way, such as abbreviated as `et`.

Predicate logic expressions can be entered in exercise files without needing to insert special characters like lambdas and wedges. This is described in more detail below. For the example above, the instructor enters into the exercise file:

```
Lx.P(x) & Q(x)
```

The correct answer, i.e. the type of this expression, does not need to be entered into the exercise file. The Lambda program will compute the correct answer itself.

### Lambda Conversion Exercises

For this exercise type, the instructor enters an expression in predicate logic. The student's task is to simplify the expression by performing lambda conversions, one at a time. For example, if the exercise is:

```
[λx.λy.R(a,y) ∧ Q(x)] (a) (b)
```

The student is expected first to answer:

```
[λy.R(a,y) ∧ Q(a)] (b)
```

The Lambda program will check this intermediate answer and then allow the student to proceed to the next step:

```
R(a,b) ∧ Q(a)
```

It is possible to lambda-convert over higher types. This proceeds as follows:

```
[λX.X(b)] (λx. R(a,x)) 
[λx.R(a,x)] (b) 
R(a,b)
```

In addition, non-reducible predicate logic expressions may also be provided in the exercise file, such as the expression `P(x)`. In this case, the student is expected to respond with the same expression.

Sometimes creating an alphabetical variant is needed before proceeding to lambda conversion. In these cases, the Lambda program will realize that an alphabetical variant is necessary, and it will require that the student enters it before preceeding with simplification. An example of this is shown below:

```
Exercise: [λx.∃y.R(y,x)] (y)
Student's responses:
1) [λx.∃yʹ.R(yʹ,x)] (y)
2) ∃yʹ.R(yʹ,y)
```

The correct answers to lambda conversion exercises, i.e. the simplifications of the expressions, do not need to be entered into the exercise file. The Lambda program will compute the correct answer (including the steps needed to reach it) itself.

### How to Enter Predicate Logic Expressions

The Lambda program will read predicate logic expressions from exercise files. Predicate logic expressions may include:

* Constants and variables. If the `single letter identifiers` directive is used, constants and variables must be single letters only. Otherwise they may contain multiple characters, including primes (entered with the apostrophe) and asterisks. Some capital letters (A, E, I, and L) are treated specially by the Lambda program; see the note below on using these letters within constants and variables.
* Constants and variable with explicit types using an underscore to separate the name and the type, such as: `x_e`, `X_<e,t>`, etc. Composite types such as `<e,t>` must be written with brackets and cannot be abbreviated as `et`. Explicit types are unnecessary when the typing conventions specified in the exercise file already provide a type for the identifier.
* Predicates, entered as `P(a)` or `P(a,b,c)` (or if the `single letter identifiers` has been specified, in abbreviated form such as `Pabc`).
* The logical connectives, which can be entered directly, or according to the shortcut table below:

  <table>
  <tr><th>Connective</th> <th>You Type</th></tr>
  <tr><td style="text-align: center">¬</td><td style="text-align: center">~</td></tr>
  <tr><td style="text-align: center">=</td><td style="text-align: center">=</td></tr>
  <tr><td style="text-align: center">≠</td><td style="text-align: center">!=</td></tr>
  <tr><td style="text-align: center">∧</td><td style="text-align: center">&amp;</td></tr>
  <tr><td style="text-align: center">∨</td><td style="text-align: center">V</td></tr>
  <tr><td style="text-align: center">→</td><td style="text-align: center">-&gt;</td></tr>
  <tr><td style="text-align: center">↔</td><td style="text-align: center">&lt;-&gt;</td></tr>
  <tr><td style="text-align: center">⊕</td><td style="text-align: center">+</td></tr>
  <tr><td style="text-align: center">⊑</td><td style="text-align: center">&lt;:</td></tr>
  </table>

* Parenthesized expressions, using parens (...) or brackets [...].
* Quantifiers and binders, which can be entered directly, or using capital Roman letters according to the table below:

  <table>
  <tr><th>Quantifier</th> <th>You Type</th></tr>
  <tr><td style="text-align: center">∃</td><td style="text-align: center">E</td></tr>
  <tr><td style="text-align: center">∀</td><td style="text-align: center">A</td></tr>
  <tr><td style="text-align: center">λ</td><td style="text-align: center">L</td></tr>
  <tr><td style="text-align: center">ι</td><td style="text-align: center">I</td></tr>
  </table>

* For example, `Ax.P(x)`, `AxEy.P(x) & Q(y)`, `Lx.P(x)`, `LxLy.P(x,y) & Q(y)`, `Iz.king(z)`. Periods may separate multiple consecutive quantifiers/binders. After quantifiers and binders, a bracketed expression should follow to avoid scope ambiguity.
* To use capital E, A, L, I, and V as those letters in a constant or variable, precede the letter with a backslash (\) to indicate to the program to treat the letter following like normal, e.g. `H\APPY`, `\L\ITT\L\E`.
* Function application, which is an expression followed by another expression (usually in parenthesis and separated by a space), such as `[Lx.P(x)] (b)`.
* Single Greek letters may be used as constants or variables by preceding the name of the letter with a backslash, as in LaTeX. For instance, `\alpha` for lowercase alpha or `\Omega` for a capital omega. (Not all Greek letters are supported.) Alternatively, Greek letters may be entered directly with a Greek keyboard or by copying and pasting from another source.
* Operator precedence: The binding domain of binders like ∃, ∀, λ, and ι extends as far to the right as possible within their nesting groups. So `[∀x.P(x) & Q(x)]` gives the quantifier scope over the conjunction. Among the propositional connectives, negation has higher precedence than anything else, and everything else is of equal precedence. So `p & q → r` is not well-formed and must be disambiguated as either `(p & q) → r` or `p & (q → r)`. the right as possible within its nesting group.

### Tree Exercises

In tree exercises, the instructor provides a tree in labeled brackets notation. The student is presented with a graphical rendition of the tree and is expected to provide the denotations of the terminal nodes and to evaluate the denotations of the nonterminal nodes up to the root.

To create a tree exercise, simply provide the brackets notation of the tree. To give nonterminal node labels, precede the label with a period. Nodes are not required to be labeled.

```
[.S [.NP John] [.VP [.V' loves] Mary] ]

[ [the [fast man]] [ate [many apples]]]
```

Superscripts and subscripts may be indicated with the caret (^) and underscore (_). Only integer subscript indices are allowed. On nonterminals, these play no special role in the program. They are for pretty display purposes only.

```
[.DP_1 the [.NP_1 man [that_1 [I [ [.V^0 saw] t_1] ] ] ]]
```

Traces and pronouns with indices (t_ _i_, he_ _i_, she_ _i_, it_ _i_, him_ _i_, her_ _i_, himself_ _i_, herself_ _i_, itself_ _i_, his_ _i_, hers_ _i_, its_ _i_, theirs_ _i_) are automatically given the denotation of g(1), g(2), etc. according to the index.

Relative pronouns with indices (that_ _i_, what_ _i_, which_ _i_, who_ _i_, such_ _i_) and bare indices (1, 2, 3, ...) are treated as "bare index" terminal nodes for lambda abstraction, following Heim & Kratzer.

Single Greek letters may be used as terminal and nonterminal labels by preceding the name of the letter with a backslash, as in LaTeX. For instance, `\alpha` for lowercase alpha or `\Omega` for a capital omega. (Not all Greek letters are supported.)

It is possible to define a lexical entry within the tree exercise. To do this, type `=` after the terminal node, specify the new lexical entry, then finish the command with `;`. This command can also be used to override globally defined lexical entries in the exercise file.

```
[ John=j_e; ran ]
```
