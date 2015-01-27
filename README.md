There are many style guidelines. This one is mine and I like it. 

These guidelines build on the [NYTimes Objective-C Style Guide](https://github.com/NYTimes/objective-c-style-guide).
Unless explicitly contradicted below, assume that all of Apple's guidelines apply as well.

## Introduction

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Alphabetization](#alphabetization)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Curly Braces](#curly-braces)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Returns](#returns)
* [Methods](#methods)
* [Variables](#variables)
* [Naming](#naming)
* [Comments](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [Blocks](#blocks)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Private Properties](#private-properties)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Imports](#imports)
* [Lazy Initialization](#lazy-initialization)
* [Categories](#categories)
* [Xcode Project](#xcode-project)

## Alphabetization

Always alphabetize all imports, properties, method declarations in headers, protocol conformation, constants, and `define`s.

**For example:**
```objc
//
//  SomeViewController.h
//  Project
//
//  Created by Adam Yanalunas on 1/27/15.
//  Copyright (c) 2015 Some Company. All rights reserved.
//

#import "SomeCollectionViewController.h"
#import "SomeOtherViewController.h"
#import "SomeResourcePresenter.h"
#import <PSPDFKit/PSPDFKit.h>
#import "UICollectionViewController+OverrideTaps.h"


@interface SomePersonalViewController : SomeCollectionViewController <SomeCollectionViewControllerDelegate, PSPDFCacheDelegate, UICollectionViewDelegateFlowLayout>

@property (nonatomic, strong) NSArray *resources;
@property (nonatomic, strong) NSMutableDictionary *resourceViewers;

- (SomeCollectionViewCell *)cellForResource:(id<SomeResourceProtocol>)resource;
- (void)cleanup;
- (void)closePresenters;
- (NSString *)messageForEmptySection:(NSInteger)section;
- (NSArray *)pathsWithDocument:(PSPDFDocument *)document;
- (NSObject<SomeResourcePresenter> *)presenterFromClass:(__unsafe_unretained Class)presenterClass;
- (void)refreshDataAndView;
- (void)thumbnailCreated:(NSNotification *)notification;

@end
```

**Not:**
**For example:**
```objc
//
//  SomeViewController.h
//  Project
//
//  Created by Adam Yanalunas on 1/27/15.
//  Copyright (c) 2015 Some Company. All rights reserved.
//

#import "UICollectionViewController+OverrideTaps.h"
#import "SomeResourcePresenter.h"
#import "SomeCollectionViewController.h"
#import "SomeOtherViewController.h"

#import <PSPDFKit/PSPDFKit.h>


@interface SomePersonalViewController : SomeCollectionViewController <
UICollectionViewDelegateFlowLayout, 
SomeCollectionViewControllerDelegate, 
PSPDFCacheDelegate
>

@property (nonatomic, strong) NSArray *resources;
- (SomeCollectionViewCell *)cellForResource:(id<SomeResourceProtocol>)resource;
- (void)cleanup;
- (NSString *)messageForEmptySection:(NSInteger)section;
- (NSArray *)pathsWithDocument:(PSPDFDocument *)document;
- (void)closePresenters;

- (NSObject<SomeResourcePresenter> *)presenterFromClass:(__unsafe_unretained Class)presenterClass;
- (void)refreshDataAndView;
- (void)thumbnailCreated:(NSNotification *)notification;

@property (nonatomic, strong) NSMutableDictionary *resourceViewers;
@end
```

## Dot-Notation Syntax

Dot-notation should **always** be used at all opportunities. Bracket notation should be reserved for passing messages that require arguments.

**For example:**
```objc
view.backgroundColor = UIColor.orangeColor;
UIApplication.sharedApplication.delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
[UIApplication sharedApplication].delegate;
```

## Spacing

* Always indent using tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the line below the statement and close on a new line.

**For example:**
```objc
if (user.isHappy)
{
//Do something
}
else
{
//Do something else
}
```
* There should be two blank lines between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Curly Braces

Opening and closing curly braces should be on new lines for method declarations and conditional statements. Blocks should have the opening curly brace on the same line and the closing brace on a newline.

**For example:**
```objc
- (UIView *)animateView:(UIView *)view
{
    if (view.isHidden)
    {
        view.hidden = NO;
    }
    
    [UIView animateWithDuration:.3 animations:^{
        …
    }];
}
```

**Not:**
```objc
- (UIView *)animateView:(UIView *)view {
    if (view.isHidden) {
        view.hidden = NO;}
    
    [UIView animateWithDuration:.3 animations:^
    {
        …
    }];
}
```


## Conditionals

Conditional bodies should always use braces except for single-line conditions, which should be all on the same line.

**For example:**
```objc
if (!error) return success;

if (foo)
{
    [thing do];
    [otherThing dontDo];
}
```

**Not:**
```objc
if (!error)
    return success;
```

### Ternary Operator

The Ternary operator, ? , should only be used when it increases clarity or code neatness. Only ever evaluate a single condition. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables. 

Wrap the expression in parenthesis for clarity. Short form, `nil` coalescing ternary operators should avoid parentheses.

**For example:**
```objc
result = (a > b ? x : y);
```

or

```objc
result = (a ?: b);
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**For example:**
```objc
NSError *error;
if (![self trySomethingWithError:&error])
{
    // Handle Error
}
```

**Not:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error)
{
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Returns

Methods that requrie a return value should do so as the last operation of the method. Use a variable to store the return value as you progress through the method instead of using multiple `return`s throughout. Only in rare cases should a `return` appear anywhere else. `goto` versus `return` to break out of nested loops, for example.

**ForExample:**

```objc
- (BOOL)isCorrect
{
    BOOL correct = NO;
    
    if (someThing)
    {
        NSInteger count = …
        NSString *foo = …
        correct = (count > 5 && [foo isEqualToString:@"bar"]);
    }
    else if (someOtherThing)
    {
        NSURL *url = …
        NSString *owner = …
        correct = (url && owner.isRegistered);
    }
    
    return correct;
 }
 ```
 
 **Not:**
```objc
- (BOOL)isCorrect
{
    if (someThing)
    {
        NSInteger count = …
        NSString *foo = …
        return (count > 5 && [foo isEqualToString:@"bar"]);
    }
    else if (someOtherThing)
    {
        NSURL *url = …
        NSString *owner = …
        return (url && owner.isRegistered);
    }
    
    return NO;
 }
 ```


## Methods

In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.

**For Example**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**For example:**

```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Not:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

#### Variable Qualifiers

When it comes to the variable qualifiers [introduced with ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), the qualifier (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`) should be placed first, e.g., `__weak NSString *text`. 

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**For example:**

```objc
UIButton *settingsButton;
```

**Not**

```objc
UIButton *setBut;
```

A three letter prefix (e.g. `ADY`, `SUB`) should always be used for class names and constants, however may be omitted for Core Data entity names. Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**For example:**

```objc
static const NSTimeInterval ADYArticleNavigationFadeAnimationDuration = 0.3;
```

**Not:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties and local variables should be camel-case with the leading word being lowercase.

Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**For example:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

## init and dealloc

`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

`init` methods should favor optimism and be structured like this:

```objc
- (instancetype)init
{
    self = [super init]; // or call the designated initializer
    if (!self) return nil;
    
    // Custom initialization

    return self;
}
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**For example:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

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

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**For example:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**For example:**

```objc
static NSString * const ADYAboutViewControllerCompanyName = @"Adam Yanalunas";

static const CGFloat ADYImageThumbnailHeight = 50.0;
```

**Not:**

```objc
#define CompanyName @"Adam Yanalunas"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**

```objc
typedef NS_ENUM(NSInteger, ADYAdRequestState) {
    ADYAdRequestStateInactive,
    ADYAdRequestStateLoading
};
```

## Bitmasks

When working with bitmasks, use the `NS_OPTIONS` macro.

**Example:**

```objc
typedef NS_OPTIONS(NSUInteger, ADYAdCategory) {
  ADYAdCategoryAutos      = 1 << 0,
  ADYAdCategoryJobs       = 1 << 1,
  ADYAdCategoryRealState  = 1 << 2,
  ADYAdCategoryTechnology = 1 << 3
};
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `ADYPrivate` or `private`) should never be used unless extending another class.

**For example:**

```objc
@interface ADYAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose should be grouped in respective groups in an Images folder.

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**For example:**

```objc
if (!someObject)
{
}
```

**Not:**

```objc
if (someObject == nil)
{
}
```

-----

**For a `BOOL`, here are two examples:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**

```objc
if (isAwesome == YES) // Never do this.
if ([someObject boolValue] == NO)
```

-----

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance
{
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Imports

If there is more than one import statement, group the statements [together](http://ashfurrow.com/blog/structuring-modern-objective-c). Commenting each group is optional. Always alphabetize your imports.

Note: For modules use the [@import](http://clang.llvm.org/docs/Modules.html#using-modules) syntax.

```objc
// Frameworks
@import QuartzCore;

// Models
#import "ADYUser.h"

// Views
#import "ADYButton.h"
#import "ADYUserView.h"
```

## Lazy Initialization

Lazy initialization should be used with caution. Don't rely on it to create objects on any execution path. When called for favor leftmost/optimistic structure.

**For example:**

```objc
- (UIView *)customView
{
    if (_customView) return customView;
    
    _customView = SomeFancyView.new;
    _customView.translatesAutoresizingMaskIntoConstraints = NO;
    _customView.tintColor = UIColor.darkGrayColor;
    
    return _customView;
}
```

**Not:**

```objc
- (UIView *)customView
{
    if (!_customView)
    {
        _customView = SomeFancyView.new;
        _customView.translatesAutoresizingMaskIntoConstraints = NO;
        _customView.tintColor = UIColor.darkGrayColor;
    }
    
    return _customView;
}
```

## Categories

 * Categories should be named for the sort of functionality they provide. Don't create umbrella categories.
 * Category methods should always be prefixed.
 * If you need to expose private methods for subclasses or unit testing, create a class extension named `Class+Private`.

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Other Objective-C Style Guides

If ours doesn't fit your tastes, have a look at some other style guides:

* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
