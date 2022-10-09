---
layout: post
title:  "How to Create a Kontakt 7 Instrument (Part 1) - Hello World!"
date:   2022-10-1 07:00:00 +0800
categories: KONTAKT
tags: kontakt7
---

Welcome to the beautiful pursuit of building your own instruments, effects, and plugins in the Kontakt Script Processor (KSP).

## Prerequisites

This tutorial requires a full version of Kontakt. Likely, Kontakt 5 or later is fine. However, screenshots in this tutorial are of Kontakt 7.

## Getting Started

The Kontakt Script Processor language is a well-documented programming language with a thriving community behind it. As you get started, I recommend getting your hands on the following resources:

* Obtain the [Kontakt Script Language](https://www.native-instruments.com/forum/attachments/kontakt-script-language-pdf.86897/) development document. Ignore the age of this document or any references to Kontakt version numbers. It is masterfully written document that is exceedingly applicable today. This document provides a narrative introduction to the scripting language.
* Obtain the [KSP Reference Manual](https://www.native-instruments.com/fileadmin/ni_media/downloads/manuals/kontakt/KONTAKT_602_KSP_Reference_Manual.pdf). As of time of this writing, the KSP Reference Manual discusses Kontakt 6. This is not a problem!
* Join the [Native Instruments Community](https://www.native-instruments.com/en/community/).
* Join the [NI Kontakt Discord](https://discord.com/invite/rTpCcpg)

### Step One - Find the Scripting Environment.

  * Open your full version of Kontakt. Likely, you will see a screen like this:

    ![kontakt splash screen with many libraries](/assets/2022-10-01-how-to-create-a-kontakt-7-instrument/001.png "kontakt splash screen")

  * Next, click the file icon and then select `New instrument`.

    ![kontakt splash screen with menu open creating new instrument](/assets/2022-10-01-how-to-create-a-kontakt-7-instrument/002.png "kontakt new instrument")

  * Next, click the wrench `edit mode` icon.

    ![kontakt interface with the wrench](/assets/2022-10-01-how-to-create-a-kontakt-7-instrument/003.png "kontakt edit mode")

  * Next, click the `Script Editor` button.

    ![kontakt opening script editor](/assets/2022-10-01-how-to-create-a-kontakt-7-instrument/004.png "kontakt opening script editor")

  * Finally, click the `Edit` button.

    ![kontakt edit button](/assets/2022-10-01-how-to-create-a-kontakt-7-instrument/005.png "kontakt edit button")

  * You are now in the Script Editor. Congrats!

    ![kontakt script editor](/assets/2022-10-01-how-to-create-a-kontakt-7-instrument/006.png "kontakt script editor")

### Step Two - Hello World!

  * Copy and paste the following code into the Script Editor window.

  ```
  on init

  	message("Hello World!")

  end on
  ```

  * Now, click the `Apply` button.

  * Upon clicking `Apply`, you should see `Hello World!` appearing in the status bar / console at the bottom of the screen.

    ![kontakt hello world](/assets/2022-10-01-how-to-create-a-kontakt-7-instrument/007.png "kontakt hello world")

### Step Three - Analyzing the Script

The block of code from `on init` to `end on` is always run first by the script compiler. Anything that you put in this block of code will run!

The `message("Hello World!")` statement tells the compiler to output `Hello World!` into the console. You can change this message within the quotes to say anything you like. In future projects, you will find it exceptionally useful to be able to output to the console -- such that you can provide yourself clues about what is happening in your code. This `message` function will be an invaluable tool you will use in the future to catch and understand bugs in your script.

### Congratulations

You have now created your first Kontakt script!

The tutorial to follow will discuss adding buttons to your interface.

*Native Instruments®️ and Kontakt®️ are registered trademarks of Native Instruments GmbH. We have no affiliation with Native Instruments.*
