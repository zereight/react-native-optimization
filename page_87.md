C++
In C++ programming language, memory management is explicit and manual. However, long 
gone are the times when you had to manually call the new and delete operators for all memory 
management. Instead, you can use smart pointers, which the C++ standard library (called std) 
provides. This way, you don't have to manually allocate and deallocate memory, resulting in 
fewer memory leaks in your application.
Further C++ materials assume some knowledge about the concept of pointers in 
C and C++ languages. If you're not familiar with it, feel free to learn more or skip 
this part.
There are multiple smart pointers available at your disposal: 
 ■std::unique_ptr<T>—it cannot be copied to another unique_ptr, passed by value to 
a function, or used in any C++ Standard Library algorithm that requires copies to be 
made. A unique_ptr can only be moved.
 ■std::shared_ptr<T>—reference-counted smart pointer. Use it when you want to 
assign one raw pointer to multiple owners, for example, when you return a copy of a 
pointer from a container but want to keep the original.
 ■std::weak_ptr<T>—special-case smart pointer for use in conjunction with shared_
ptr. A weak_ptr provides access to an object owned by one or more shared_ptr 
instances but does not participate in reference counting.
The most commonly used smart pointer is a std::shared_ptr<T>, which is a wrapper object 
providing a reference counting mechanism for the specified type, a memory management 
mechanism known from Obj-C and Swift. Let's go over an example usage of smart pointers: 
Let's start with std::unique_ptr:
void takeOwnership(std::unique_ptr<std::string> s1) {
    std::cout << *s1;
    // Gets automatically deleted when function ends
}
int main() 
{
    auto str1 = std::make_unique<std::string>("Hello World");
    
    std::cout << *str1;
    
    // Can only be moved. Doesn't allow for copying
    takeOwnership(std::move(str1));
    
    // str1 is cleared here
    
    return 0;
}
In the example above, we create a new unique pointer holding a std::string  using  make_
unique. Then, using dereferencing, we can print out the value of the string. Additionally,  the 
example shows the uniqueness of this smart pointer by transferring the ownership to a different 
function, which can only be done by moving. This is useful when you want to have exclusive 
ownership over the pointer for example when building immutable data structures.
Now, let's explore std::shared_ptr. We'll use the same example to make things easier to 
understand:
void takeOwnership(std::shared_ptr<std::string> s1) {
    std::cout << *s1;
    // After function returns, reference count gets back to 1
}
// We accept a reference
void takeReference(const std::shared_ptr<std::string> &s1) {
  std::cout << *s1;
}
int main() 
{
      // Create a shared pointer
    auto str1 = std::make_shared<std::string>("Hello World");
    
    std::cout << *str1;
    
    // We increase the reference count by 1 and create a copy of 
    the pointer (not underlying values)
    takeOwnership(str1);
    
    // Since we didn't move the object it can be still accessed 
    here
    std::cout << *str1;
    
    // We don't alter the reference count since we pass a reference 
    here
    // This doesn't copy anything
    takeReference(str1);
    
    std::cout << *str1;
    
    return 0;
}
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
95