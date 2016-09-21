# GLFW-Controller-Database
A database containing mappings of every known controller working with GLFW 3.3

## Using the database

So according to GLFW, the joystick name is not guaranteed to be unique per device, and it may report differently on different OS, even the name. This leaves us with only the number of axes and buttons to identify. Additionally, we can keep keywords per mapping, and select the mapping which matches the maximum keywords. This should be the way to globally identify the controllers.

The goal of GLFW Controller Database is not to map every controller to an ideal controller, but to document the input locations on a wide range of controllers. We want to generalize the controllers between XBOX layout and PS layout controllers.

And in XBOX controllers posted, and also third party XBOX style controllers, I've identified two layouts, one with 14 buttons and 6 axes and another with 11 buttons and 8 axes. There might be the same happening in PS style controllers too, we just need to test more.

The new layout in the database (I'll push to the repository soon) is to have files with the naming scheme Style-Buttons-Axes-Platform-Type.json. For examples of valid names, Xbox-14-6-Linux-1.json and Xbox-11-8-Linux-2.json are good examples. The type is a number, which only changes when a controller is of an existing style, have the same number of axes and buttons, but the indices are different.

The internal file format is also going to change a small bit, it accepts names as an array of strings which are known variations that give the same indices for the names, and they are also going to include a new keywords section, which is an array of strings as well.

~~~json
{
    "names": [
        "name1", "name2", ...
    ],

    "keywords": [
        "keywords", "to", "identify", "uniquely"
    ],

    "buttons": [
        { "index": 0, "name": "NAME" }, ...
    ]

    "axes": [
        { "index": 0, "name": "NAME" }, ...
    ]
}
~~~

The layout section is now gone, because that information is present in the file name, and is already provided by the GLFW, there is no need for redundant data. Also the indices are now JSON number fields and not strings anymore.

We also make it strict for names within the mappings for axes and buttons, so that they are consistent across the mappings. The names for the XBOX style controllers are as follows.

**X, Y, A, B, BACK, START, GUIDE, LB, RB, DPAD_UP, DPAD_RIGHT, DPAD_LEFT, DPAD_DOWN, LS, RS** for the buttons and
**LS_X, LS_Y, RS_X, RS_Y, LTRIGGER, RTRIGGER, DPAD_X, DPAD_Y** for the axes.

Similarly the names for PS style controllers are as follows.

**TRIANGLE, CIRCLE, CROSS, SQUARE, SELECT, START, PS, L1, R1, L2, R2, DPAD_UP, DPAD_RIGHT, DPAD_LEFT, DPAD_DOWN, LS, RS** for the buttons and
**LS_X, LS_Y, RS_X, RS_Y, DPAD_X, DPAD_Y** for the axes.

Every mapping should have the names only from the above selected list. Any other names will render it invalid. Coming to the styles PS and XBOX, there is a need for the differences between them, apart from physical layouts (which only change the location of the left stick), triggers are buttons in PS controllers where they are axes in XBOX style.

With this, I think I have covered abnormalities in controllers enough. There are more layouts and different controllers in the wild, but let's start with these.
