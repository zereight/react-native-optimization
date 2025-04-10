BEST PRACTICE
USE NATIVE ASSETS FOLDER
A fair chunk of the app size typically comes from assets, such as images, sounds, or videos 
bundled into it. Both iOS and Android have ways to optimize the delivery of such assets based 
on the device's screen size or pixel density. For example, it's a good practice to load larger images 
on a mobile phone so that they appear crisp and clear. Mobile devices typically have large 
resolutions packed into a small screen, which translates to high pixel density. With more pixels 
on the screen, we can show more; hence, larger images are preferred. When used correctly, these 
platform optimization mechanisms can help us load only necessary image resolutions when the 
user downloads the app from the application store.
Remember to also optimize your assets using dedicated tools to reduce their size.
Using size suffixes
When dealing with assets inside a React Native application, it is generally recommended to 
create multiple size variants of the same image: a base size (1x), twice the size (2x), and thrice 
the size (3x). The convention is to use size suffixes in file names, such as @2x or @3x. 
assets/
  ├── image.jpg      // 1x resolution
  ├── image@2x.jpg   // 2x resolution
  └── image@3x.jpg   // 3x resolution
Directory structure for logo.png with size density extensions
When you import such an image with standard require, React Native selects the best one, 
depending on the current device's screen density.
Best Practice: Use Native Assets Folder
The Ultimate Guide to React Native Optimization
170