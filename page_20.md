Most people perceive 60 FPS as smooth motion. However, as technology advances, users have 
access to screens that can refresh 120 or even 240 times a second. Once a user enjoys a 120 Hz 
screen, chances are they'll consider 60 Hz "choppy," "laggy," or "slow". It means that our target 
of 16,6 ms to render a frame (1/60 s) is moving, and we should aim for 8,3 ms per frame.
If you fail to think about your app's performance and choose the right tools to 
tackle this challenge, you're on the right track to dropping frames sooner or later.
React Perf Monitor
Thankfully, React Native comes with a handy tool called React Perf Monitor, which can display 
FPS live as an overlay on top of your app. You can open it by selecting it from the React Native 
Dev Menu.
You can open DevMenu by shaking the device or by using dedicated keyboard 
shortcuts:
 ■iOS Simulator: Ctrl + Cmd ⌘ + Z (or Device> Shake).
 ■Android emulators: (Cmd ⌘ / Ctrl )+ M.
 React Native Dev Menu drawer  Perf Monitor visible in the top-left corner
It opens a pane in the upper left corner of the application. You can hide it with the "Hide Perf 
Monitor" option by accessing the React DevMenu once again.
Guide: How to Measure JS FPS
The Ultimate Guide to React Native Optimization
22