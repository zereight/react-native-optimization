Time to Interactive (TTI)
Measuring the app's boot-time performance is described by the TTI metric. It measures 
how quickly your app becomes usable after launch. This is the moment when users can start 
interacting with the app without delays or jank. But TTI isn't just about the app's initial load. 
Companies often track variations of this metric, such as Time to Home (how long it takes to 
load the home screen) or Time to Specific Screen (how long it takes to navigate to a key feature). 
These metrics are especially important for apps with complex navigation flows or heavy initial 
data fetching.
If your app takes too long to load, users may abandon it before they even get a chance to explore 
its features or simply get fed up with it being slow anytime they try to do something, making 
them feel miserable and starting to associate your app, and sometimes even its brand, with that 
feeling.
Frames Per Second (FPS)
Once your app is up and running, FPS becomes the key metric for runtime performance. FPS 
measures how smoothly your app responds to user interactions, such as scrolling, swiping, or 
tapping buttons. A high FPS (ideally 60 frames per second or more) ensures that animations 
and transitions feel smooth and natural. When FPS drops, user experience lags, and animations 
begin to stutter, making your app feel unpolished or slow to respond. This is especially critical 
for apps with rich visual content or complex interactions.
What does fast even mean?
As much as we love data and a scientific approach to validating our hypotheses, we realize that 
there is insufficient amount of independent and high-quality research on what "fast" means for 
mobile users. Usually, it's in the form of big tech companies boasting about their conversion 
numbers or paid reports based on surveys. 
Regardless, what's even more important is that the feeling of speed is a moving target. With 
access to higher-end and better-performing devices, while using plenty of different apps on 
a single smartphone, users' expectations are growing higher. It's also worth remembering that 
these expectations are vastly dependent on the demographic—e.g., younger generations will 
typically expect apps to load and operate faster than their parents and grandparents, who are 
used to things taking more time. The expectations and incentives to improve are also different 
for internal enterprise app users compared to individual customers.
What will typically give you the most reliable indication of whether your app is fast enough is 
knowing your users and your competition. The analytics you gather on your users' behavior 
will give you personalized insights into places where they tend to drop. Analyzing whether 
your competitors' apps are objectively faster or slower, on the other hand, will hint at whether 
performance is potentially the reason you're losing users.
In the first two parts of this book, we'll focus on how you can improve the runtime performance 
through JavaScript, React, and native optimizations. We will guide you through all the necessary 
tools and techniques you can utilize to better understand what influences FPS so you can fix it 
anywhere.
Introduction
The Ultimate Guide to React Native Optimization
8