setValue(text);
  };
  return (
    <TextInput
      onChangeText={onChangeText}
-     value={value}
    />
  );
};
As a result, the data will flow only one way, from the native RCTSinglelineTextInputView 
on iOS or AndroidTextInputNativeComponent on Android to the JavaScript side. Native 
components emit onChangeText and control the internal input state on their own, eliminating 
the synchronization issues described earlier.
Best Practice: Uncontrolled Components
The Ultimate Guide to React Native Optimization
33