A flame graph presenting re-render of the only affected TodoList component when toggling item status 
Once again, getting out of the React programming model can benefit our app's overall 
performance. For years, atomic state libraries helped us reduce the overall number of re-renders 
while leveraging React APIs such as hooks and useSyncExternalStore under the hood. Now 
that we have access to the React Compiler, it may be unwise to migrate your whole app to 
a new library for the sake of performance when the compiler can do the heavy work for us. You 
can read more about it in the React Compiler chapter.