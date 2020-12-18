[The Lambda Calculator](https://lambdacalculator.com)

## Documentation
**For students and instructors:** When typing predicate logic expressions in
the program, some special keyboard combinations are used to enter logical
symbols. See the [special symbols](https://github.com/nyusemantics/LambdaCalculator-Issues/wiki/Special-Symbols)
guide for entering special symbols for instructions. Some help is also provided
within the program.

**For Instructors:** If you would like to write an exercise file, you may read
the [exercises file](https://github.com/nyusemantics/LambdaCalculator-Issues/wiki/Exercise-File-Format) documentation
for the exercise file format or consult the example0.txt complete and
documented sample exercise file for instructors

## A Note on Operator Precedence

### Binding Domains

The scope of binding operators extends as far to the right as possible, within
the binder's nesting group.
* `∀x.P(x) ∧ Q(x)` is parsed as `[∀x[P(x) ∧ Q(x)]]`
* `λyλx.f(x)(a)` is parsed as `[λy[λx[f(x)(a)]]]`

Importantly, brackets are used only for grouping; they do not delimit scope.
* `∀x.[P(x) ∧ Q(x)] ∧ R(x)` is parsed as `[∀x[[P(x) ∧ Q(x)] ∧ R(x)]]`
* `λyλx.[f(x)](a)` is parsed as `[λy[λx[f(x)(a)]]]`

The dot following the binder is purely aesthetic, as is white space. However,
unless the exercise explicitly declares `single letter identifiers`, there will
need to be some indication as to when the binding prefix ends and the body of
the lambda term begins.

All of the following are parsed as `[λy[λx[f(x)(a)]]]`.
* `λyλx.f(x)(a)`
* `λyλx f(x)(a)`
* `λyλx[f(x)(a)]`
* `λyλx. f(x)(a)`
* `λyλx.[f(x)(a)]`
* `λyλx [f(x)(a)]`
* `λyλx. [f(x)(a)]`
* `λyλxf(x)(a)` (will not work unless `single letter identifiers` declared)

### Connectives

Within a nesting group, the following precedence among logical connectives are
imposed:
1. ¬
2. ∩, ∪, ⊕
3. =, ≠, <, ≤, ⊆, ⊑
4. ∧, ∨, →, ↔

Some examples
* `S ∩ T = Y` is parsed as `[[S ∩ T] = Y]`
* `X ⊕ T ⊑ Y` is parsed as `[[X ⊕ T] ⊑ Y]`
* `¬p ∨ q = r` `[[¬p] ∨ [q = r]]`
* `¬p ∨ q → r` is ambiguous and will not parse
* `¬p ∨ [q → r]` is parsed as `[[¬p] ∨ [q → r]]`

## Polymorphism
Atomic types come in two varieties: concrete and variable. Concrete types are designated by `e`, `t`, `s`, `n`, `v`, and `i`. Variable types can be any alphabetic character, lowercase or uppercase, as long as they are specified with a preceding apostrophe (e.g. `'a`). These variable types are displayed as Greek letters in the Lambda Calculator.

Atomic variable types are "equal" to all other types. Product types and composite types are compared component-wise. For example:

```
'a  == e
'a  == 'b
'a  == <e,t>
<'a,t> == <e,t>
<'a,t> == <et,t>
<'a,t> != <e,tt>
<'a,t> != e
```

When using Function Application, for example, a function of type `<'a,t>` will accept an individual argument of type `e` and produce a truth value of type `t` as a result.

More generally, when a flexible function encounters a concrete argument, all of the type variables in its signature are made concrete. For example, when the fully general function composition operator, of type `<<'b,'c>,<<'a,'b>,<'a,'c>>>` is applied to a specific argument of type `<e,t>`, the remaining function composition closure is reset to type `<<'a,e>,<'a,t>>`. That is, while type `'a` is still free, the type variables `'b` and `'c` have been "captured" by the `et` property that saturated the first argument. When multiple instances of the same variable occur in the same type signature, as `'a` does in our example, the value of these instances must co-vary. At the next application step, another concrete argument will fix the type of all instances of `'a`, and thus by the third step the final closure will be entirely concrete. 

This polymorphism can be used for flexible functions (often called combinators) applied to concrete lexical items. The Lambda Calculator also supports the application of a combinator to other combinators, or the definition of lexical items (e.g. coordinating conjunctions, pronouns) that take multiple types of arguments. For instance, the Predicate Modification rule will now combine any two arguments of the form `<x,t>`, where `x` is any constant, variable, or composite type. We can define "and" along the same lines, giving it type `<<'a,t>,<<'a,t>,<'a,t>>>`. The first argument, if concrete, will fix the required type of the second and third arguments.

This functionality can also be used to combine two polymorphic expressions. As explained above, when multiple instances of the same variable occur in the same type signature, as in `<'a,'a>`, these instances must all be fixed to the same concrete type. However, when the same variable occurs in the type signature of separate expressions, these variables are treated as distinct. For example, the `'a` in the two separate type signatures `<<e,'a>,t>` and `<'a,t>` are not treated as the same variable, and two expressions of these types may be combined using the Function Application rule without error. This means the same variable names can be reused multiple times across lexical entries without name collision.

More complicated examples:

```
<e,'a> == <'a,t>
<'b,'b>  == <'a,'b>
<'a,'a> == <'c,t>
<'a,<'a,t>> != <'c,<e,'c>>
<'A,'A> != <e,t>
```

As a note, a nonterminal node dominating two children, both of type `<'a,t>`, will satisfy three composition rules: Predicate Modification, Left-to-Right Function Application, and Right-to-Left Function Application. The user will be prompted to choose among applicable composition rules, even in the teacher version.