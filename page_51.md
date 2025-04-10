target: '19' // <- pick your 'react' version
};
module.exports = function () {
  return {
    plugins: [
      ['babel-plugin-react-compiler', ReactCompilerConfig],
    ],
  };
};
babel.config.js 
If you want to use React Compiler on a project that's lower than React Native 0.78 (which 
ships with React 19), you'll need to add one extra package: react-compiler-runtime@beta 
and configure the target to "18" accordingly in ReactCompilerConfig. Now you're all set! 
Or almost set.
You will likely notice that compiler may fail on certain files. For such occasions, the Babel 
plugin allows for some level of customization with sources config, where you can skip some 
components or even whole directories:
const ReactCompilerConfig = {
  sources: (filename) => {
    return filename.indexOf('src/path/to/dir') !== -1;
  },
};
babel.config.js
This way you, can add React Compiler incrementally into your project, lowering the risk of 
regressions caused by bugs in beta software. Let's see how the compiler affects our source code 
with a short example of a TextInput with onChangeText callback:
export default function MyApp() {
  const [value, setValue] = useState('');
  return (
    <TextInput
      onChangeText={() => {
        setValue(value);
      }}>
      Hello World
    </TextInput>
  );
}
Source code before running compiler
Best Practice: React Compiler
The Ultimate Guide to React Native Optimization
54