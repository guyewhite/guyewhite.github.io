---
layout: post
title:  "How to Create a Kontakt 7 Instrument (Part 3) - Switcheroo"
date:   2022-10-3 07:00:00 +0800
categories: KONTAKT
tags: kontakt7
---

Welcome to the third installment of my multi-part series about how to engage in the beautiful pursuit of building your own instruments, effects, and plugins in the Kontakt Script Language.

## Prerequisites

This tutorial continues from the [second installment](https://gelvinwhite.com/kontakt/2022/10/01/how-to-create-a-kontakt-7-instrument-part-2/) of this multi-part series. So, if you have not followed the instructions in our previous installments, best to backtrack and complete those steps. Further, this tutorial requires a full version of Kontakt. Likely, Kontakt 5 or later is fine. However, screenshots in this tutorial are of Kontakt 7.

## Getting Started

Continuing from where we left off, please ensure that the following starter code is in place in the scripting window:

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

Ensure that the script functions by clicking `Apply`. Upon clicking `Apply` followed by clicking `switch` you should see `it's on!` or `it's off!` appearing in the status bar / console at the bottom of the screen.

### Step One - Buttons and More Buttons

  * To begin, we will a series of buttons that interact with one another. First, let's change our `switch` button to be called `run`. Modify your code as follows:

    ```
    {this always runs when the script starts}
    on init

      {set the basics of the UI, setting the height at 213 and width at 720}
      make_perfview
      set_ui_height_px(213)
      set_ui_width_px(633)

      {declare a variable called $run that is tied to a button. Set that button to on }
      declare ui_switch $run
      $run := 1

    end on

    {this runs when the ui_control called $switch is changed}
    on ui_control($run)

      if($run = 1)
        message("it's on!")
      else
        message("it's off!")
      end if
    end on
    ```
    Notice that all references to `switch` have been changed to `run`, effectively renaming this button.

  * To ensure that your script is functioning, hit `Apply`. The code should function as it did prior.
  * Second, let's add three other buttons as well. Modify your code as follows:

    ```
    {this always runs when the script starts}
    on init

      {set the basics of the UI, setting the height at 213 and width at 720}
      make_perfview
      set_ui_height_px(213)
      set_ui_width_px(633)

      {declare a variable called $run that is tied to a button. Set that button to on }
      declare ui_switch $run
      $run := 1

      {declare another button called $option1 that is tied to a button. The initial value is set to on}
      declare ui_switch $option1
      $option1 := 1
      declare $option1Id
      $option1Id := get_ui_id($option1)
      set_control_par_str($option1Id, $CONTROL_PAR_TEXT, "A")

      {declare another button called $option2 that is tied to a button. The initial value is set to off}
      declare ui_switch $option2
      $option2 := 0
      declare $option2Id
      $option2Id := get_ui_id($option2)
      set_control_par_str($option2Id, $CONTROL_PAR_TEXT, "B")

      {declare another button called $option3 that is tied to a button. The initial value is set to off}
      declare ui_switch $option3
      $option3 := 0
      declare $option3Id
      $option3Id := get_ui_id($option3)
      set_control_par_str($option3Id, $CONTROL_PAR_TEXT, "C")

    end on

    {this runs when the ui_control called $switch is changed}
    on ui_control($run)

      if($run = 1)
        message("it's on!")
      else
        message("it's off!")
      end if
    end on
    ```
  * Let's dissect this code a bit. Notice that a new button is added with the following code:

    ```
    {declare another button called $option1 that is tied to a button. The initial value is set to on}
    declare ui_switch $option1
    $option1 := 1
    declare $option1Id
    $option1Id := get_ui_id($option1)
    set_control_par_str($option1Id, $CONTROL_PAR_TEXT, "A")
    ```
    Notice that the first line `declare ui_switch $option1` is the same code that created the `run` button, simply changed to address `option1`. Second, `$option1 := 1` sets the switch's initial value to `1` or `true` or `on`. Next, `declare $option1Id` declares a new variable that we will use to keep track of the unique ID that the compiler will use to keep track of this new switch. Every switch will have a unique ID assigned to it by the compiler. The line `\$option1Id := get_ui_id(\$option1)` asks the compiler to tell us what the unique ID is for `$option1`. This value is stored in the `$option1Id` variable so it can be used later. The final line `set_control_par_str(\$option1Id, \$CONTROL_PAR_TEXT, "A")` simply tells the compiler what text should be displayed inside this switch. This code is repeated for both `$option2` and `$option3`, changing the values accordingly.

  * Make sure your code functions by clicking the `Apply` button and testing out the buttons.

### Step Two - Toggle Time

  * It's possible to make the buttons react to one another. From this point forward in the tutorial, I will refer to each button by the text that appears within them. In this case, we want to make it so `A`, `B`, and `C` cannot be selected at the same time. For example, if `B` is selected, `A` and `C` should automatically turn themselves to the off position. This can be accomplished through the `on ui_control` callback. Modify your code as follows. Specifically, notice the new callbacks added to the end of the code:

    ```
    {this always runs when the script starts}
    on init

      {set the basics of the UI, setting the height at 213 and width at 720}
      make_perfview
      set_ui_height_px(213)
      set_ui_width_px(633)

      {declare a variable called $run that is tied to a button. Set that button to on }
      declare ui_switch $run
      $run := 1

      {declare another button called $option1 that is tied to a button. The initial value is set to on}
      declare ui_switch $option1
      $option1 := 1
      declare $option1Id
      $option1Id := get_ui_id($option1)
      set_control_par_str($option1Id, $CONTROL_PAR_TEXT, "A")

      {declare another button called $option2 that is tied to a button. The initial value is set to off}
      declare ui_switch $option2
      $option2 := 0
      declare $option2Id
      $option2Id := get_ui_id($option2)
      set_control_par_str($option2Id, $CONTROL_PAR_TEXT, "B")

      {declare another button called $option3 that is tied to a button. The initial value is set to off}
      declare ui_switch $option3
      $option3 := 0
      declare $option3Id
      $option3Id := get_ui_id($option3)
      set_control_par_str($option3Id, $CONTROL_PAR_TEXT, "C")

    end on

    {this runs when the ui_control called $switch is changed}
    on ui_control($run)

      if($run = 1)
        message("it's on!")
      else
        message("it's off!")
      end if
    end on

    {this runs when the ui_control called $option1 is changed}
    on ui_control ($option1)

      $option1 := 1

      {toggle the other buttons off}
      $option2 := 0
      $option3 := 0

    end on

    {this runs when the ui_control called $option2 is changed}
    on ui_control ($option2)

      $option2 := 1

      {toggle the other buttons off}
      $option1 := 0
      $option3 := 0

    end on

    {this runs when the ui_control called $option3 is changed}
    on ui_control ($option3)

      $option3 := 1

      {toggle the other buttons off}
      $option1 := 0
      $option2 := 0

    end on
    ```
    Notice how applying the above code will result in only one of the three `A`, `B`, `C` buttons being on at a time.

  * So we can better understand what's happening, let's turn our attention to each of the new callbacks for `on ui_control`:

      ```
      {this runs when the ui_control called $option1 is changed}
      on ui_control ($option1)

        $option1 := 1

        {toggle the other buttons off}
        $option2 := 0
        $option3 := 0

      end on
      ```
      When the above code is executed, `$option1` is set to the on position. Next, all the other buttons are set to `0` or the off position. The same logic is employed with the other buttons.

### Step Three - The State of Things

  * Now, all of our buttons are in place for our first Kontakt instrument. We are almost to the point where our script can start doing things musically interesting. To prepare for musical magic, we have some final edits to complete.
  * First, let's modify the initial value of `$run`. Up until this point, by default it has been set to the `on` position. Modify your code in `on init` as follows:

    ```
    $run := 0
    ```
    Notice how where it was prior set to `1`, we have changed it to `0`.

  * Second, let's create a new variable, a string, called `@state` in our `on init` callback. We will use this string to keep track of which of our `A` `B` `C` buttons is set to the on position:

    ```
    {set initial instrument state value}
    declare @state
    @state := "A"
    ```
  * Third, in each of our `on ui_control ($option)` callbacks, we will add a line that sets the state for that callback. For example, for `on ui_control ($option1)` we will modify the `@state` variable as follows:

    ```
    {this runs when the ui_control called $option1 is changed}
    on ui_control ($option1)

      $option1 := 1

      {toggle the other buttons off}
      $option2 := 0
      $option3 := 0

      @state := "A"

    end on
    ```

  * Fourth, let's make it so the `run` button is set to `off` every time the `A`, `B` or `C` option is changed. In each of our `on ui_control($option)` callbacks, we will add another line that turns off `run`. For example, for `on ui_control ($option1)` we will modify the code as follows:

    ```
    {this runs when the ui_control called $option1 is changed}
    on ui_control ($option1)

      $option1 := 1

      {toggle the other buttons off}
      $option2 := 0
      $option3 := 0

      {reset the run button}
      $run := 0
      message("Ready to run")

      @state := "A"

    end on
    ```

  * Finally, we will modify the `on ui_control($run)` callback as follows:

    ```
    {this runs when the ui_control called $switch is changed}
    on ui_control($run)

      if($run = 1)
        message("Running " & @state)
      else
        message("Ready to run")
      end if
    end on
    ```
    Notice that when the `run` button is pressed, it places a message in the console that echos the state that is running, whether `A`, `B`, or `C`. Otherwise, the console messages `Ready to run`.

  * A measure of consistency, let's add a `message("Ready to run")` in the `on init` function such that there is always something displaying in the console. Notice how the console provides us important feedback as the programmer, such that we can understand what's happening inside of our program. Your final code for this step should be the following:

    ```
    {this always runs when the script starts}
    on init

      {set the basics of the UI, setting the height at 213 and width at 720}
      make_perfview
      set_ui_height_px(213)
      set_ui_width_px(633)

      {declare a variable called $run that is tied to a button. Set that button to off }
      declare ui_switch $run
      $run := 0
      message("Ready to run")

      {declare another button called $option1 that is tied to a button. The initial value is set to on}
      declare ui_switch $option1
      $option1 := 1
      declare $option1Id
      $option1Id := get_ui_id($option1)
      set_control_par_str($option1Id, $CONTROL_PAR_TEXT, "A")

      {declare another button called $option2 that is tied to a button. The initial value is set to off}
      declare ui_switch $option2
      $option2 := 0
      declare $option2Id
      $option2Id := get_ui_id($option2)
      set_control_par_str($option2Id, $CONTROL_PAR_TEXT, "B")

      {declare another button called $option3 that is tied to a button. The initial value is set to off}
      declare ui_switch $option3
      $option3 := 0
      declare $option3Id
      $option3Id := get_ui_id($option3)
      set_control_par_str($option3Id, $CONTROL_PAR_TEXT, "C")

      {set initial instrument state value}
      declare @state
      @state := "A"

    end on

    {this runs when the ui_control called $switch is changed}
    on ui_control($run)

      if($run = 1)
        message("Running " & @state)
      else
        message("Ready to run")
      end if
    end on

    {this runs when the ui_control called $option1 is changed}
    on ui_control ($option1)

      $option1 := 1

      {toggle the other buttons off}
      $option2 := 0
      $option3 := 0

      {reset the run button}
      $run := 0
      message("Ready to run")

      @state := "A"

    end on

    {this runs when the ui_control called $option2 is changed}
    on ui_control ($option2)

      $option2 := 1

      {toggle the other buttons off}
      $option1 := 0
      $option3 := 0

      {reset the run button}
      $run := 0
      message("Ready to run")

      @state := "B"

    end on

    {this runs when the ui_control called $option3 is changed}
    on ui_control ($option3)

      $option3 := 1

      {toggle the other buttons off}
      $option1 := 0
      $option2 := 0

      {reset the run button}
      $run := 0
      message("Ready to run")

      @state := "C"

    end on
    ```
### Step Four - Your First Function

  * Functions are callbacks of your own design. Functions can be useful in executing blocks of code at will -- especially those that are repeated throughout your code. Notice how a specific block of code tends to repeat over and over again:

    ```
    $run := 0
    message("Ready to run")
    ```
    This code appears in no less than four places currently.

  * It's generally considered bad design to repeat the same code time and time again where a line of code could be employed in place of the repeats. Immediately after the `on init` callback, after `end on`, add the following:

    ```
    {creates a function that resets the values of run}
    function resetRun()

      $run := 0
      message("Ready to run")

    end function
    ```

  * Further, in the three places in the `on ui_control` for `$option1`, `$option2`, and `$option3`, replace the two lines that reset the `$run` variable and messages `Ready to run` to `call resetRun()`. Sadly, we cannot utilize such functions in `on init` -- which makes me sad as a programmer. In the end, your final code should appear as follows:

    ```
    {this always runs when the script starts}
    on init

      {set the basics of the UI, setting the height at 213 and width at 720}
      make_perfview
      set_ui_height_px(213)
      set_ui_width_px(633)

      {declare a variable called $run that is tied to a button. Set that button to off }
      declare ui_switch $run
      $run := 0
      message("Ready to run")

      {declare another button called $option1 that is tied to a button. The initial value is set to on}
      declare ui_switch $option1
      $option1 := 1
      declare $option1Id
      $option1Id := get_ui_id($option1)
      set_control_par_str($option1Id, $CONTROL_PAR_TEXT, "A")

      {declare another button called $option2 that is tied to a button. The initial value is set to off}
      declare ui_switch $option2
      $option2 := 0
      declare $option2Id
      $option2Id := get_ui_id($option2)
      set_control_par_str($option2Id, $CONTROL_PAR_TEXT, "B")

      {declare another button called $option3 that is tied to a button. The initial value is set to off}
      declare ui_switch $option3
      $option3 := 0
      declare $option3Id
      $option3Id := get_ui_id($option3)
      set_control_par_str($option3Id, $CONTROL_PAR_TEXT, "C")

      {set initial instrument state value}
      declare @state
      @state := "A"

    end on

    {creates a function that resets the values of run}
    function resetRun()

      $run := 0
      message("Ready to run")

    end function

    {this runs when the ui_control called $switch is changed}
    on ui_control($run)

      if($run = 1)
        message("Running " & @state)
      else
        message("Ready to run")
      end if
    end on

    {this runs when the ui_control called $option1 is changed}
    on ui_control ($option1)

      $option1 := 1

      {toggle the other buttons off}
      $option2 := 0
      $option3 := 0

      {reset the run button}
      call resetRun()

      @state := "A"

    end on

    {this runs when the ui_control called $option2 is changed}
    on ui_control ($option2)

      $option2 := 1

      {toggle the other buttons off}
      $option1 := 0
      $option3 := 0

      {reset the run button}
      call resetRun()

      @state := "B"

    end on

    {this runs when the ui_control called $option3 is changed}
    on ui_control ($option3)

      $option3 := 1

      {toggle the other buttons off}
      $option1 := 0
      $option2 := 0

      {reset the run button}
      call resetRun()

      @state := "C"

    end on
    ```
    Notice how the function called `resetRun` must appear above those callbacks that utilize this function. Through the utilization of this function, we are *abstracting away*, or simplifying, our code by taking a task that took six lines of code and reducing it down to a single line of code. Abstraction of this nature will become of particular importance as you develop as a Kontakt Script Language developer.

### Congratulations

You have now finalized the physical interface of your first Kontakt instrument. It could be tempting to get lured in by graphical interfaces, prototyping backgrounds and button layouts in specialized software. However, my recommendation is to keep things simple for the time being. Focus on function over aesthetics for a short while.

The tutorial to follow will allow you to *finally* produce something musically interesting.

*Native Instruments®️ and Kontakt®️ are registered trademarks of Native Instruments GmbH. We have no affiliation with Native Instruments.*
