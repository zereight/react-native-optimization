mount of the main useEffect of, for example, a home screen. Eventually, find a better spot, 
such as when a top-level content of your app, that users typically start with, is displayed:
export default HomeScreen() {
  useEffect(() => {}, [])
  return <TabNavigator {...} />
}
screenInteractive marker in React app
Putting it all together, you'll have a bird's eye view of the whole initialization pipeline of your 
app—from the native init to the screen, or ideally, multiple different screens, to being interactive. 
You can send this data to a database or a real-time user metrics platform, observe how your 
app's performance is affected on various devices, and—what's even more important—how it 
changes over time. Having this data, you'll be able to make exact correlations between TTI, 
and your app's customer success metrics. This data can and should influence the priorities 
of your engineering team, giving you signs that it's either time to optimize or cut back on 
optimizations that are not effective anymore and move focus somewhere else.
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
92