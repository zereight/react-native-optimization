A diagram showing what happens while typing TEST
As soon as the user starts inputting a new character into the native input, an update is sent to 
React Native via the onChangeText  prop (operation 1 on the diagram above). React processes 
that information and updates its state accordingly by calling setState. Next, a controlled 
component synchronizes its JavaScript value with the native component value (operation 2 on 
the diagram above).
There are benefits to such an approach. React is a source of truth that dictates the value of your 
inputs. This technique lets you alter the user input as it happens by performing a validation, 
masking it, or completely modifying it.
Unfortunately, while ultimately cleaner and more compliant with the way React works, the 
approach mentioned above has one downside. It is most noticeable when there are limited 
resources available and/or the user is typing at a very high rate. 
      onChangeText={onChangeText} 
      value={value} 
    />
  );
};
The above code sample will cause the native text input to update its value through the React 
model whenever a user inputs text. The speed at which input comes is often quite fast or even 
guided by some autocomplete. Pair that with a low-end Android device or an app doing some 
heavy computing while the user is typing, and your users may end up with lags and dropped 
frames. Even worse, in legacy asynchronous architecture, your React state may get out of sync 
with the native input state, causing unexpected behaviors. 
The problem with controlled TextInput de-synchronization shouldn't exist in 
New Architecture. 
To better understand what is happening here, let's first look at the order of standard operations 
that occur while the user is typing and populating your <TextInput /> with new characters.
Best Practice: Uncontrolled Components
The Ultimate Guide to React Native Optimization
31