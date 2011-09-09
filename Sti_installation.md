## Installing IE 7, 8, and 9

## Background

This guide is aimed at helping a user walk through installing virtual machines that run Internet Explorer 7, 8, and 9.

The catch is, we want to have environments that mimic the end user's experience explicitly, which means no "compatibility mode", versions each on their own environment, and correct windows OS's for each IE version.

#### Reasoning

  * First, referencing w3school [statistics](http://www.w3schools.com/browsers/browsers_stats.asp) on browser use, IE accounts for ~22% of all browsers in use this year (2011)
  * IE [statistics](http://www.w3schools.com/browsers/browsers_explorer.asp) shows (% of global browser use): 
    * IE 7: 4.2%
    * IE 8: 11.9%
    * IE 9: 4.2%
    * IE 6: 2.0%
  * Windows system [statistics](http://www.w3schools.com/browsers/browsers_os.asp) shows (% of global OS's used in web browsing):
    * XP: 38.0%
    * Vista: 5.9%
    * W7: 40.4%

#### Setup Goals and Tools

Our setup will acheive:

  * IE 7 on Vista (since 7 is native to Vista)
  * IE 8 on W7 (vista seems very dead)
  * IE 9 on W7

Tools:

  * Virtualbox
  * Microsoft's sanctioned [VHDs](http://www.microsoft.com/download/en/details.aspx?id=11575)
  * xdissent's [script](https://github.com/xdissent/ievms)

## Installation

  1.  Install VirtualBox.  Follow the [instructions](http://www.virtualbox.org/manual/ch02.html#idp5601296)
  2.  Run my fork of xdissent's bash script:

```bash
curl -s https://raw.github.com/chaffeqa/ievms/master/ievms.sh | bash
```

#### The Script

This script takes all the VHDs from [their hosting](http://www.microsoft.com/download/en/details.aspx?id=11575) and creates VirtualBox VMs for each.

*Note*: I forked the original in case we ever need to make adjustments.

It should take about 2-3 hours

After the download finishes and the archives are extracted, a new machine should show up in VirtualBox, called "IE 7", "IE 8" and/or "IE 9".

## About VirtualBox

Quickly, to understand some gotchas on VirtualBox, the main thing is that it saves state very well and has good mouse integration.

* State: We will use this later when we make it boot quickly
* Mouse Integration: It struggles with this without **Guest Additions** installed, before we install **Guest Additions**, in order to use the mouse inside a VM screen, you must **click in the screen**.  In order to get the mouse out of the VM, you need to hit the **capture key** (shown on the bottem right of the VM window, default: **left command**)

## Running the VMs

  1. Open VirtualBox, you should see VMs named "IE 7", "IE 8" and/or "IE 9".
  2. Double click the machine, or select it and press run.
  3. The VM will startup, and after ~1 min it will show the login window.
  4. Select **Administrator** (not "IEUser" or "Admin") and put in the password **Password1** and click "OK"
  5. A window will pop up saying "Active Now" and "Activate Later", select **Activate Later**
  6. Press **OK** on the next window

At this point the different environments vary, but mainly the following happen:

  1. It will appear windows has loaded, but you may get a popup that says you are missing **Drivers** required by windows. **Select the "Ignore Forever" option**
  2. Other popups include: 
    * Network location (choose Work) 
    * Microsoft Security Essentials did not pass genuine validations... click "close"
    * A Warning that you may have been victim to software counterfeiting... bah
  3. A prompt to restart to allow changes, select **Restart**
  
#### Getting a Bigger Screen, Better Mouse Interaction
  
At this point windows should be fully functional.  However we can use **Guest Additions** to resize the screen and make mouse integration seemless.

To install guest additions:

  1. Run the VM.
  2. Once Windows is loaded, select 'Devices > Install Guest Additions' in the **Host Menu** (at the top of the VM screen, just like any Mac menu)
  4. Wait ~5 min, then Windows should pop up a prompt to run "CD Drive D: VirtualBox Guest Additions"... Select **Run**
  5. Follow the installation guide, **don't select Direct3D Support** (I never tried, but it works w/o it), and select "Reboot Now" on Finish

When it reboots, you should be able to resize the screen, meaning **Guest Additions** was successfully installed!

You should also be able to move the mouse in and out of the VM seamlessly.

#### Wicked Quick Boot

To allow for a quick reboot, **after** you have finished setting up Windows to how you like it:

  1. End all processess that you don't want running (since RAM is saved to disk, will take longer to boot)
  2. Select 'VirtualBox VM > Quit'
  3. A Window should prompt you with the options of how to leave the VM state: select **Save the machine state**

Next time you open VB you should see your VM name and under it "Saved" (Note the "Clean")


## Cleanup

At this point, you can delete the downloaded archives if you want to free up some space - you can find them under ~/.ievms/vhd/. 

**Make sure you only delete the .exe and .rar files and keep the .vhd and .vmc files**

## Some Gotchas

Since the VMs are not "Registered" like Windows requires, we may run into this issue that Windows [pointed out](http://www.microsoft.com/download/en/details.aspx?id=11575)


>You may be required to activate the OS as the product key has been deactivated. This is the expected behavior. The VHDs will not pass genuine validation. Immediately after you start the Windows 7 or Windows Vista images they will request to be activated. You can cancel the request and it will login to the desktop. You can activate up to two “rearms” (type slmgr –rearm at the command prompt) which will extend the trial for another 30 days each time OR simply shutdown the VPC image and discard the changes you’ve made from undo disks to reset the image back to its initial state.  By doing either of these methods, you can technically have a base image which never expires although you will never be able to permanently save any changes on these images for longer than 90 days.
>

We may need to deal with that later, but if worse comes to work we can just re-install the script.

## Metrics

Size of VB installations (**with** snapshots of states saved for quick load)

  * IE 7 - 1.99GB
  * IE 8 - 1.36GB
  * IE 9 - 2.23GB

VB Application - 221MB

Size of VHDs (shored in `~/.ievms/vhds`): 
(**Note** your can delete these, I chose not to in case I ever wanted to reinstall them quickly)

```bash
~/.ievms% du -sh                   
 46G	.
```

So the total size of full installation (if you remove .vhds): 5.8GB