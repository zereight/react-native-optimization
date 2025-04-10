<MyNativeComponent>
  <Child1/>
  <Child2/>
  <Child3/>
</MyNativeComponent>
MyNativeComponent with 3 child views
In the example above, the MyNativeComponent can expect to receive an array of 3 views on 
the native side. But what happens if view flattening decides that the Child1's container view 
is "layout-only"? You can unexpectedly get all of the children of this view passed to the native 
side. This means that if Child1 had 3 child views inside, you would get 5 children passed to 
your native component instead of 3. To illustrate that:
<MyNativeComponent>
  /* Child1 unexpectedly flattened to 3 Views */
  <View/> 
  <View/>
  <View/>
  <Child2/>
  <Child3/>
</MyNativeComponent>
Greedy view flattening
This is unexpected behavior if you assume the number of JavaScript children will match the 
children's array on the native side. As you can see, it's not a safe assumption and may prompt 
you to validate your component logic. But sometimes, you really need to know the children 
count. To solve it, you can use the collapsable prop, which controls view flattening behavior. 
Setting it to false will prevent the view from being merged with others.
<MyNativeComponent>
  <Child1 collapsable={false} />
  <Child2 collapsable={false} />
  <Child3 collapsable={false} />
</MyNativeComponent>
MyNativeComponent with 3 child views with collapsable prop set to false
After applying this prop, we can be sure that we will always receive 3 child views in our native 
component.
Best Practice: Use View Flattening
The Ultimate Guide to React Native Optimization
114