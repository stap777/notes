
# 09:09:26 | 06:05 -- 06:40

## System Architecture Roadmap

## Month 1 • Day 1 • Session 1 (90 min)

## What actually happens when you run a program?

**Objective:** Trace a program from a text file on disk to electrical signals inside the CPU.

Today is not about memorizing the pipeline.

It's about understanding **why every layer exists**.

---

## The Journey We'll Follow

We'll use a simple C program.

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

By the end of today's lesson, you'll understand how this eventually becomes millions of microscopic transistors switching on and off.

Complete pipeline:

```text
Source Code
      ↓
Preprocessor
      ↓
Compiler Frontend
      ↓
Parser
      ↓
AST
      ↓
Compiler Backend
      ↓
Assembly
      ↓
Assembler
      ↓
Object File
      ↓
Linker
      ↓
Executable
      ↓
OS Loader
      ↓
Virtual Memory
      ↓
CPU Fetch-Decode-Execute
```

Notice something.

The "compiler" is only one piece.

There are actually **nine major transformations** before the CPU executes a single instruction.

---

## Part 1: Source Code Is Just Data

Open `hello.c`.

What do you see?

```c
printf("Hello");
```

Humans immediately understand this.

The computer does not.

Why?

Because the file contains **bytes**, not "programming."

## What is actually stored?

Every character is stored using an encoding.

Example:

|Character|ASCII|Binary|
|---|---|---|
|`p`|112|`01110000`|
|`r`|114|`01110010`|
|`i`|105|`01101001`|

So inside the SSD:

```text
01110000
01110010
01101001
01101110
01110100
```

There is no concept of:

- function
    
- variable
    
- keyword
    

Only bytes.

The operating system also doesn't know this is C code.

It only knows:

> "This file contains bytes."

This is our first important realization.

> **Programming languages are conventions built on top of ordinary files.**

---

## Part 2: What Happens When You Press Compile?

Suppose we run:

```bash
gcc hello.c
```

Many students think GCC directly creates machine code.

It doesn't.

GCC launches several internal programs.

Real pipeline:

```text
hello.c
   ↓
cpp
   ↓
cc1
   ↓
as
   ↓
ld
```

Each tool has one responsibility.

|Tool|Responsibility|
|---|---|
|`cpp`|Preprocessor|
|`cc1`|Compiler|
|`as`|Assembler|
|`ld`|Linker|

Unix philosophy appears here.

Instead of one giant program doing everything, several specialized programs cooperate.

We'll now study each one.

---

## Part 3: The Preprocessor

The preprocessor runs **before compilation**.

Example:

```c
#include <stdio.h>

#define PI 3.14
```

The preprocessor performs textual substitution.

Think of it as an advanced search-and-replace engine.

Input:

```c
#define PI 3.14

float x = PI;
```

Output:

```c
float x = 3.14;
```

No intelligence.

No grammar.

No understanding.

It simply rewrites text.

## Include expansion

This line:

```c
#include <stdio.h>
```

doesn't magically import code.

Instead,

```text
hello.c
```

is expanded into something much larger.

`stdio.h` contains declarations like:

```c
int printf(const char *, ...);
```

The compiler now knows:

> "A function named `printf` exists."

Important distinction.

Headers usually contain **declarations**, not implementations.

The actual implementation lives inside system libraries.

We'll revisit this during linking.

---

## Part 4: The Compiler Frontend

Now real compilation begins.

The frontend performs three major jobs.

1. Lexical Analysis
    
2. Parsing
    
3. Semantic Analysis
    

Today we'll preview them.

The next several days will implement each one ourselves.

---

## Lexical Analysis Preview

The compiler first reads characters.

Input:

```text
i n t   x = 5 ;
```

Output:

```text
KEYWORD(int)
IDENTIFIER(x)
ASSIGN
NUMBER(5)
SEMICOLON
```

Notice what changed.

Characters became **tokens**.

Tokens are the vocabulary of the language.

This stage is called **lexical analysis**.

Tomorrow we'll build a lexer ourselves.

---

## Parsing Preview

Tokens alone aren't enough.

The compiler must understand relationships.

Example:

```c
x = y + 5;
```

Is this:

```text
(x = y) + 5
```

or

```text
x = (y + 5)
```

The parser uses grammar rules.

It constructs a tree.

Example AST:

```text
Assign
├── Variable(x)
└── Add
    ├── Variable(y)
    └── Number(5)
```

This tree preserves meaning.

Why a tree?

Because programming languages are hierarchical.

Consider:

```c
a + b * c
```

Multiplication has higher precedence.

The tree naturally represents this.

Without the tree, optimization becomes extremely difficult.

---

## Semantic Analysis

Suppose we write:

```c
int x;
x = "hello";
```

Grammar is valid.

But meaning is wrong.

Semantic analysis checks:

- types
    
- variable declarations
    
- function signatures
    
- scope
    

This is where many compiler errors originate.

Example:

```text
error: incompatible types
```

That isn't a parsing error.

It's a semantic error.

---

## Part 5: Code Generation

Once the compiler understands the program, it must produce instructions.

It doesn't usually jump directly to binary.

Instead it often generates **assembly**.

Example:

C:

```c
x = x + 1;
```

Assembly:

```asm
mov eax,[x]
add eax,1
mov [x],eax
```

Let's decode this.

`mov`

means copy.

`eax`

is a CPU register.

The compiler has now translated a high-level idea into CPU operations.

Registers are tiny storage locations inside the processor.

They're much faster than RAM.

We'll study registers deeply in Month 3.

---

## Part 6: The Assembler

Assembly is still readable.

The CPU cannot read words like:

```asm
mov
```

The assembler converts each instruction into opcodes.

Example:

```asm
mov eax,1
```

becomes bytes.

Example (simplified):

```text
B8 01 00 00 00
```

These hexadecimal values are actual machine instructions.

Every CPU architecture defines its own instruction set.

Examples:

- x86
    
- ARM
    
- RISC-V
    

This is why software compiled for Windows x86 cannot simply run on an ARM processor.

Different instruction languages.

---

## Part 7: Object Files

After assembly, we don't yet have an executable.

We get an **object file**.

Example:

```text
hello.o
```

Think of it as a half-finished puzzle.

It contains:

- machine code
    
- symbol table
    
- relocation information
    

## Symbol table

Suppose:

```c
printf();
```

Where is `printf`?

The object file doesn't know.

Instead it records:

```text
printf
```

as an unresolved symbol.

The linker solves this later.

---

## Part 8: The Linker

The linker is one of the most overlooked components.

Its job:

> Connect every missing piece.

Suppose our program contains:

```c
printf();
```

The implementation exists inside:

```text
libc
```

The linker searches libraries.

Then connects references.

Visualization:

```text
hello.o
     │
     ├──── printf()
     │
     ▼
libc.so
```

Now the executable knows where `printf` lives.

Without linking,

your program literally contains holes.

---

## Static vs Dynamic Linking

Static:

```text
Program
├── printf
├── malloc
└── everything copied inside
```

Large executable.

No external dependency.

Dynamic:

```text
Program
   │
   ▼
libc.so
```

Smaller executable.

Shared libraries.

Linux uses dynamic linking extensively.

We'll inspect this using:

```bash
ldd hello
```

later.

---

## Part 9: The Loader

Now we execute:

```bash
./hello
```

The kernel's loader begins working.

The loader performs several critical tasks.

1. Creates a process.
    
2. Creates virtual memory.
    
3. Maps executable sections.
    
4. Loads shared libraries.
    
5. Initializes the stack.
    
6. Starts execution.
    

Notice:

The executable is **not copied byte-by-byte into RAM**.

Instead,

Linux often uses **memory mapping**.

The executable remains on disk.

Pages enter RAM only when needed.

This is called **demand paging**.

We'll study this in Month 3.

---

## Process Memory Layout

Every process receives its own virtual address space.

```
High Memory
+--------------------+
| Stack              |
|                    |
| grows downward     |
+--------------------+
| Shared Libraries   |
+--------------------+
| Heap               |
| grows upward       |
+--------------------+
| Data               |
+--------------------+
| Code (Text)        |
+--------------------+
Low Memory
```

Each region has a purpose.

|Region|Purpose|
|---|---|
|Text|Machine instructions|
|Data|Global variables|
|Heap|Dynamic memory|
|Stack|Function calls|

Understanding this layout later explains:

- segmentation faults
    
- stack overflow
    
- heap allocation
    
- `malloc()`
    

---

## Part 10: The CPU Finally Starts

The loader jumps to the program's entry point.

The CPU begins its infinite cycle.

## Fetch

The Program Counter holds an address.

Example:

```
0x401000
```

The CPU fetches bytes.

Example:

```
B8 01 00 00
```

## Decode

Control circuitry identifies the opcode.

It determines:

> "This means move."

## Execute

The ALU performs the operation.

Registers change.

Memory changes.

The Program Counter advances.

Then everything repeats.

Billions of times each second.

---

## Why Different Languages Behave Differently

C:

```
Source
→ Compiler
→ Machine Code
→ CPU
```

Java:

```
Source
→ Compiler
→ Bytecode
→ JVM
→ Machine Code
→ CPU
```

Python:

```
Source
→ Bytecode
→ Python VM
→ CPU
```

Different paths.

Same destination.

Every language eventually reaches machine instructions.

The difference is **when and where translation occurs**.

---

## Key Insights From Today's Session

1. A source file is only bytes.
    
2. Compilation is a multi-stage pipeline, not one step.
    
3. The preprocessor performs text substitution before compilation.
    
4. Tokens are created before parsing.
    
5. The ==parser== builds an Abstract Syntax Tree.
    
6. The compiler often generates assembly before binary.
    
7. The assembler creates machine instructions.
    
8. Object files still contain unresolved symbols.
    
9. The linker connects libraries like `printf`.
    
10. The loader creates a process and maps memory before the CPU executes anything.
    
11. Every program ultimately runs through the CPU's Fetch → Decode → Execute cycle. 
    

---

## Quick Self-Test

Without looking back, answer these:

1. Why is `#include <stdio.h>` handled before compilation?
    
2. What's the difference between an object file and an executable?
    
3. Why does the linker need a symbol table?
    
4. Why doesn't Linux always copy an executable entirely into RAM?
    
5. Why do C, Java, and Python follow different execution paths even though they all end up on the CPU?

## 10:09:26 1:35 - 02:05 -- 02:15 - 02:50 -- 03:20 -- 03:40(95min)
- 10 min: Active recall (Day 1)      
    
- 20 min: What problem does a lexer solve?
    
- 25 min: What is a token?
    
- 20 min: How a lexer works internally (finite-state machine)
    
- 15 min: Build our own lexer (Part 1)

## What problem does a lexer solve?

We'll only cover one concept before moving on.

### Imagine a compiler without a lexer

Suppose the parser receives this directly:

```
i n t   x   =   5   ;
```

Every stage of the compiler would need to answer questions like:

- Is this whitespace?
    
- Is this part of an identifier?
    
- Is this the keyword `int`?
    
- Did this number end?
    
- Is `>=` one operator or two?
    

That means the parser would spend most of its time recognizing characters instead of understanding the language.

### The lexer's job

The lexer acts like a scanner at an airport.

People arrive as a continuous crowd.

The scanner doesn't care about their destination.

It only separates them into recognizable categories.

Likewise, the lexer converts a stream of characters into a stream of tokens.

Example:

Input

```
int total = 42;
```

Output

```
KEYWORD(int)
IDENTIFIER(total)
ASSIGN(=)
NUMBER(42)
SEMICOLON(;)
```

Notice something important.

The lexer does not understand that this is a variable declaration.

It only says:

> "I found a keyword. I found an identifier. I found a number."

The parser will later determine what those tokens mean together.

## Mental model

Think of a book.

- Characters are ink.
    
- Words are tokens.
    
- Sentences are what the parser understands.
    

The lexer's entire job is converting ink into words.

## What exactly is a Token? (15-20 min)

We're not moving to finite-state machines yet. First we need to understand what a token actually is internally.

## Definition

A token is the smallest meaningful unit of a programming language that the parser can understand.

Notice the wording:

> Meaningful to the parser, not to the CPU.

For example:

```
int total = 42;
```

becomes

|Source|Token Type|Token Value|
|---|---|---|
|`int`|`KEYWORD`|`"int"`|
|`total`|`IDENTIFIER`|`"total"`|
|`=`|`ASSIGN`|`"="`|
|`42`|`NUMBER`|`"42"`|
|`;`|`SEMICOLON`|`";"`|

### Token = Type + Value

This is a crucial concept.

A token isn't just `"42"`.

Internally, it's closer to this structure:

```
Token {
    type: NUMBER
    value: "42"
}
```

Similarly,

```
Token {
    type: IDENTIFIER
    value: "total"
}
```

Real compilers store even more information.

Example:

```
Token {
    type: IDENTIFIER
    value: "total"
    line: 3
    column: 8
}
```

Why?

Because when the compiler reports:

```
Error at line 3, column 8
```

it already knows exactly where every token came from.

## How does the lexer decide a token's type?

It doesn't use a dictionary for everything.

Instead, different token categories follow different recognition rules.

|Category|Recognition Rule|
|---|---|
|Keyword|Exact match (`if`, `while`, `int`)|
|Identifier|Letter or `_`, followed by letters/digits/`_`|
|Number|One or more digits|
|Operator|Match longest valid operator (`+=`, `==`, `<=`)|
|String|Starts and ends with `"`|
|Whitespace|Usually skipped|

Notice something interesting.

The lexer first recognizes "this looks like an identifier."

Only afterward does it ask:

> "Is this identifier actually one of the reserved keywords?"

For example:

```
int
```

matches the identifier pattern.

Then the keyword table upgrades it to `KEYWORD`.

But:

```
integer
```

is not upgraded.

It stays:

```
IDENTIFIER(integer)
```

This distinction is why variables like `integerCount` are valid even though they begin with `int`.

## Mini Challenge (Think Like a Lexer)

Don't think like a programmer. Think like the scanner.

Tokenize this exactly as a real lexer would:

```
if(count>=100){
    total_value+=25;
}
```

Write one token per line in the format:

```
TOKEN_TYPE(value)
```

Pay special attention to:

- `if`
    
- `>=`
    
- `{` and `}`
    
- `_` inside `total_value`
    
- `+=`
    

There's one subtle rule hidden in this example that almost everyone misses on the first attempt.

## Point 3: How does a lexer recognize tokens?

Now we reach the first implementation concept.

We're going to build a lexer tomorrow, but before writing code, you need to understand the algorithm.

## The naïve approach

Imagine reading one character at a time.

```
i n t   x = 5 ;
```

You might write:

```
Read i
Read n
Read t
```

But when do you decide you've finished reading `int`?

After `t`?

What if the word was `integer`?

This is the exact problem the lexer solves.

## The Golden Rule: Longest Valid Match

A lexer keeps reading until adding one more character would make the current token invalid.

Let's trace it.

Input:

```
integer_count=25;
```

We'll move one character at a time.

|Current characters|Valid identifier?|Action|
|---|---|---|
|`i`|Yes|Continue|
|`in`|Yes|Continue|
|`int`|Yes|Continue|
|`inte`|Yes|Continue|
|...|Yes|Continue|
|`integer_count`|Yes|Continue|
|`integer_count=`|No|Stop|

At that moment, the lexer emits:

```
IDENTIFIER(integer_count)
```

Then it starts scanning again at `=`.

### Why this rule matters

Consider:

```
>=
```

If the lexer stopped too early:

```
GREATER(>)
ASSIGN(=)
```

Wrong.

Using the longest valid match rule:

1. Read `>`.
    
2. Peek at the next character.
    
3. `>=` is a valid operator.
    
4. Emit `GREATER_EQUAL`.
    

This is called greedy matching, and nearly every modern lexer uses it.

## Internal memory of a lexer

A lexer doesn't need to understand the whole program.

At any moment, it mainly tracks three things:

```
Current Position
Current Character
Current Token Being Built
```

Example:

Source:

```
total=25;
```

During scanning:

```
Position: 0
Current: t
Building: "t"
```

Next:

```
Position: 1
Current: o
Building: "to"
```

Eventually:

```
Building: "total"
Next Character: '='
```

Since `=` cannot be part of an identifier, the lexer emits:

```
IDENTIFIER(total)
```

Then resets:

```
Building: ""
Current: '='
```

This simple cycle repeats until the end of the file.

## Point 4: The Lexer's Brain (Finite-State Machine)

Now we reach the first implementation concept. We're not writing code yet. First, you need to understand the machine we'll build.

Every lexer is essentially a Finite-State Machine (FSM).

> A Finite-State Machine is a machine that is always in one state, reads one input symbol at a time, changes state according to predefined rules, and performs actions when needed.

Think of it like a railway switch. Each character moves the train onto a different track.

## State 0: START

Every token begins here.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22260%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20640%20260%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Ccircle%20cx%3D%2280%22%20cy%3D%22130%22%20r%3D%2235%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%2280%22%20y%3D%22135%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3ESTART%3C%2Ftext%3E%3Ccircle%20cx%3D%22250%22%20cy%3D%2260%22%20r%3D%2235%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22250%22%20y%3D%2265%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3EIDENTIFIER%3C%2Ftext%3E%3Ccircle%20cx%3D%22250%22%20cy%3D%22200%22%20r%3D%2235%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22250%22%20y%3D%22205%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3ENUMBER%3C%2Ftext%3E%3Ccircle%20cx%3D%22430%22%20cy%3D%2260%22%20r%3D%2235%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22430%22%20y%3D%2265%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3EOPERATOR%3C%2Ftext%3E%3Cpath%20d%3D%22M115%20120%20L215%2070%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%22155%22%20y%3D%2288%22%20font-size%3D%2212%22%3Eletter%2F_%3C%2Ftext%3E%3Cpath%20d%3D%22M115%20140%20L215%20190%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%22150%22%20y%3D%22176%22%20font-size%3D%2212%22%3Edigit%3C%2Ftext%3E%3Cpath%20d%3D%22M115%20130%20L395%2060%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%22250%22%20y%3D%2236%22%20font-size%3D%2212%22%3E%2B%2C%20-%2C%20%3D%2C%20%26amp%3Bgt%3B%3C%2Ftext%3E%3C%2Fsvg%3E)

From `START`, the first character decides where we go.

|Character|Next State|
|---|---|
|Letter or `_`|IDENTIFIER|
|Digit|NUMBER|
|`+ - * = > <`|OPERATOR|
|`"`|STRING|
|Space|Skip|

The machine never guesses. Every transition is defined by rules.

## Walking through a real example

Input:

```
count+=25
```

Let's become the lexer.

### Step 1

Current character:

```
c
```

We're in `START`.

Rule:

> Letter → IDENTIFIER state.

Buffer:

```
"c"
```

### Step 2

Next character:

```
o
```

Still valid.

Buffer:

```
"co"
```

The same happens for:

```
u
n
t
```

Now:

Buffer:

```
"count"
```

### Step 3

Next character:

```
+
```

Question:

Can `+` belong to an identifier?

No.

So the machine performs two actions.

1. Emit:
    

```
IDENTIFIER(count)
```

2. Return to `START`.
    

Notice something subtle.

The `+` has not been consumed yet.

The lexer will process it next.

This behavior is called reprocessing or lookahead-driven emission, and it's one of the easiest places to introduce bugs when writing a lexer.

## The Operator State

Now we're back at `START`.

Current character:

```
+
```

Rule:

Go to `OPERATOR`.

Buffer:

```
"+"
```

Peek ahead.

Next character:

```
=
```

Together:

```
+=
```

Valid operator.

Emit:

```
PLUS_ASSIGN
```

Then continue scanning.

## Why FSMs are used instead of giant `if` statements

A beginner lexer often looks like this:

```
if c == 'i':
    ...
elif c == '1':
    ...
elif c == '+':
    ...
```

This quickly becomes hundreds of conditions.

An FSM organizes behavior into states, so each state only answers one question:

> "Given my current state and this character, where do I go next?"

That's why real compiler generators like Lex/Flex internally generate finite-state machines rather than enormous chains of `if` statements.

## The Subtle Bug: The lexer does NOT switch directly from NUMBER → IDENTIFIER

Your reasoning was:

> "It encounters `a`, number becomes invalid, so it changes to identifier."

A real lexer doesn't transition directly from the `NUMBER` state to the `IDENTIFIER` state.

Instead, it performs three distinct actions.

## Let's trace `42abc` like a real compiler

Input:

```
4 2 a b c
```

We'll keep track of four things:

|Step|Current Char|State|Buffer|
|---|---|---|---|
|1|`4`|START → NUMBER|`4`|
|2|`2`|NUMBER|`42`|
|3|`a`|NUMBER|`42`|

Now comes the crucial moment.

### What happens at `a`?

The lexer asks:

> Can `a` legally continue a NUMBER token?

Answer: No.

But notice something:

> `a` is not part of the number.

If we consumed it while still inside the number state, we'd lose information.

Instead, the lexer does this.

### Step A: Emit the completed token

```
NUMBER(42)
```

The buffer is cleared.

### Step B: Return to START

We haven't consumed `a` yet.

The lexer's position is still pointing at:

```
a
```

### Step C: Process `a` again

Now we're in START.

START sees:

```
a
```

Rule:

> Letter → IDENTIFIER

Now the machine builds:

```
IDENTIFIER(abc)
```

## Why can't it jump directly?

Imagine this source code:

```
123if
```

If the lexer simply switched NUMBER → IDENTIFIER after reading `i`, it would blur the boundary between two tokens.

Instead, the lexer creates two separate tokens:

```
NUMBER(123)
IDENTIFIER(if)
```

Later, the parser or semantic analyzer decides whether that sequence is legal.

The lexer's job is only to separate the stream into the longest valid tokens.

## The Golden Rule (Write this in your notes)

A lexer follows this exact cycle for every token:

1. Start building a token.
    
2. Keep reading while the current state remains valid.
    
3. When the next character would make the token invalid:
    
    - Emit the completed token.
        
    - Do not consume the invalid character.
        
    - Return to START.
        
    - Reprocess that character as the beginning of the next token.
        
    

This "reprocess the current character" behavior is called lookahead without consumption, and it's one of the most fundamental implementation patterns in compiler construction.

## Point 5: Building a Lexer (Part 1)

> Goal: Build a lexer the same way a compiler engineer would think about it, not by copying a tutorial.

We'll implement it in Python because the syntax stays out of the way. The algorithm is the same one used in C, Java, Go, and Rust compilers.

Today we will only build:

- Identifiers
    
- Keywords
    
- Numbers
    
- Whitespace handling
    
- End-of-file detection
    

Operators and strings come after the foundation is solid.

## Step 1: Before Writing Code, Design the Data

A common beginner mistake is writing scanning logic first.

Real compiler engineers first decide:

> What information does a token need to carry?

We'll use this structure.

```
Token:
    type
    value
    line
    column
```

Why each field?

|Field|Purpose|
|---|---|
|`type`|What kind of token it is|
|`value`|The actual text|
|`line`|Error reporting|
|`column`|Exact position|

Example:

Source:

```
count=42;
```

The identifier token becomes:

```
Token(
    type=IDENTIFIER,
    value="count",
    line=1,
    column=1
)
```

Notice something.

The compiler never has to search the source file later.

Every token already remembers where it came from.

## Step 2: What Does the Lexer Remember?

Our lexer needs surprisingly little memory.

Internally we'll keep:

```
Source Code
Current Position
Current Character
Current Line
Current Column
```

Think of a bookmark moving through a book.

Example:

Source:

```
abc 123
```

At the beginning:

```
Position: 0
Character: a
```

After moving:

```
Position: 1
Character: b
```

The lexer never jumps randomly.

It walks forward through the source exactly once.

This is why lexers are usually O(n).

If a file has one million characters, the lexer reads roughly one million characters.

## Step 3: The First Function Every Lexer Needs

Instead of showing the whole lexer, we'll build one piece.

### `advance()`

Purpose:

> Move exactly one character forward while keeping position information correct.

Pseudo-code:

```
advance()

Move position forward

Update current character

If newline:
    increase line
    reset column

Otherwise:
    increase column
```

### Why this function matters

Suppose the source is:

```
a
b
```

Without tracking line and column:

```
Error
```

is useless.

With proper tracking:

```
Error at line 2, column 1
```

Every real compiler depends on this.

## Step 4: The Second Essential Function

Before reading tokens, we need another primitive.

### `peek()`

Purpose:

> Look at the next character without moving.

Example:

Current:

```
+
```

Next:

```
=
```

`peek()` returns:

```
=
```

Current position does not change.

Why?

Because we haven't decided whether we're reading:

```
+
```

or

```
+=
```

This is the same lookahead concept we discussed earlier.

## Step 5: Whitespace Isn't a Token

This surprises many people.

Given:

```
count     =      5
```

A beginner lexer might produce:

```
SPACE
SPACE
SPACE
```

Real compilers usually don't.

Instead:

```
IDENTIFIER(count)
ASSIGN
NUMBER(5)
```

Whitespace is simply skipped.

Pseudo-code:

```
While current character is whitespace:

    advance()
```

Notice the pattern.

The lexer doesn't emit anything.

It just keeps moving.

## Think Like an Engineer (Design Exercise)

Before we write a single line of Python in the next part, I want you to make one design decision.

Suppose we're storing tokens.

Which design would you choose?

### Option A

```
["int", "count", "=", "5", ";"]
```

### Option B

```
[
  {type: KEYWORD, value:"int", line:1, column:1},
  {type: IDENTIFIER, value:"count", line:1, column:5},
  {type: ASSIGN, value:"=", line:1, column:11},
  ...
]
```

Pick one, and justify it like you're designing a compiler that thousands of developers will use. Don't just say "Option B has more information." Tell me what future compiler stages (parser, semantic analysis, error reporting, debugging) gain from that design.

## Point 6: Designing Our Lexer in Java (First Real Implementation)

We're not writing the whole lexer. We'll design it the way a compiler engineer would.

## Step 1: The `TokenType` enum

Before creating tokens, we need a vocabulary.

Instead of writing strings like `"IDENTIFIER"` everywhere, we use an `enum`.

```
public enum TokenType {
    IDENTIFIER,
    KEYWORD,
    NUMBER,

    ASSIGN,
    PLUS_ASSIGN,

    LEFT_PAREN,
    RIGHT_PAREN,
    LEFT_BRACE,
    RIGHT_BRACE,

    SEMICOLON,

    EOF
}
```

### Why an enum instead of strings?

Imagine writing:

```
if(token.type.equals("IDENTIFER"))
```

Notice the typo: `"IDENTIFER"`.

The compiler won't catch it.

With an enum:

```
if(token.getType() == TokenType.IDENTIFIER)
```

Java catches mistakes at compile time.

This is exactly why compilers themselves use strongly typed representations internally.

## Step 2: Designing the `Token` class

Now we build the object that flows through the compiler.

```

public class Token {

    private final TokenType type;
    private final String value;
    private final int line;
    private final int column;

    public Token(TokenType type, String value, int line, int column) {
        this.type = type;
        this.value = value;
        this.line = line;
        this.column = column;
    }

    public TokenType getType() { return type; }
    public String getValue() { return value; }
    public int getLine() { return line; }
    public int getColumn() { return column; }
}
```

### Why `final`?

This is a compiler design decision.

Once the lexer says:

```
IDENTIFIER(count)
```

No later stage should accidentally change it into:

```
IDENTIFIER(user)
```

Tokens are immutable.

Real compilers favor immutable data because multiple compiler passes may read the same objects safely.

## Step 3: What does the Lexer object remember?

Now we're designing the machine itself.

```

public class Lexer {

    private final String source;

    private int position;
    private int line;
    private int column;

    private char current;
}
```

This is our entire machine.

Let's understand every field.

### `source`

The complete program.

Example:

```
int x = 5;
```

### `position`

Current index.

|Character|Index|
|---|---|
|`i`|0|
|`n`|1|
|`t`|2|

If `position = 4`, we're reading `x`.

### `line`

Used for errors.

Example:

```
int x = 5;
int y =
```

If an error occurs, we know it's line 2.

### `column`

Even better.

Instead of saying:

> Error on line 2

we can say:

> Error on line 2, column 7

Modern IDEs rely on this precision.

### `current`

This deserves special attention.

Many beginners repeatedly write:

```
source.charAt(position)
```

everywhere.

Instead, the lexer keeps the current character cached.

Why?

Two reasons:

1. Cleaner code.
    
2. Every operation becomes "look at current" rather than repeatedly indexing the string.
    

This mirrors how scanners are implemented in many production compilers.

## Mental Simulation

Let's pretend our lexer has just started.

Source:

```
count=25;
```

Initial state:

|Field|Value|
|---|---|
|`position`|0|
|`line`|1|
|`column`|1|
|`current`|`'c'`|

# 23/09/26 => 08:30 - 09:10 -- 09:45 - 10:30 (85)

## Campus Edge – Compiler & OS Team

## Month 1 • Day 2

## Point 8 Onwards: Designing the Lexer's Input Model and Program Entry (Engineering Notes)

> **Objective:** Before writing a single line of C code, understand what a compiler actually receives from the operating system, why a lexer should consume bytes instead of "characters," and how a C program begins execution.

These notes establish the contracts that our Campus Edge compiler will build on. Every later component (lexer, parser, runtime, bytecode VM) assumes these foundations.

---

## 1. First Principles: What is a File?

Most people think a `.c` file or a `.ces` file is "text."

The operating system disagrees.

## Definition

> **A file is an ordered sequence of bytes stored by the operating system.**

Nothing more.

The operating system does **not** know whether a file contains:

- C source code
    
- Campus Edge scripts
    
- JPEG images
    
- MP3 audio
    
- PDFs
    

It stores and retrieves bytes.

## Warehouse Mental Model

Imagine a warehouse.

Each storage box contains exactly **one byte**.

```text
Storage

+----+----+----+----+----+
|6E  |6F  |64  |65  |73  |
+----+----+----+----+----+
```

The warehouse does not know those bytes spell `"nodes"`.

It simply hands the boxes back when asked.

The **meaning** of those bytes belongs to programs like our compiler.

### Important Consequence

The filesystem's responsibility ends at:

> "Here are your bytes."

Everything after that is our responsibility.

---

## 2. What is a Byte?

Before building a lexer, we need to understand its smallest input.

## Bit

A **bit** is the smallest unit of digital information.

Possible values:

|Value|Meaning|
|---|---|
|`0`|Off|
|`1`|On|

A bit is a single electrical state.

## Byte

A **byte** consists of **8 bits**.

Example:

```text
01100011
```

This is one byte.

A byte is simply an **8-bit number**.

Possible values:

Nothing about a byte says:

- "I'm a letter."
    
- "I'm part of a variable."
    
- "I'm source code."
    

Those meanings come later.

---

## 3. Why Do We Use 8 Bits?

This wasn't inevitable.

Early computers experimented with different byte sizes.

Examples:

- 6-bit
    
- 7-bit
    
- 9-bit
    

IBM's System/360 helped standardize the 8-bit byte.

Modern processors are designed around that convention.

This historical decision still affects every compiler written today.

---

## 4. Characters Are an Interpretation of Bytes

Consider the Campus Edge command:

```text
nodes
```

Humans see five letters.

The computer receives five numbers.

|Character|Decimal|Hex|Binary|
|---|---|---|---|
|`n`|110|`0x6E`|`01101110`|
|`o`|111|`0x6F`|`01101111`|
|`d`|100|`0x64`|`01100100`|
|`e`|101|`0x65`|`01100101`|
|`s`|115|`0x73`|`01110011`|

Notice something important.

`0x6E` is **not** inherently the letter `n`.

It's simply the number **110**.

We choose to interpret 110 as `n` using an encoding like ASCII.

## Key Principle

> **Bytes exist first. Characters are an interpretation.**

This distinction becomes crucial when reading files, network packets, and binary formats.

---

## 5. Why Our Lexer Will Consume Bytes

This was our first major engineering decision.

## Initial Question

Should our lexer read:

- characters?
    
- strings?
    
- bytes?
    

## Final Decision

> The lexer consumes **raw bytes**.

### Why?

Because that matches reality.

The operating system returns bytes.

Our compiler should not depend on hidden conversions.

### Benefits for Campus Edge

|Benefit|Why it matters|
|---|---|
|Realistic|Matches how operating systems behave|
|Reusable|Same lexer can process files and network streams|
|Explicit|We control encoding decisions|
|Low-level|Prepares us for OS concepts|

Later, when Campus Edge receives commands over sockets, those commands will also arrive as **byte streams**.

The lexer won't care whether the bytes came from:

- a file
    
- the network
    
- stdin
    

That is good engineering.

---

## 6. The Cursor: How a Lexer Reads Input

A lexer does not repeatedly read "the whole file."

Instead it maintains a **cursor**.

Imagine sliding your finger across a book.

```text
n o d e s
    ^
  Cursor
```

The cursor always points to exactly one byte.

## Our Cursor State

We designed four pieces of state.

|State|Purpose|
|---|---|
|`position`|Which byte index we're reading|
|`line`|Current source line|
|`column`|Position inside the line|
|`current`|Current byte (stored as an `int`)|

Example.

Source:

```text
nodes
```

Initial state:

|Field|Value|
|---|---|
|Position|0|
|Line|1|
|Column|1|
|Current|`0x6E`|

After one move:

|Field|Value|
|---|---|
|Position|1|
|Line|1|
|Column|2|
|Current|`0x6F`|

The cursor only moves forward.

This gives lexers linear time complexity.

Every byte is processed roughly once.

---

## 7. Designing `advance()` Before Writing Code

Instead of immediately coding, we designed the function contract.

## Purpose

`advance()` moves the cursor exactly one byte forward.

## Behavioral Contract

### Normal character

Current:

```text
c
```

Next:

```text
o
```

`advance()` should:

1. Move the position.
    
2. Update the current byte.
    
3. Increase the column.
    

### Newline

If the next byte is:

```text
\\n
```

then:

- increase the line
    
- reset the column to 1
    

Notice:

The line changes **only when an actual newline byte is read.**

### End of File

This produced an important correction.

Initially we thought reaching EOF should reset the line and column.

That would be wrong.

Example:

```text
nodes
```

There is no newline after `s`.

After moving beyond the final byte:

|State|Value|
|---|---|
|Position|5|
|Line|1|
|Column|6|
|Current|EOF|

Nothing resets.

The cursor simply enters a special EOF state.

---

## 8. What is EOF?

EOF stands for:

> **End Of File**

This is one of C's oldest and most important design decisions.

## The Problem

Suppose a file contains:

```text
nodes
```

Five bytes.

The lexer keeps asking:

> "Give me the next byte."

Eventually there isn't one.

How does the reading function communicate that?

## Why Not `NULL`?

`NULL` is a pointer concept.

A byte is not a pointer.

So this cannot work.

## Why Not Byte 255?

Because 255 is a perfectly valid byte.

The file might legitimately contain:

```text
0xFF
```

We need something outside the byte range.

## C's Elegant Solution

Instead of returning a `char`, functions like `fgetc()` return an **`int`**.

Why?

An `int` can store:

- every valid byte (0–255)
    
- one extra value
    

That extra value is:

```c
EOF
```

Internally it's usually:

```text
-1
```

### Return Values

|Return|Meaning|
|---|---|
|`110`|Byte `n`|
|`111`|Byte `o`|
|`115`|Byte `s`|
|`EOF` (`-1`)|No more bytes|

This is why our cursor's `current` field should eventually be an `int`.

Not because characters are bigger.

Because EOF needs one extra state.

---

## 9. Visualizing EOF

```text
File

[110][111][100][101][115]
   n    o    d    e    s

Cursor movement

0 → 1 → 2 → 3 → 4 → EOF
```

Notice something subtle.

> **EOF is not stored inside the file.**

The file ends after the final byte.

EOF is a signal produced by the reading function.

---

## 10. What is a C Program?

Before writing compiler code, we needed to understand what a C program actually is.

Suppose we create:

```text
lexer.c
```

Writing code inside it does nothing.

It's still just a text file.

Someone has to read and translate it.

That "someone" is the C compiler.

---

## 11. Every Executable Needs an Entry Point

Every C program begins execution at a function called:

```c
int main()
```

Think of a building.

Many rooms exist.

But everyone enters through the main door.

That door is `main()`.

Later our compiler will contain functions like:

- `advance()`
    
- `peek()`
    
- `scan_identifier()`
    
- `scan_number()`
    

None of those run first.

Execution always starts at `main()`.

---

## 12. Understanding `int main()`

We analyzed this line one word at a time.

## `int`

`int` is the **return type**.

It promises:

> "When this function finishes, it will return an integer."

This has nothing to do with resetting memory.

Instead it communicates the program's exit status.

## `main`

This is the program's entry point.

The operating system begins execution here.

## `()`

The parentheses describe what the function receives as input.

Initially we wrote:

```c
int main()
```

Later we'll expand this to receive command-line arguments.

---

## 13. Who Receives `return 0`?

A common misconception was:

> "`return -1` resets memory."

That's incorrect.

When a process finishes:

- the operating system destroys the process
    
- frees its memory
    
- closes resources
    

The return value serves a different purpose.

It tells the operating system whether the program succeeded.

### Convention

|Return|Meaning|
|---|---|
|`0`|Success|
|Non-zero|Error|

Example:

```c
return 0;
```

This literally tells Linux:

> "The compiler finished successfully."

---

## 14. How Does the Compiler Learn Which File to Compile?

Suppose the user runs:

```bash
./ces_compiler script.ces
```

A question naturally appeared.

How does `main()` know about `script.ces`?

The answer begins before our program starts.

## Command Flow

```text
User
   │
   ▼
./ces_compiler script.ces
   │
   ▼
Shell
   │
   ▼
Operating System
   │
   ▼
main()
```

The shell already understands the command.

The operating system then passes that information into `main()`.

---

## 15. Why Unix Passes a List Instead of One Giant String

We compared two possible operating-system designs.

## Option A

Pass one string.

```text
"./ces_compiler script.ces"
```

Problem:

Every program would need to parse:

- spaces
    
- quotes
    
- multiple arguments
    

## Option B

Pass a list.

Example:

```bash
./ces_compiler script.ces
```

becomes:

|Position|Value|
|---|---|
|0|`./ces_compiler`|
|1|`script.ces`|

Another example.

```bash
git commit -m "first commit"
```

becomes:

|Position|Value|
|---|---|
|0|`git`|
|1|`commit`|
|2|`-m`|
|3|`first commit`|

The shell performs parsing once.

Every program receives the same clean interface.

This perfectly matches Unix's philosophy.

> **Do one job well.**

- Shell parses commands.
    
- OS launches programs.
    
- Compiler compiles code.
    

Each layer has one responsibility.

---

## 16. The Real C Entry Signature

The operating system's interface to C is:

```c
int main(int argc, char *argv[])
```

We intentionally postponed explaining the syntax.

For now, only remember the contract.

The OS gives `main()`:

1. how many command-line pieces exist
    
2. the list of those pieces
    

We'll decode every symbol (`argc`, `argv`, `char`, pointers, arrays) from first principles in the next session.

---

## Engineering Decisions Finalized Today

|Decision|Reason|
|---|---|
|Lexer consumes bytes|Matches OS behavior|
|Cursor stores position, line, column, current|Accurate scanning and errors|
|`current` becomes an `int`|Supports EOF|
|EOF is a reading-state signal|Not part of the file|
|`main()` is the program entry|OS starts execution there|
|`return 0` reports success|OS reads the exit status|
|OS passes arguments as a list|Universal interface for every program|

---

## How These Decisions Map to Campus Edge

|Concept|Future Feature|
|---|---|
|Byte stream|Reading `.ces` scripts|
|Cursor|`advance()` and `peek()`|
|Line/column|Compiler error reporting|
|EOF|Safe scanning loops|
|Program entry|Launching the compiler|
|Command-line arguments|`./ces_compiler script.ces`|
|Unix argument model|Future CLI commands like `nodes`, `task`, `compute`|

These aren't isolated C concepts. They are the contracts that every later compiler stage will rely on, from lexical analysis to parsing, semantic analysis, bytecode generation, and eventually executing Campus Edge commands across distributed campus nodes.