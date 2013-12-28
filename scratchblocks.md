# Scratchblocks Language Specification

**Author:** Nathan Dinsmore
<br>
**Contributors:** Tim Radvan, Kartik Chandra

## Introduction

The scratchblocks language allows users to describe [Scratch][] scripts with a human-readable text format instead of the visual or JSON formats that Scratch uses. Its syntax mimics the way blocks appear in the Scratch studio by using special characters to delimit visual boundaries.

This document formally and completely specifies the scratchblocks language. Implementors should use it as the definitive reference for the language syntax and semantics. Issues should be [reported][repo-new-issue] on the [Github repository][repo], and changes should be proposed by [filing][repo-new-pull] a pull request.

<!-- References -->

[scratch]: http://scratch.mit.edu/ "The Scratch programming language website"
[repo]: https://github.com/scratchblocks/spec/ "scratchblocks/spec on Github"
[repo-new-issue]: https://github.com/scratchblocks/spec/issues/new/ "Report an issue with scratchblocks/spec on Github"
[repo-new-pull]: https://github.com/scratchblocks/spec/compare/ "Send a pull request to scratchblocks/spec on Github"
