IILinq
======

IILinq (the name is probably temporary) is small Objective-C library that defines LINQ like operators on NSArrays.
It's highly inspired by the .Net LINQ framework, but adapted to encorporate more Cocoa-isms.

Basically, it allows you to chain "array operators", but it will handle enumeration smartly. That is: it won't generate intermediate arrays unless there's no other option.
This is different from similar implementations like Yolokit, Underscore.m or Coby that do work with intermediate arrays.

## Key features

* It's inspired by the LINQ methods of .Net. But also a bit by Ruby.
* It does not hijack NSArray (but does define a category with convenience methods)
* It tries to defer enumeration and processing of blocks as much as possible
* Like YoloKit, it's forgiving. It doesn't crash unless there's no other way.
* It works with objects, not primitives.
* It's extensible

## Examples

So, you can do stuff like this:

```objc
NSArray *bunchOfInts = @[@7, @9, @7, @2, @0, @4];

NSArray *result = bunchOfInts.ii_linq.select(^id(id item) {
  return [NSString stringWithFormat:@"%@%@", item, item];
}).allObjects;

// result = @[@"77", @"99", @"77", @"22", @"00", @"44"]
```

or you can use the shorthand (`ii_linq.where` -> `ii_where`):

```objc
NSArray *bunchOfInts = @[@7, @9, @7, @2, @0, @4];

NSArray *result = bunchOfInts.ii_where(^BOOL(id item) {
  return [item integerValue] > 5;
}).select(^id(id item) {
  return [NSString stringWithFormat:@"%@%@", item, item];
}).allObjects;

// result = @[@"77", @"99", @"77"]
```

There's some ruby inspired stuff:

```objc
id sum = @[@1, @2, @3, @4].ii_inject(@0, ^(NSNumber* result, NSNumber* element) {
    return @([result integerValue] + [element integerValue]);
});
NSLog(@"result = %@", sum);

// prints "result = 10"
```

It will behave properly when just enumerating:

```objc
NSArray *bunchOfInts = @[@7, @9, @7, @2, @0, @4];

IILinq *filtered = bunchOfInts.ii_skip(2).select(^id(NSNumber* num) {
  NSLog(@"processing %@", num);
  return [NSString stringWithFormat:@"%@%@", num, num];
});

for (NSString* item in filtered) {
  NSLog(@"Got %@", item);
  if ([item isEqualToString:@"00"]) {
    break;
  }
}

// Logs:
// processing 7
// Got 77
// processing 2
// Got 22
// processing 0
// Got 00
```

## Testing

Tests are based on [Mxcl](https://github.com/mxcl)'s ruby based test "framework".

Running tests: cd into the root folder of the project then:

```
tests/xctest.rb
```

And that's it.

## Current state

* Highly experimental.
* No example project.
* Unfinished:
  * there's a bunch of LINQ operators left to port
  * add some more cocoa-isms
  * more tests
* No idea about performance but I guess it's pretty good

# License

**IILinq** published under the MIT license:

*Copyright (C) 2014, Tom Adriaenssen*

*Permission is hereby granted, free of charge, to any person obtaining a copy of*
*this software and associated documentation files (the "Software"), to deal in*
*the Software without restriction, including without limitation the rights to*
*use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies*
*of the Software, and to permit persons to whom the Software is furnished to do*
*so, subject to the following conditions:*

*The above copyright notice and this permission notice shall be included in all*
*copies or substantial portions of the Software.*

*THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR*
*IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,*
*FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE*
*AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER*
*LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,*
*OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE*
*SOFTWARE.*
