# Safe-Memory-Reduction
It seems that many competitors use the same `reduce_mem_usage` function (with modifications).
Though I see a few major drawbacks in using it as is:
1. Such functions either:
    1. Don't use minimal possible types for the sake of (imaginary) safety, and therefore use more memory than actually needed.
    2. Use float16 but don't guarantee that you don't lose precision or unique values much (see **Issue 1** below)
2. None of them try to perform float to int conversion.
3. It's done only once and don't allow you to easily minify newly created features.

So my functons address all of these problems.
They allow using really minimal amount of memory and guarantee not losing anything (precision, na values, unique values, etc.).
And you can do minification on the fly for new columns: `df['a/b'] = sd(df['a']/df['b'])`.

Also my `sd` (stands for `safe downcast`) function is very flexible. 
If you consider you can allow to lose 0.1 precision when rounding but wanna save more memory,
then no problem, just set `sd(col, max_loss_limit=0.1, avg_loss_limit=0.1)`.
