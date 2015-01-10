There are many style guidelines. This one is mine and I like it. 

These guidelines build on the [GitHub Objective-C Style Guide](https://github.com/github/objective-c-style-guide) and [NYTimes Objective-C Style Guide](https://github.com/NYTimes/objective-c-style-guide).
Unless explicitly contradicted below, assume that all of GitHub and Apple's guidelines apply as well.

## Table of Contents
* [Whitespace](#whitespace)
* [Documentation and Organization](#documentation-and-organization)
* [Declarations](#declarations)
* [Methods](#methods)
* [Expressions](#expresssions)
* [Control Structures](#control-structures)
* [Exceptions and Error Handling](#exceptions-and-error-handling)
* [Blocks](#blocks)
* [Literals](#literals)
* [Categories](#categories)

## Whitespace

 * Tabs, not spaces.
 * End files with a newline.
 * Make liberal use of vertical whitespace to divide code into logical chunks.
    * Two returns between methods, a single return inside methods
 * Don’t leave trailing whitespace.
    * Leading indentation on blank lines is okay.

## Documentation and Organization

 * All method declarations should be documented in either the `.h` or a class extension in the `.m` file.
 * Comments should be avoided. Generally comments should only be needed for code that is necessarily complex or solves a problem in a non-obvious way. Still, try to simplify your code before writing a comment.
    * If you must, make them hard-wrapped at 80 characters.
 * Use `#pragma mark`s to categorize methods into functional groupings and protocol implementations, following this general structure:

```objc
#pragma mark Properties

@dynamic someProperty;


- (void)setCustomProperty:(id)value {}


#pragma mark Lifecycle
+ (instancetype)objectWithThing:(id)thing {}


- (instancetype)init {}


#pragma mark Drawing
- (void)drawRect:(CGRect) {}


#pragma mark Another functional grouping


#pragma mark GHSuperclass
- (void)someOverriddenMethod {}


#pragma mark NSCopying
- (id)copyWithZone:(NSZone *)zone {}


#pragma mark NSObject
- (NSString *)description {}
```

## Declarations

 * Never declare an ivar unless you need to change its type from its declared property.
    * Still, you probably shouldn't use ivars.
 * Don’t use line breaks in method declarations.
 * Prefer exposing an immutable type for a property if it being mutable is an implementation detail. This is a valid reason to declare an ivar for a property.
 * Always declare memory-management semantics even on `readonly` properties.
 * Declare properties `readonly` if they are only set once in `-init`.
 * Don't use `@synthesize` unless the compiler requires it. Note that optional properties in protocols must be explicitly synthesized in order to exist.
 * Declare properties `copy` if they return immutable objects and aren't ever mutated in the implementation. `strong` should only be used when exposing a mutable object, or an object that does not conform to `<NSCopying>`.
 * Avoid `weak` properties whenever possible. A long-lived weak reference is usually a code smell that should be refactored out.
 * Instance variables should be prefixed with an underscore (just like when implicitly synthesized).
 * Don't put a space between an object type and the protocol it conforms to.

```objc
@property (attributes) id<Protocol> object;
@property (nonatomic, strong) NSObject<Protocol> *object;
```

 * C function declarations should have no space before the opening parenthesis, and should be namespaced just like a class.

```objc
void GHAwesomeFunction(BOOL hasSomeArgs);
```

 * Constructors should generally return [`instancetype`](http://clang.llvm.org/docs/LanguageExtensions.html#related-result-types) rather than `id`.
 * Prefer C99 struct initialiser syntax to helper functions (such as `CGRectMake()`).

```objc
  CGRect rect = { .origin.x = 3.0, .origin.y = 12.0, .size.width = 15.0, .size.height = 80.0 };
```

## Methods

In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.

**For Example:**

```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## Expressions

 * Don't access an ivar unless you're in `-init`, `-dealloc` or a custom accessor.
 * Use dot-syntax when invoking idempotent methods, including setters and class methods (like `NSFileManager.defaultManager`).
 * Use object literals, boxed expressions, and subscripting over the older, grosser alternatives.
 * Comparisons should be explicit for everything except `BOOL`s.
 * Prefer positive comparisons to negative.
 * Long form ternary operators should be wrapped in parentheses and only used for assignment and arguments.

```objc
Blah *a = (stuff == thing ? foo : bar);
```

* Short form, `nil` coalescing ternary operators should avoid parentheses.

```objc
Blah *b = thingThatCouldBeNil ?: defaultValue;
```

 * Separate binary operands with a single space, but unary operands and casts with none:

```c
void *ptr = &value + 10 * 3;
NewType a = (NewType)b;

for (int i = 0; i < 10; i++) {
    doCoolThings();
}
```

## Control Structures

 * Always surround `if` bodies with curly braces if there is an `else`. Single-line `if` bodies without an `else` should be on the same line as the `if`.
 * All curly braces should begin a new line below their associated statement. They should end on a new line.
 * Put a single space after keywords and before their parentheses.
 * Return and break early.
 * No spaces between parentheses and their contents.

```objc
if (somethingIsBad) return;

if (something == nil) {
	// do stuff
} else {
	// do other stuff
}
```

## Exceptions and Error Handling

 * Don't use exceptions for flow control.
 * Use exceptions only to indicate programmer error.
 * To indicate errors, use an `NSError **` argument or send an error on a [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) signal.
 * When methods return an error parameter by reference, switch on the returned value, not the error variable.

**For example:**

```objective-c
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Not:**

```objective-c
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Blocks

 * Blocks should have a space between their return type and name.
 * Block definitions should omit their return type when possible.
 * Block definitions should omit their arguments if they are `void`.
 * Parameters in block types should be named unless the block is initialized immediately.

```objc
void (^blockName1)(void) = ^{
    // do some things
};

id (^blockName2)(id) = ^ id (id args) {
    // do some things
};
```

## Literals

 * Avoid making numbers a specific type unless necessary (for example, prefer `5` to `5.0`, and `5.3` to `5.3f`).
 * The contents of array and dictionary literals should not have a space on both sides.
 * Dictionary literals should have no space between the key and the colon, and a single space between colon and value.

``` objc
NSArray *theStuff = @[@1, @2, @3];

NSDictionary *keyedStuff = @{GHDidCreateStyleGuide: @YES};
```

 * Longer or more complex literals should be split over multiple lines (optionally with a terminating comma):
 * Do not bother aligning dictionary colons to key length

``` objc
NSArray *theStuff = @[
    @"Got some long string objects in here.",
    [AndSomeModelObjects too],
    @"Moar strings."
];

NSDictionary *keyedStuff = @{
    @"this.key": @"corresponds to this value",
    @"otherKey": @"remoteData.payload",
    @"some": @"more",
    @"JSON": @"keys",
    @"and": @"stuff",
};
```

## Categories

 * Categories should be named for the sort of functionality they provide. Don't create umbrella categories.
 * Category methods should always be prefixed.
 * If you need to expose private methods for subclasses or unit testing, create a class extension named `Class+Private`.
