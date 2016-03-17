# Learning C++ - Session 1 - Whats new with C++? 

**Session 1 on 20160317**

## Resources:
* [http://www.cplusplus.com/](http://www.cplusplus.com/)
* [http://en.cppreference.com/w/](http://en.cppreference.com/w/)
* [C plus plus](github.com/cplusplus/draft)

## New in C++ 11 onwards

* `> >` vs `>>` e.g. in `map<string, vector<int> > m;` - pre c++ 11 would treat `>>` as bit shift and hence the spacing, now OK with `>>`
* `auto`: Lesser typing, sets LHS type properly based on RHS. However lesser readability.
* auto vs auto&: c++ 14 `decltype(auto) a7 = ref` will get exactly what ref is
* Range based loop, now you can do `for (int i: v) {}`
* Range based loops on maps, use const & to not make a copy each time.
* constexpr: compile time instant expressions
* static_assert - To throw build time errors instead of runtime (to enforce constraints)
* Init with `{}` is uniform/standard everywhere
* Pre c++ 11, every constructor in a class had to init every variable. Not anymore.
* Delegating constructors, one constructor can call other.
* `override` & `final`: In parent, you can set function as final so child cannot override it. What override does is that if parent has a virtual func and you mark child fncion as override, then compile will break if someone removes the parent function.
* `final` classes - No child classes can be inherited from this class.
* By default classes are copy constructable. default and delete methods (*check*)
* nullptr: Replaces oldstyle NULL. type is `std::nullptr_t` and is implicitly convertible to any other type (but then no more int type ops?)
* enum class: e.g. `enum class Color {BLUE, BLACK, RED}; auto c = Color::RED;`
* Fixed width integers e.g. int8_t, int16_t etc.
* `std::chrono` for secs/msec/years? e.g in `void read(int timeout)`. Instead `void read(std::chrono::milliseconds timeout);`
* ^^^ So you could do 50_ms or 10_s ("common/time/TimeLiterals.h")
* Smart pointers. `std::shared_ptr` based on boost::shared_ptr and `std::unique_ptr` (replaces `std::auto_ptr`)
* `shared_ptr` reference counts to the underlying object.
* rvalue references: Transferring expensive objects without much overhead (giving ownership via a tmp reference)
* `&&` - "vampire init" - use it and destroy it. See `std::move`. (Caller does'nt need passed arg anymore?)
* lvalue: `foo(MyObj& o)` - full access, `const MyObj& o` - use but no change. `MyObj&& o` -> ???
* `std::move()` - cast an lvalue into a rvalue - Once you've std::move'ed then **do not** reuse that var in caller.
* If you pass in a literal/rvalue directly, then `std::move` is implicit (*check*)
* Caveat: Rvalue references turn into regular reference inside a function body.
* `std::unique_ptr` - Movable but not copyable. Single owner. Single copy. Destroyed when out of scope.

## Functional programming

* Functor + lambdas
* e.g. `auto lessThan = [](int a, int b) { return a < b; };`
* e.g. `int sum{0}; auto addN = [&sum](int x) { sum += n; };` - Be wary of returning lambdas outside scope (of variables used in it)

### Lambdas syntax breakup

```
auto fn = [x, &y](int a, intb) {
	//body
};
```

* `[=]` - Capture everything, By Value. Preferrable (beginning)
* `[&]` - Capture by reference. Captured vars values will be changed outside of lambda too.
* Move capture - If you want c++ 11 behaviour in 14, then folly/MoveWrapper.h (but avoid)
* Type of lambda? (we used auto so far) - Unnamed type. Cannot refer to it by name (each is a *new type* created for you)
* So how to store/copy it? - `std::function` - e.g. `std::function <bool(Const Foo&, int)> fn;` can be lvalue for a lambda that operates on Foo/int and returns an int (i.e. same signature)
* Template types (*Analogous to auto for a lambda? check*)

### Concurrency

* Threads.
* `std::thread` and then join() etc.
* Syncronization: `std::atomic` - Low level atomic access & mem barriers. `std::mutex` - High level.
* Handshakes between threads (reader and writer) i.e sync cant be onesided
* `std::atomic` - Guarantees indivisible effect 
* `std::mutex` - RTFM
* Futures `std::future`, `std::promise` and `std::async` - Prefer [Wangle](https://github.com/facebook/wangle)

### New template features

* Variadic templates - arbitrary number of args - folly/Conv.h
* `auto call(Fn , Args&&... args) { return f(std::forward<Args>(args)...)}; }`
* auto return type ^^^ (c++ 14 will figure it out for you... c++11 not so well)
* Can `std::forward` be used anywhere `std::move` can be used? You specify arg type to fwd but not move. (*check*)
* `std::tuple` - arbitrary number of values. `auto t = make_tuple(8, "hello", true);`. 
* `int x; string s; bool b; tie(x, s, b) = t;` - similar to `a, b = b, a` in python/ruby etc. Copied by reference?
* `tie()` temp object, can call assignments on it.
* `enable_if`, type traits, decltype, variable templates etc
* Can do variable templates.

### Literal changes

* User defined literals e.g `10_ms`
* UTF8 & raw e.g. `u8"hello"` or `R"hello"` - Preferably use urf8 and keep it simple.
* Binary values like `0b0110...`
* All `std::list::size()` is O(1) and not O(n)
* `std::string::data()` is null terminated, **always**.
* `std::regex` - Proper support/features gcc 4.9+ onwards only. Clang support - Not sure. Support is still nascent and pick regex lib based on project needs.




