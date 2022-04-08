---
layout: page
title:  "Embedding"
platform: iOS
step: 9
pageSection: "Customization"
---
<section id="{{page.title | slugify }}" markdown=1>
# Embedding

The Judo SDK can also be integrated with UIKit.  Judo experiences are displayed within an `ExperienceViewController`.  The most common method is to simply present the experience view controller on top of the current view controller. The experience can also be embedded within a custom view, typically using view controller containment.

</section>
<section id="presented-view-controller" markdown=1>
## Presented View Controller

The most straightforward method of displaying an experience is to simply present the `ExperienceViewController`.  The convenience method `Judo.sharedInstance.openURL()` will instantiate the view controller and present it.

### Sample Code

```swift
import UIKit
import JudoSDK

class ViewController: UIViewController {

  override func viewDidLoad() {
    super.viewDidLoad()
    
    if let url = URL(string: "<Your Experience URL>") {
      Judo.sharedInstance.openURL(url, animated: true)
    }
  }
}
```
</section>
<section id="view-controller-containment" markdown=1>
## View Controller Containment

An experience may be embedded within another view controller, by adding the `ExperienceViewController` as a child view controller.  Once the view controller has been added as a child, its frame must be given a fixed size, then its view can be added to the parent.   Once the view has been added to the parent, call `ViewController.didMove(toParent:)`.   When done with the experience, call `ViewController.removeParent()`.

The `ExperienceViewController` does not support autosizing from content, so it will need to be given a fixed size.

Additional details are available at: [Implementing a Container View Controller](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/ImplementingaContainerViewController.html)

### Sample Code

```swift
import UIKit
import JudoSDK

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        if let url = URL(string: "<Your Experience URL>") {
            let experienceVC = ExperienceViewController.init(url: url)
            addChild(experienceVC)
            experienceVC.view.frame = view.bounds. \\Fixed size is required here.
            view.addSubview(experienceVC.view)
            experienceVC.didMove(toParent: self)
        }
    }
}
```
</section>