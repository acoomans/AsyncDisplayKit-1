---
layout: page
title: Smooth asynchronous user interfaces for iOS apps
---

![logo]({{ site.baseurl }}/assets/logo.png)

AsyncDisplayKit is an iOS framework that keeps even the most complex user
interfaces smooth and responsive.  It was originally built to make Facebook's
[Paper](https://facebook.com/paper) possible, and goes hand-in-hand with
[pop](https://github.com/facebook/pop)'s physics-based animations &mdash; but
it's just as powerful with UIKit Dynamics and conventional app designs.


<br />
### Quick start

ASDK is available on [CocoaPods](http://cocoapods.org).  Add the following to your Podfile:

```ruby
pod 'AsyncDisplayKit'
```

Import the framework header, or create an [Objective-C bridging
header](https://developer.apple.com/library/ios/documentation/swift/conceptual/buildingcocoaapps/MixandMatch.html)
if you're using Swift:

```objective-c
#import <AsyncDisplayKit/AsyncDisplayKit.h>
```

AsyncDisplayKit Nodes are a thread-safe abstraction layer over UIViews and
CALayers:

![logo]({{ site.baseurl }}/assets/node-view-layer.png)

You can construct entire node hierarchies in parallel, or instantiate and size
a single node on a background thread &mdash; for example, you could do
something like this in a UIViewController:

```objective-c
dispatch_async(_backgroundQueue, ^{
  ASTextNode *node = [[ASTextNode alloc] init];
  node.attributedString = [[NSAttributedString alloc] initWithString:@"hello!"
                                                          attributes:nil];
  [node measure:CGSizeMake(screenWidth, FLT_MAX)];
  node.frame = (CGRect){ CGPointZero, node.calculatedSize };

  // self.view isn't a node, so we can only use it on the main thread
  dispatch_sync(dispatch_get_main_queue(), ^{
    [self.view addSubview:node.view];
  });
});
```

You can use `ASImageNode` and `ASTextNode` as drop-in replacements for
UIImageView and UITextView, or [create your own
nodes](https://github.com/facebook/AsyncDisplayKit/blob/master/AsyncDisplayKit/ASDisplayNode%2BSubclasses.h)
to implement node hierarchies or custom drawing.  `ASTableView` is a node-aware
UITableView subclass that can asynchronously preload cell nodes without
blocking the main thread.


<br />
### Learn more

* Read the [Getting Started guide]({{ site.baseurl }}/guide)
* Get the [sample projects](https://github.com/facebook/AsyncDisplayKit/tree/master/examples)
* Browse the [API reference]({{ site.baseurl }}/appledoc)
* Watch the [NSLondon talk](http://www.youtube.com/watch?v=h4QDbgB7RLo)
