Is it time to remove manual memoization?
Not yet. While the React Compiler automates memoization, it does not fully replace 
React.memo, useMemo, or useCallback in all cases. If your code heavily relies on these 
optimizations, you should keep them in place until the compiler reaches a stable release 
and official recommendations from the React team confirm that manual memoization is no 
longer needed. We expect the linter to guide you on which optimizations are unnecessary.
What performance improvements can you expect?
React Compiler aims to reduce unnecessary re-renders and minimize the need for manual 
optimizations. By automatically memoizing component calculations, it prevents cascading 
updates and improves performance, particularly in large applications with deep component 
trees. 
Testing the React Compiler on the Expensify app showed measurable improvements in 
performance. For example, one of its most important metrics, "Chat Finder Page Time to 
Interactive" (effectively TTI) improved by 4.3%. While the React Compiler significantly reduces 
unnecessary re-renders, applications already optimized with manual memoization techniques 
may see only marginal improvements.
Best Practice: React Compiler
The Ultimate Guide to React Native Optimization
56