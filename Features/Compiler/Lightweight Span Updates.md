When doing incremental compilation, it is important to separate code being repositioned / moved from the code being altered. Ideally, code being moved should avoid having to recompile any sections, but should maintain knowledge of where, for example, variables are declared and used, to allow for better error messages.

In order to solve this, character spans in the Diom compiler also include the index of the token that they were originally sourced from.

This means that, after the lexing and code diffing has been performed, the compiler can split into 2 distinct passes:
1. Full recompilation of any spans with semantic changes
2. "Skipped updates" to any spans where code has been moved, i.e. only the spans attached to tokens have changed.

Skipped updates include the standard [[Red-Green Colouring]] techniques, but only use these to determine the nodes that need to be updated in the final, fully labelled AST. These nodes can then be updated by finding their original token used, locating its new position after the code diff and applying the new span resulting from it.

<!--
This needs a diagram.
Show:
1. the index giving the token in the cached copy
2. the diff giving the index in the new copy
3. the new copy giving the updated span
-->
