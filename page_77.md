GUIDE
HOW TO MEASURE TTI
The app's Time to Interactive (TTI) metric is one of the two most important metrics every 
app should track. It tells us how much time elapses from touching the app icon to displaying 
meaningful content that users can interact with through touch, voice, or other input methods. 
Users expect this process to be as fast as possible, ideally instant. 
Various reports of mixed quality indicate that users typically expect an app to load in under 
2 to 4 seconds; otherwise, they will drop for alternatives (if available). Our experience tells 
us, however, that you should trust your data and take external reports with a healthy dose of 
skepticism. The same experience also tells us that the TTI should be as low as possible. 
But where to draw the line between the optimal effort and outcome? Is it worth spending 
months of the engineering team's capacity to get from 2.9s to 2.3s on a reference Android 
device? Your particular user base is unique, and only observing user behavior can give you an 
answer. In this chapter, we'll focus on the techniques to get you to the lowest TTI possible.
Measuring TTI reliably
Depending on the underlying OS and its capabilities, our app can perform a cold start, warm 
or hot start, or it can be prewarmed. This means the same news app you open every day around 
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
85