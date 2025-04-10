class Person {
    let name: String
    
    init(name: String) {
        self.name = name
    }
}
do {
    let person1 = Person(name: "John") // Reference Count: 1
    
    do {
        let person2 = person1  // Reference count: 2
    } // person2 goes out of scope, Reference count: 1
    
} // person1 goes out of scope, Reference count: 0
When we assign person1 to person2, we increase the reference count (we do not create a copy!). 
Then, after we go out of the do block scope, the reference count is set back to 1 and then to 0.
Sources of Memory Leaks
Let's examine some common patterns that cause our app to leak memory.
Forgetting to deallocate in manual management
When using manual memory management, it's easy to introduce a leak. As mentioned before, 
if you choose manual management, you are on your own. With great power comes great 
responsibility. To show you how easy it is to introduce a leak, let's consider this example:
int main() 
{
    std::string *str1 = new std::string {"Hey"};
    
    std::cout << *str1;
    
    // Oops! Memory leak, we forgot to delete str1.
    
    return 0;
}
You allocate a new std::string, print its value, and, at the end, we forgot to delete this string; 
therefore, we introduced a memory leak. To fix it, you need to call delete when a resource is 
not needed:
int main() 
{
    std::string *str1 = new std::string {"Hey"};
    // ...
    delete str1;
 
    return 0;
}
Another option would be to allocate this on the stack (dropping the new), which will 
automatically deallocate str1 when it goes out of scope:
int main() 
{
    std::string str1 = std::string {"Hey"};
    
    cout << str1;
    
    return 0;
}
You should always think twice before putting something on the heap, as it has a significantly 
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
100