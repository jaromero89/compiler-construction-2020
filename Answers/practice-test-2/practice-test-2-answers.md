Parse 

    int main(){int x=++x-++x*x}

with the grammar [cpp.cf](https://github.com/alexhkurz/compiler-construction-2020/blob/master/Sources/Cpp/cpp.cf)

|Stack| Input| Rule |
|---:|:---| :--: |
| | `int main(){int x=++x-++x*x}` |
|`int` | `main(){int x=++x-++x*x}` |
|`Type` | `main(){int x=++x-++x*x}` | Type
|`Type main` | `(){int x=++x-++x*x}` | 
|`Type Id` | `(){int x=++x-++x*x}` | Id
|`Type Id (` | `){int x=++x-++x*x}` | 
|`Type Id ( )` | `{int x=++x-++x*x}` |
|`Type Id ( ) {` | `int x=++x-++x*x}` |
|`Type Id ( ) { int` | ` x=++x-++x*x}` |
|`Type Id ( ) { Type` | ` x=++x-++x*x}` | Type
|`Type Id ( ) { Type x` | `=++x-++x*x}` |
|`Type Id ( ) { Type Id` | `=++x-++x*x}` | Id
|`Type Id ( ) { Type Id =` | `++x-++x*x}` | 
|`Type Id ( ) { Type Id = ++` | `x-++x*x}` |
|`Type Id ( ) { Type Id = ++ x` | `-++x*x}` |
|`Type Id ( ) { Type Id = ++ Id` | `-++x*x}` | Id
|`Type Id ( ) { Type Id = ++ Exp15` | `-++x*x}` | EId
|`Type Id ( ) { Type Id = ++ Exp14` | `-++x*x}` | 
|`Type Id ( ) { Type Id = Exp13` | `-++x*x}` | EIncr
|`Type Id ( ) { Type Id = Exp11` | `-++x*x}` |
|`Type Id ( ) { Type Id = Exp11 -` | `++x*x}` |
|`Type Id ( ) { Type Id = Exp11 - ++` | `x*x}` |
|`Type Id ( ) { Type Id = Exp11 - ++ x` | `*x}` |
|`Type Id ( ) { Type Id = Exp11 - ++ Id` | `*x}` | Id
|`Type Id ( ) { Type Id = Exp11 - ++ Exp15` | `*x}` | EId
|`Type Id ( ) { Type Id = Exp11 - ++ Exp14` | `*x}` |
|`Type Id ( ) { Type Id = Exp11 - Exp13` | `*x}` | EIncr
|`Type Id ( ) { Type Id = Exp11 - Exp12` | `*x}` |
|`Type Id ( ) { Type Id = Exp11 - Exp12 *` | `x}` |
|`Type Id ( ) { Type Id = Exp11 - Exp12 * x` | `}` |
|`Type Id ( ) { Type Id = Exp11 - Exp12 * Id` | `}` | Id
|`Type Id ( ) { Type Id = Exp11 - Exp12 * Exp15` | `}` | EId
|`Type Id ( ) { Type Id = Exp11 - Exp12 * Exp13` | `}` | 
|`Type Id ( ) { Type Id = Exp11 - Exp12` | `}` | ETimes
|`Type Id ( ) { Type Id = Exp11` | `}` | EMinus
|`Type Id ( ) { Type Id = Exp` | `}` | 
| | |
| at this point the deriviation gets stuck because of ... | ... a missing ";" |
| we can continue if we correct the remaining input to be: | `;}`|
| | |
|`Type Id ( ) { Type Id = Exp ;` | `}` |
|`Type Id ( ) { Stm` | `}` | SInit
|`Type Id ( ) { Stm }` |  |
|`Def` |  |DFun
|`Program` | | PDefs 

## Concrete Syntax Tree

The first page of [Trees](syntax-trees.pdf) shows how much of the concrete syntax tree can be built before discovering that a `;` is missing. The second page shows the concrete syntax tree for the corrected program.

## Abstract Syntax Tree

Linearized (from the bnfc-generated parser):

    PDefs [DFun Type_int (Id "main") [] [SInit Type_int (Id "x") (EMinus (EIncr (EId (Id "x"))) (ETimes (EIncr (EId (Id "x"))) (EId (Id "x"))))]]

The third page of [Trees](syntax-trees.pdf) shows how much of the abstract syntax tree can be built before discovering that a `;` is missing. The fourth page shows the abstract syntax tree for the corrected program (there should be an edge from PDefs to DFun).