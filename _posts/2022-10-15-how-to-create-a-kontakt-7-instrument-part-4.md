---
layout: post
title:  "How to Create a Kontakt 7 Instrument (Part 4) - Midi Scale"
date:   2022-10-15 07:00:00 +0800
categories: KONTAKT
tags: kontakt7
---

Welcome to the fourth installment of my multi-part series about how to engage in the beautiful pursuit of building your own instruments, effects, and plugins in the Kontakt Script Processor (KSP).

## Prerequisites

This tutorial continues from the [third installment](https://gelvinwhite.com/kontakt/2022/10/02/how-to-create-a-kontakt-7-instrument-part-3/) of this multi-part series. So, if you have not followed the instructions in our previous installments, best to backtrack and complete those steps. Further, this tutorial requires a full version of Kontakt. Likely, Kontakt 5 or later is fine. However, screenshots in this tutorial are of Kontakt 7.

## Getting Started

Continuing from where we left off, please ensure that the following starter code is in place in the scripting window:

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

Ensure that the script functions by clicking `Apply`. Upon clicking `Apply` followed by clicking `C`, and `run` you should see `Running C` appearing in the status bar / console at the bottom of the screen.

### Step One - Building Scales

  * Generally speaking, *arrays* are contiguous, back-to-back, areas of memory where a series of numbers, letters, or other data types are stored.
  * KSP offers array types for integers, real numbers, and strings.
  * For example, consider the following *integer array*:

    ```
    {this is an integer array}
    declare %myArray[4] := (0,1,2,3)
    ```
    Notice that the `declare` statement creates an array of type integer, signified by the `%` symbol. The number of elements allowed in this array are `4`. Then, the values for each of those four locations are assigned. So, the *zeroth* spot contains the value `0`. `myArray` of `1` (the second location of the array) contains an integer `1`. `myArray` of `2` (the third location of the array) contains an integer `2`. The last location of `myArray` is `myArray` of `3`, which contains a `3`.

  * Arrays are *zero-indexed*, in that we start counting the locations in the array starting at zero. This comports with most programming languages that count starting at zero. For example, consider this second array:

    ```
    {this is another integer array}
    declare %myOtherArray[3] := (52,65,44)
    message(%myOtherArray[0])
    ```
    Notice that `myOtherArray` can hold three integers. The first location, `%myOtherArray[0]` holds the integer value `52`. This code will `message` that a value to the console. What would happen if you modified your `message` statement to `message(%myOtherArray[2])`? If you guessed that `44` would be displayed in the console, you are correct!

  * Arrays are very useful for holding collections of values. For example, let's add the following to the bottom of your `on init` function in your starter code:

    ```
    {c major scale in midi notes}
    declare %cMajorScale[8] := (60,62,64,65,67,69,71,72)
    ```
    Notice that this array of integers has eight locations where one could store values. Then, eight values are assigned to these positions. The zeroth value is `60`. The last value in the array is `72`.

  * Midi notes are assigned specific integer values by industry standard. `60` is designated as a middle C (C3). `62` is designated as the D above middle C (D3). Accordingly, the array you just added to your code essentially holds the values of the C major scale. You can obtain a midi note chart simply by Googling "midi note chart".
  * Under the code you just added, also add the line `declare $count := 0` as follows:

    ```
    {c major scale in midi notes}
    declare %cMajorScale[8] := (60,62,64,65,67,69,71,72)
    declare $count := 0
    ```
    We will be using this additional `count` variable in the step to come.

  * After adding the above code to the end of your `on init` function, hit `Apply`. Provided that no errors appear and your program functions as it did before, you have added it correctly.

### Step Two - Looping with While

  * `while` loops will repeat a block of code as long as a condition is true. For example, consider the following code:

    ```
    {this loop will repeat while count is less than 8}
    while ($count < 7)
      {message the count to the console}
      message($count)

      {wait 1000000 microseconds or 1 second}
      wait(1000000)

      {increment the count by 1}
      inc ($count)
    end while
    ```
    Notice that this will loop as long as `count` is less than `7`. First, the loop messages the current count. Then, we `wait` for `1000000` microseconds. Then, the `count` is increased by `1`. The loop will then repeat over and over again until `count` is no longer less than `8`.

  * We can effectively create a function within our script that loops through each note of the `cMajorScale` array. Add the following code immediately below the end `on init` callback.

    ```
    function play()

      while ($count < 7)
        message("Note # " & %cMajorScale[$count])
        play_note(%cMajorScale[$count],127,0,2000000)
        wait(2000000)
        inc ($count)
      end while

      $count := 0

    end function
    ```
    Notice how this function, called `play`, will first enter a `while` loop. The `loop` will check to see if `count` is less than `8`. If so, it will message the console the current note being played in midi notation. Since, `%cMajorScale[0]` is `60` this will cause `60` to appear in the console. Then, the next line of code engages the midi engine to play a note. In this initial case, it plays the note `60` or C3. It plays it at velocity `127`, with `0` sample offset (more on that later), for length 2000000 or 2 seconds. Then, the script engine waits for `2000000` microseconds or `2` seconds. Finally, the count is increased. Next time through the loop, `%cMajorScale[1]` or `62` is played. This is a D3 note. This repeats until the scale is done. In the end, `count` is reset to `0`.

  * Finally, we need to tell the script processor to run this `play` function when the `run` button is pressed. Accordingly, in the `on ui_control($run)` callback, modify its contents to be the following:

    ```
    {this runs when the ui_control called $run is changed}
    on ui_control($run)

      call play()

      {always set the run button back to its original state}
      $run := 0
    end on
    ```
    Notice that this function simply executes the `play` callback. Then, it resets itself to the `off` position.

  * Your program is ready to run some midi to your DAW!

### Step Three - Overcoming the Curses of Midi Routing

  * Ironically, despite the complications that are inherent in coding, simply getting one's DAW to recognize the MIDI data emerging from Kontakt can be the most challenging aspect of this slice of our larger project.
  * Kontakt's side of things is exceptionally easy. You'll want to head to the gear `Options` icon at the top of Kontakt and navigate to `Engine`. There, you will want to modify `Send MIDI to outside world` to include `Script generated notes` and `script generated CCs`. Your options should appear as follows:

    ![kontakt panel with script generated notes selected](/assets/2022-10-15-how-to-create-a-kontakt-7-instrument-part-4/001.png "kontakt panel")

  * Additionally, I find it helpful in some situations to ensure that the MIDI channel of your Kontakt instrument is set to `Omni` as follows:

    ![kontakt panel with omni midi selected](/assets/2022-10-15-how-to-create-a-kontakt-7-instrument-part-4/002.png "kontakt midi settings")

  * Within your DAW of choice, getting the DAW to recognize the midi data and route it properly involves specific settings.
  * Within Ableton Live, this is relatively easy. First, load Kontakt with your script onto a MIDI track. Second, load a MIDI instrument of your choosing into a second track. Finally, modify the track routing as follows:

    ![two ableton live tracks set to record monitoring midi](/assets/2022-10-15-how-to-create-a-kontakt-7-instrument-part-4/003.png "ableton live tracks")
    Notice that the left hand track, which is currently assigned Kontakt with our script loaded, is set to monitor `All Ins`. `Monitor` is set to `In`. The right hand track, assigned to a midi instrument of our choosing (I try to use a simple piano for development), is set to get `MIDI From` our first track. Both the top and bottom dropdowns are set to monitor the first track. Finally, and most importantly in Ableton Live, both tracks are `record enabled`.

  * If you are a Logic user, you will need to run Kontakt in the stand-alone application. With the stand-alone application running, with your script copied in, you will go to `Options > MIDI > Outputs` and ensure that 'Kontakt X Virtual Instrument'. Once you have a virtual instrument loaded within Logic, it will *likely* play the resulting MIDI.

    ![kontakt panel with midi options selected](/assets/2022-10-15-how-to-create-a-kontakt-7-instrument-part-4/004.png "kontakt midi settings")

### Congratulations

You have now worked with MIDI for the first time in your project. In our next iteration of this series, we will be adding further possibilities for midi generation.

*Native Instruments®️ and Kontakt®️ are registered trademarks of Native Instruments GmbH. We have no affiliation with Native Instruments.*
