# Scratchblocks Language Specification

**Author:** Nathan Dinsmore
<br>
**Contributors:** Tim Radvan, Kartik Chandra

## Introduction

The scratchblocks language allows users to describe [Scratch][] scripts with a human-readable text format instead of the visual or JSON formats that Scratch uses. Its syntax mimics the way blocks appear in the Scratch studio by using special characters to delimit visual boundaries.

This document formally and completely specifies the scratchblocks language. Implementors should use it as the definitive reference for the language syntax and semantics. Issues should be [reported][repo-new-issue] on the [Github repository][repo], and changes should be proposed by [filing][repo-new-pull] a pull request.

The formal grammar for scratchblocks programs is a [parsing expression grammar][peg].

## Syntax

Each document consists of zero or more scripts surrounded by optional whitespace.x

    document = WS? (script WS?)*

**Note:** This implies that an empty document valid.

Each script begins with an optional hat block and must contain at least one block in total. Scripts can also be individual reporter blocks.

    script = (hat / block) (WS block)* / reporter

Primitive hat blocks and block definitions are recognized as hats, as are normal blocks with hat annotations:

    hat = parts "::" S? hat_annotations
        / args "when" args green_flag args "clicked" args
        / args "when" args "key" args "pressed" args
        / args "when" args "this" args "sprite" args "clicked" args
        / args "when" args "backdrop" args "switches" args "to" args
        / args "when" args ">" args
        / args "when" args "I" args "receive" args
        / "define" S block?

    green_flag = "gf"
               / "flag"
               / "green" args "flag"
               / "@green-flag"

Boolean reporter blocks are delimited with angle brackets. Other reporters are delimited with parentheses.

    reporter = "(" block ")" / "<" block ">" / "〈" block "〉"

Blocks have a flexible syntax. They may contain any number of labels, inputs, or nested reporter blocks, collectively referred to as **parts**. They may optionally have **annotations**, which tell the parser how to display the block

    block = parts ("::" S? non_hat_annotations)?

    parts = S? part (S part)* S?

    part = arg / label

    args = (S arg)* S?
    arg = num_input / bool_input / color_input / num_dropdown_input / dropdown_input / text_input / reporter

### Parts

Labels can contain any characters except whitespace

    label = ! ("::" (S / EOF)) [^}>)\] \t\r\n]+

Numeric inputs can contain any sequence of numeric characters, even if the result is not a valid number. This matches the behavior of the Scratch studio.

    num_input = "(" NUMBER (")" / & (NL / EOF))

    NUMBER = [0-9.e\x2D]*

Boolean inputs are empty angle brackets. They may contain whitespace.

    bool_input = "<" S? (">" / & (NL / EOF)) / "〈" S? ("〉" / & (NL / EOF))

Color inputs look like text inputs, but use hexadecimal color notation.

    color_input = "[" COLOR ("]" / & (NL / EOF))

    COLOR = ("#" [0-9a-f]i [0-9a-f]i [0-9a-f]i ([0-9a-f]i [0-9a-f]i [0-9a-f]i)?)

Dropdown inputs end with ` v`.

    num_dropdown_input = "(" NUMBER " v" ( ")" / & (NL / EOF))
    dropdown_input = "[" (! " v" [^\x5D])* " v" ("]" / & (NL / EOF))

Text inputs may contain any character except a closing bracket.

    text_input = "[" [^\x5D]* ("]" / & (NL / EOF))

### Annotations

Annotations allow the user to explicitly describe the shape, color, and other attributes of a block.

    hat_annotations = & (non_hat_annotations S "hat") annotations
    non_hat_annotations = non_hat_annotation (S non_hat_annotation)*
    annotations = ANNOTATION (S ANNOTATION)*

    non_hat_annotation = ! ("hat" ([ \t\r\n\])>] / EOF)) ANNOTATION
    ANNOTATION = [^ \t\r\n\])>]+

### Whitespace

    WS = ((S? NL)+ S?)
    S = [ \t]+
    NL = [\r\n]+
    EOF = ! .

<!-- References -->

[scratch]: http://scratch.mit.edu/ "The Scratch programming language website"
[repo]: https://github.com/scratchblocks/spec/ "scratchblocks/spec on Github"
[repo-new-issue]: https://github.com/scratchblocks/spec/issues/new/ "Report an issue with scratchblocks/spec on Github"
[repo-new-pull]: https://github.com/scratchblocks/spec/compare/ "Send a pull request to scratchblocks/spec on Github"
[peg]: http://en.wikipedia.org/wiki/Parsing_expression_grammar "Parsing expression grammar on Wikipedia"
