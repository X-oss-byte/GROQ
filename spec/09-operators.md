# Operators

## And operator

And : Expression `&&` Expression

EvaluateAnd(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If {left} or {right} is {false}:
  - Return {false}.
- If {left} or {right} is not a boolean:
  - Return {null}.
- Return {true}.

## Or operator

Or : Expression `||` Expression

EvaluateOr(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If {left} or {right} is {true}:
  - Return {true}.
- If {left} or {right} is not a boolean:
  - Return {null}.
- Return {false}.

## Not operator

Not : `!` Expression

EvaluateNot(scope):

- Let {valueNode} be the {Expression}.
- Let {value} be the result of {Evaluate(valueNode, scope)}.
- If {value} is {false}:
  - Return {true}.
- If {value} is {true}:
  - Return {false}.
- Return {null}.

## Equality operators

Equality : Expression EqualityOperator Expression

EqualityOperator : one of `==`, `!=`

EvaluateEquality(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- Let {result} be the result of {Equal(left, right)}.
- If the operator is `!=`:
  - If {result} is {true}:
    - Return {false}.
  - If {result} is {false}:
    - Return {true}.
- Return {result}.

## Comparison operators

Comparison : Expression ComparisonOperator Expression

ComparisonOperator : one of `<`, `<=`, `>`, `>=`

EvaluateComparison(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- Let {cmp} be the result of {PartialCompare(left, right)}.
- If {cmp} is {null}:
  - Return {null}.
- If {cmp} is {Less} and the operator is {<} or {<=}:
  - Return {true}.
- If {cmp} is {Greater} and the operator is {>} or {>=}:
  - Return {true}.
- If {cmp} is {Equal} and the operator is {<=} or {>=}:
  - Return {true}.
- Return {false}.

## In operator

In : Expression in Expression

EvaluateIn(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If {right} is an array:
  - For each {value} in {right}:
    - If {Equal(left, value)} is {true}:
      - Return {true}.
  - Return {false}.
- If {right} is a range:
  - Let {lower} be the start value of {right}.
  - Let {upper} be the end value of {right}.
  - Let {leftCmp} be the result of {PartialCompare(left, lower)}.
  - Let {rightCmp} be the result of {PartialCompare(left, upper)}.
  - If {leftCmp} or {rightCmp} is {null}:
    - Return {null}.
  - If {leftCmp} is {Less}:
    - Return {false}.
  - If {rightCmp} is {Greater}:
    - Return {false}.
  - If the {right} range is exclusive and {rightCmp} is {Equal}:
    - Return {false}.
  - Return {true}.
- Return {null}.

## Match operator

The match operator is defined in terms of _patterns_ and _tokens_: It returns true when any of patterns matches all of the tokens. The exact way of tokenizing text and interpreting patterns is left as an implementation detail.

Match : Expression match Expression

EvaluateMatch(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- Let {tokens} be an empty array.
- If {left} is a string:
  - Concatenate {MatchTokenize(left)} to {tokens}.
- If {left} is an array:
  - For each {value} in {left}:
    - If {value} is a string:
      - Concatenate {MatchTokenize(value)} to {tokens}.
- Let {patterns} be an empty array.
- If {right} is a string:
  - Append {MatchAnalyzePattern(right)} to {patterns}.
- If {right} is an array:
  - For each {value} in {right}:
    - If {value} is a string:
      - Append {MatchAnalyzePattern(value)} to {patterns}.
    - Otherwise: \* Return {false}.
- If {patterns} is empty:
  - Return {false}.
- For each {pattern} in {patterns}:
  - If {pattern} does not matches {tokens}:
    - Return {false}.
- Return {true}.

MatchTokenize(value):

- Return an array of tokens.

MatchAnalyzePattern(value):

- Return a pattern for the given string.

## Asc operator

The asc operator is used by the {order()} function to signal that you want ascending sorting. Evaluating it in any other context is not allowed.

Asc : Expression `asc`

ValidateAsc():

- Report an error.

## Desc operator

The desc operator is used by the {order()} function to signal that you want descending sorting. Evaluating it in any other context is not allowed.

Desc : Expression `desc`

ValidateDesc():

- Report an error.

## Unary plus operator

UnaryPlus : `+` Expression

EvaluateUnaryPlus(scope):

- Let {valueNode} be the {Expression}.
- Let {value} be the result of {Evaluate(valueNode, scope)}.
- If {value} is a number:
  - Return {value}.
- Return {null}.

## Unary minus operator

UnaryMinus : `-` Expression

EvaluateUnaryMinus(scope):

- Let {valueNode} be the {Expression}.
- Let {value} be the result of {Evaluate(valueNode, scope)}.
- If {value} is a number:
  - Return {value} with opposite sign.
- Return {null}.

## Binary plus operator

Plus : Expression `+` Expression

EvaluatePlus(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If both {left} and {right} are strings:
  - Return the string concatenation of {left} and {right}.
- If both {left} and {right} are numbers:
  - Return the addition of {left} and {right}.
- If both {left} and {right} are arrays:
  - Return the concatenation of {left} and {right}.
- If both {left} and {right} are objects:
  - Return the merged object of {left} and {right}. For duplicate fields the value from {right} takes precedence.
- If {left} is a datetime and {right} is a number:
  - Return a new datetime that adds (or subtracts, if negative) {right} as a number of seconds to {left}.
- Return {null}.

## Binary minus operator

Minus : Expression `-` Expression

EvaluateMinus(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If both {left} and {right} are numbers:
  - Return the subtraction of {left} from {right}.
- If both {left} and {right} are datetimes:
  - Return the difference, in seconds, between {left} from {right}.
- If {left} is a datetime and {right} is a number:
  - Return a new datetime being {left} minus {right} as seconds.
- Return {null}.

## Binary star operator

Star : Expression `*` Expression

EvaluateStar(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If both {left} and {right} are numbers:
  - Return the multiplication of {left} and {right}.
- Return {null}.

## Binary slash operator

Slash : Expression `/` Expression

EvaluateSlash(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If both {left} and {right} are numbers:
  - Return the division of {left} by {right}.
- Return {null}.

## Binary percent operator

Percent : Expression `%` Expression

EvaluatePercent(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If both {left} and {right} are numbers:
  - Return the remainder of {left} after division by {right}.
- Return {null}.

## Binary double star operator

StarStar : Expression `**` Expression

EvaluateStarStar(scope):

- Let {leftNode} be the first {Expression}.
- Let {left} be the result of {Evaluate(leftNode, scope)}.
- Let {rightNode} be the last {Expression}.
- Let {right} be the result of {Evaluate(rightNode, scope)}.
- If both {left} and right are numbers:
  - Return the exponentiation of {left} to the power of {right}.
- Return {null}.
