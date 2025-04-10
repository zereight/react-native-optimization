// Simple data manager class
interface DataListener {
    fun onDataChanged(data: String)
}
class DataManager {
    // List to store all registered listeners
    private val listeners = mutableListOf<DataListener>()
    
    // Method to register listeners
    fun registerListener(listener: DataListener) {
        listeners.add(listener)
    }
    
    // Method to unregister listeners
    fun unregisterListener(listener: DataListener) {
        listeners.remove(listener)
    }
    
    // Method to notify listeners of data changes
    fun notifyDataChanged(data: String) {
        listeners.forEach { it.onDataChanged(data) }
    }
}
Then, when we use this class, we have to remember about unregistering the listeners because 
otherwise, we will introduce memory leaks:
class MyClass {
    private val dataManager = DataManager()
    
    private val listener = object : DataListener {
        override fun onDataChanged(data: String) {
            println("Data changed: $data")
        }
    }
    
    init {
        // Register listener but never unregister
        dataManager.registerListener(listener)
    }
    
    // Memory leak: No way to unregister the listener
    // MyClass instance will never be garbage collected
    // as DataManager holds a strong reference to the listener
}
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
102