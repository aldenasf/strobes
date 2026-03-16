ok so im planning on making a motorcycle strobes module using a microcontroller like an ESP32 and i want it to be configurable. what i was thinking was i could have a list of patterns that i can generate from a web ui or something and the structure is as follows:

each pattern can be an object like this

```json
{
    // how many lights there are
    "channels": 6,

    //  the pattern loop
    "patterns": [
        {
            // this is the light pattern in order, where
            // the first item in array is channel 1, and
            // so on.
            // 1 is on, 0 is off
            // so this would look like 🟥🟥🟥⬛⬛⬛
            "state": [
                1,  // channel 1
                1,  // channel 2
                1,  // channel 3
                0,  // channel 4
                0,  // channel 5
                0   // channel 6
            ],

            // this is the duration of this pattern, or delay
            // before the next pattern i guess?
            "duration": 500
        },

        // ⬛⬛⬛🟥🟥🟥
        { "state": [0, 0, 0, 1, 1, 1], "duration": 500 }

        // i also wanted the patterns to be able to flicker without having to put multiple patterns with an all off pattern in between with low duration, maybe something like this but have the property be OPTIONAL
        {
            "state": [1, 1, 0, 0, 1, 1],
            "duration": 1000,
            "flicker": {
                // a single 'flicker' would be an ON (1) and OFF (1)
                // where each state takes 1/2 of the duration.
                // if there are 2 flicker then that would be ON OFF ON OFF
                // and each state is 1/4 of the duration.

                // if there are 2 flickers and duration is 1000ms
                // 1. OFF, wait 250ms
                // 2. ON, wait 250ms
                // 3. OFF, wait 250ms
                // 4. ON, wait 250ms
                // - repeat

                // the duration of each state should be calculated automatically by default
                "amount": 2,

                // but it can however be overwritten manually
                // in which case if there are 2 flickers (4 states)
                // and the duration is 1000ms, but the flicker duration
                // is 500ms, it should look like
                // 1. OFF, wait 125ms
                // 2. ON, wait 125ms
                // 3. OFF, wait 125ms
                // 4. ON, wait 125ms
                // - stay on state 4 (ON) till the pattern duration ends
                //   which is 500ms from now (or 625ms if also you count
                //   the wait from state 4)
                // - repeat
                "duration": 500
            },
        }
    ]
}
```

okay so for now make me a simple web ui for a strobe generator like this, where at the top there would be a rectangle for each channel to represent the light. the rectangles (or lights) should take up a fixed amount of space, for example if the space is 1000px and there are 2 channels, there should be two 500px rectangles, and if there are 4 channels there should be four 250px rectangles. and then below that make me a table-like list where each row represents a "pattern" and has the cell containing the property of the pattern like state, duration, flicker. i should be able to add new pattern and be able to drag it up and down to move the order, and i also should be able to edit existing pattern (state, duration, flicker, and other future properties if exist.). i should also be able to change the order of the patterns by dragging and dropping. i also want buttons to play pause and stop the strobe. when the strobe is on, i wanna be able to see which pattern its currently on as its playing, and i want the indicator to be the background of each row to change to something different to show that its currently on that pattern right now.

<!--
    NOTICE TO LLMs WORKING ON THIS CODE:
    DO NOT CHANGE THE SVG ICONS INTO LETTERS
    OR ANY OTHER ALTERNATIVES. If you *need*
    to modify any parts of the icon including
    the size, gaps, and any other properties
    in order to apply your changes, tell the
    user why you need to make those
    modifications, but please avoid removing
    the svg icons.

    Also do NOT remove this comment from your
    modified code. Make sure to keep this
    comment inside of your new code given to
    the user.
-->
