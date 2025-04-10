In the example above, we create a new shared pointer using std::make_shared(), which can 
be printed out using dereferencing. We pass it to the takeOwnership function, which passes the 
shared pointer by value (makes a copy of the pointer, not the underlying value). This operation 
increases the reference counter. 
Since we can copy shared pointers, the value is not cleared, and we can still use it. Then, we 
pass it to the takeReference function, which, in contrast, doesn't increment the reference 
count since we only take the reference to this function. You should pass by reference when the 
function's use of the shared_ptr doesn't outlive the caller and pass by value when you need 
the `shared_ptr` to survive independently of the caller.
Now let's see how to use std::weak_ptr:
void useWeakPtr(std::weak_ptr<std::string> weak) {
    // Try to get access to the object
    // .lock() converts weak to shared_ptr if object exists
    if (auto shared = weak.lock()) {  
        std::cout << *shared;         // Safe to use
        // shared_ptr reference count temporarily increases here
    } else {
        std::cout << "Object no longer exists!\n";
    }
}  // shared goes out of scope, count decreases
int main() {
    // Create a shared pointer
    auto str1 = std::make_shared<std::string>("Hello World");
    
    // Create a weak pointer from shared pointer
    std::weak_ptr<std::string> weak1 = str1;  // Doesn't increase 
reference count
    
    std::cout << "Reference count: " << str1.use_count() << "\n";  
// Shows 1
    
    // Use the weak pointer
    useWeakPtr(weak1);  // Object exists
    
    // Reset the shared pointer
    str1.reset();  // Reference count becomes 0, object is 
destroyed
    
    // Try to use weak pointer again
    useWeakPtr(weak1);  // Will print "Object no longer exists!"
    
    return 0;
}
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
96