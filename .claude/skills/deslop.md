# deslop

Remove AI-generated code slop from a branch. Use when cleaning up AI-generated code, removing unnecessary comments, defensive checks, or type casts.

## Instructions

Review all changes on the current branch (compared to the base branch) and clean up AI-generated slop. Look for and fix the following issues:

### Unnecessary comments
- Remove comments that merely restate what the code does (e.g., `// Initialize the variable`, `// Return the result`)
- Remove comments that explain obvious logic
- Keep comments that explain *why* something is done, not *what* is done

### Excessive defensive checks
- Remove null/undefined checks where the type system already guarantees a value
- Remove redundant type guards that duplicate existing narrowing
- Remove unnecessary try/catch blocks around code that cannot throw
- Remove fallback values where the input is already guaranteed (e.g., `?? []` on a value that's already an array)

### Unnecessary type casts
- Remove `as` type assertions that don't change the type or are redundant with inference
- Remove unnecessary generic type parameters that TypeScript can infer
- Remove redundant type annotations on variables where the type is obvious from the assignment

### Verbose or over-engineered patterns
- Simplify overly verbose conditional expressions
- Replace unnecessary abstractions with direct code
- Remove unused imports or variables introduced by AI
- Collapse single-use helper functions that obscure rather than clarify

### Other slop patterns
- Remove empty error handlers or handlers that just re-throw
- Remove unnecessary `async`/`await` on non-async operations
- Remove redundant `return` statements
- Remove unnecessary template literals (e.g., `` `${variable}` `` instead of `variable`)
- Remove unnecessary spread operators (e.g., `{...obj}` when `obj` would suffice)

## Process

1. Run `git diff $(git merge-base HEAD master)..HEAD` to see all changes on the branch
2. For each changed file, read the full file for context
3. Make edits to clean up slop, preserving the intended functionality
4. After all edits, review the changes to ensure nothing was broken
