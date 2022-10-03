---
layout: post
title:  "How to Create a Kontakt 7 Instrument (Part 2) - Push It!"
date:   2022-10-1 07:00:00 +0800
categories: KONTAKT
tags: kontakt7
---

Welcome to the second installment of my multi-part series about how to engage in the beautiful pursuit of building your own instruments, effects, and plugins in the Kontakt Script Language.

## Prerequisites

This tutorial continues from the [first installment](https://gelvinwhite.com/kontakt/2022/09/30/how-to-create-a-kontakt-7-instrument/) of this multi-part series. So, if you have not followed the instructions there, best to backtrack and complete those steps. Further, this tutorial requires a full version of Kontakt. Likely, Kontakt 5 or later is fine. However, screenshots in this tutorial are of Kontakt 7.

## Getting Started

Continuing from where we left off, please ensure that the following starter code is in place in the scripting window:

    ```
    on init

      message("Hello World!")

    end on
    ```
    Ensure that the script functions by clicking `Apply`. Upon clicking `Apply`, you should see `Hello World!` appearing in the status bar / console at the bottom of the screen.

### Step One - Creating the Performance View

  * To begin, we will create the *performance view* of the instrument. You can think of the performance view as the canvas upon which your buttons, dials, and other graphical elements will appear when someone is using the instrument. Modify your code as follows:

    ```
    on init

      {set the basics of the UI, setting the height at 213 and width at 720}
    	make_perfview
    	set_ui_height_px(213)
    	set_ui_width_px(633)

    end on
    ```
    Notice that `make_perfview` informs the compiler that there is a user interface that needs to be rendered to the user, even when they are not in the script editor. Without this line of code, your user interface will not be visible. Then, `set_ui_height_px` and `set_ui_width_px` tell the compiler the size of the performance view. As of Kontakt 7, the performance view is approximately 633px. Accordingly, anything wider than 633px will cause the window to resize. Anything less than 633px, at least on my computer, does not seem to render anything smaller than this baseline value.

    Most important, notice the curly `{}` braces, which allows you to comment your code. Throughout your process of learning how to program, I highly advise keeping good notes about what your code does! After all, when you return later in a week, month, or year, it's helpful to know what your code is doing.

  * To ensure that your script is functioning, hit `apply`. Then, click the wrench icon to switch to performance view. Your window should look like this:

    ![kontakt wrench](/assets/2022-10-02-how-to-create-a-kontakt-7-instrument-part-2/001.png "kontakt wrench")
    Note that if your screen does not look like the above, you probably did not click that wrench.

### Step Two - Placing Your First Button

  * Next, let's place your first button. Modify your code as follows:

    ```
    on init

    	{set the basics of the UI, setting the height at 213 and width at 720}
    	make_perfview
    	set_ui_height_px(213)
    	set_ui_width_px(633)

    	{declare a variable called $switch that is tied to a button. Set that switch to on }
    	declare ui_switch $switch
    	$switch := 1

    	message("Hello World!")

    end on
    ```
    Notice that the `declare` command is used to create a variable called `switch`. This variable has a `$` sign in front of it, which tells us that this variable is an integer. Further, this variable called `switch` is tied to a button, signified by `ui_switch`. Then `$switch` is set to `1`. When a `ui_switch` has a value of `0` it is off. When it is `1` it is on.

  * Now, click the `Apply` button.

  * Upon clicking `Apply`, you should see the following:

    ![kontakt switch](/assets/2022-10-02-how-to-create-a-kontakt-7-instrument-part-2/002.png "kontakt switch")

  * Click on the button called `switch`. You'll notice that in the *on* position it is lighter in color. In the *off* position, it is darker in color

### Step Three - Making the Button Interactive

  * Now, it's time to learn a little about *callbacks*. A callback is a block of code that is executed at a specific time. For example, the `on init` callback is run each time the script is run. This callback is the first callback to be run, always. You can create multiple callbacks in your program. Modify your code as follows:

    ```
    {this always runs when the script starts}
    on init

    	{set the basics of the UI, setting the height at 213 and width at 720}
    	make_perfview
    	set_ui_height_px(213)
    	set_ui_width_px(633)

    	{declare a variable called $switch that is tied to a button. Set that switch to on }
    	declare ui_switch $switch
    	$switch := 1

    end on

    {this runs when the ui_control called $switch is changed}
    on ui_control($switch)

      if($switch = 1)
        message("it's on!")
      else
        message("it's off!")
      end if
    end on
    ```
    Notice that a new callback called `ui_control($switch)` is included. When `$switch1` is manipulated, this callback is called. Notice that the value of `$switch1` is passed to this callback using the `()` parentheses. Then, inside the callback, the `if` statement makes a decision. If `$switch = 1` or *on*, then `it's on!` is messaged in the console. Otherwise, `it's off` is messaged.

  * Click "Apply" and play with the button. Notice what is displayed in the console.

    ![kontakt console](/assets/2022-10-02-how-to-create-a-kontakt-7-instrument-part-2/003.png "kontakt console")

### Congratulations

You have now created your first interactive button!

The tutorial to follow will discuss the placement of buttons within the performance view.

*Native Instruments®️ and Kontakt®️ are registered trademarks of Native Instruments GmbH. We have no affiliation with Native Instruments.*
