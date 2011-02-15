OSCeleton
=========

What is this?
-------------

As the title says, it's just a small program that takes kinect
skeleton data from the OpenNI framework and spits out the coordinates
of the skeleton's joints via OSC messages. These can can then be used
on your language / framework of choice.


How do I use it?
----------------

### First you need to install the OpenNI driver, framework, and middleware
#### Windows / Linux / Mac OSX
Get avin's hacked Primesense PSDK driver for kinect:
[https://github.com/avin2/SensorKinect](https://github.com/avin2/SensorKinect)
Folow his instructions for installing the OpenNI framwork, the driver,
and the NITE middleware.

#### After OpenNI / NITE is working
Then you can run one of the precompiled binaries in the "bin"
directory or compile your own:

on Linux or Mac OSX:
    make

> NOTE FOR MAC USERS: You must run OSCeleton from the terminal or it
> will not run correctly.

on windows: you can use the precompiled binary in bin\win32 or use the
VC++ express .sln file.

If you run the executable without any arguments, it will send the OSC
messagens in the default format to localhost on port 7110.
To learn about the OSC message format, continue reading below or check
out our processing examples at
[https://github.com/Sensebloom/OSCeleton-examples](https://github.com/Sensebloom/OSCeleton-examples)

#### Other stuff
Another fun way to test OSCeleton is to use the awesome animata
skeletal animation software by the Kitchen Budapest guys. You can get
it at:
[http://animata.kibu.hu/](http://animata.kibu.hu/)

Animata needs its OSC messages in a very specific format, so you must
use the "-k" ("kitchen" mode) option. Multiplying the x and y
coordinates and adding some offsets can also be useful to tune the
size of the skeleton, try running it like this:
    OSCeleton.exe -k -mx 640 -my 480 -ox -160

If your animation is going crazy try to play with -mx and -my values,
and -ox and -oy values a bit.

To get a complete list of available options run OSCeleton -h.


OSC Message format
------------------

### New user detected - no skeleton available yet. This is a good time
### for you to ask the user to do the calibration pose:
    Address pattern: "/new_user"
    Type tag: "i"
    i: A numeric ID attributed to the new user.

### New skeleton detected - The calibration was finished successfully,
### joint coordinate messages for this user will be incoming soon ;):
    Address pattern: "/new_skel"
    Type tag: "i"
    i: ID of the user whose skeleton is detected.

### Lost user - we have lost the user with the following id:
    Address pattern: "/lost_user"
    Type tag: "i"
    i: The ID of the lost user. (This ID will be free for reuse from now on)

### Joint message - message with the coordinates of each skeleton joint:
    Address pattern: "/joint"
    Type tag: "sifff"
    s: Joint name, check out the full list of joints below.
    i: The ID of the user.
    f: X coordinate of joint in interval [0.0, 1.0]
    f: Y coordinate of joint in interval [0.0, 1.0]
    f: Z coordinate of joint in interval [0.0, 7.0]

#### NOTE: Kitchen mode
To send OSC messages compatible with the awesome animata skeletal
animation software use the "-k" option. The messages will have the
following format:
    Address pattern: "/joint"
    Type tag: "sff"
    s: joint name concatenated with user id (ex: "l_shoulder0")
    f: X coordinate of joint in interval [0.0, 1.0]
    f: Y coordinate of joint in interval [0.0, 1.0]
In this mode new_user, new_skel and lost_user messages
will not be sent.

#### NOTE: Quartz Composer mode
You can enable a message format that is more friendly to Quartz
composer with the "-q" option. The messages will have the following format:
    Address pattern: "/joint/name/id"
    Type tag: "fff"
    f: X coordinate of joint in interval [0.0, 1.0]
    f: Y coordinate of joint in interval [0.0, 1.0]
    f: Z coordinate of joint in interval [0.0, 7.0]
Example (left knee of user 3):
    /joint/l_knee/3 0.08823146 0.5761504 0.44253197

#### NOTE: Scratch mode
To send scratch_broadcast messages compatible with the Scratch software developed by the Lifelong Kindergarten Group at the MIT Media Lab, use the "-c" option. The messages is not OSC and have the
following format:
    broadcast message_name
    sensor-update joint_x X coord of joint joint_y Y coord of joint ...
    s: joint
    f: X coordinate of joint in interval [-320.0, 320.0]
    f: Y coordinate of joint in interval [-240.0, 240.0]
Example (left knee of user 3):
    sensor-update "l_knee_x" -32.837425 "l_knee_y" -42.730053 

### Full list of joints

* head
* neck
* torso

* r_collar #not working yet
* r_shoulder
* r_elbow
* r_wrist #not working yet
* r_hand
* r_finger #not working yet

* l_collar #not working yet
* l_shoulder
* l_elbow
* l_wrist #not working yet
* l_hand
* l_finger #not working yet

* r_hip
* r_knee
* r_ankle
* r_foot

* l_hip
* l_knee
* l_ankle
* l_foot


Other
-----

### For feature request, reporting bugs, or general osceleton discussion, come join the fun in our [google group](http://groups.google.com/group/osceleton)!

Have fun!
