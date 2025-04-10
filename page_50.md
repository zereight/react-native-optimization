Configure it in your Babel config in the following way:
const ReactCompilerConfig = {
> npm install -D eslint-plugin-react-compiler@beta
React Compiler is compatible with React 17+ applications and libraries. However, 
its effectiveness depends on adherence to React's best practices. It does not optimize 
class components, outdated patterns, or components that break the Rules of React.
To install the plugin, add  eslint-plugin-react-compiler  from  the  beta tag to your dev 
dependencies:
> npm install -D babel-plugin-react-compiler@beta
Then, configure ESLint to enforce compiler rules:
import reactCompiler from 'eslint-plugin-react-compiler'
export default [
  {
    plugins: {
      'react-compiler': reactCompiler,
    },
    rules: {
      'react-compiler/react-compiler': 'error',
    },
  },
]
eslint.config.js 
And you're ready to fix any violations of the Rules of React in your codebase.
Running the compiler
Now that we're all set with the linter, it's time to address the elephant in the room: the Babel 
plugin that holds the compiler:
Best Practice: React Compiler
The Ultimate Guide to React Native Optimization
53