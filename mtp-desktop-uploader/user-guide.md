# User Guide

_A note on Connections / Blue Lines_

It’s important to note, Google does some server side processing of Street View images too.

[Google recommends when taking 360 Street View images](https://support.google.com/maps/answer/7012050?hl=en&ref_topic=627560).

> Space the photos about two small steps apart \(1 m / 3 ft\) when indoors and five steps apart \(3 m / 10 ft\) when outdoors.

This video from the Street View Conference in 2017 also [references the 3m interval](https://www.youtube.com/watch?v=EW8YKwuFGkc).

[According to this Stack Overflow answer](https://stackoverflow.com/questions/54237231/how-to-create-a-path-on-street-view):

> You need to have &gt; 50 panoramas with a distance &lt; 5m between two connected panoramas. After some days \(weeks?\) Google will convert them to a blue line in a separate processing step.

It’s safe to assume in some cases Google servers might automatically connect your images into a blue line even if a connection target is not defined, [as addressed here](https://support.google.com/contributionpolicy/answer/7411351):

> When multiple 360 photos are published to one area, connections between them may be automatically generated. Whether your connections were created manually or automatically, we may adjust, remove, or create new connections — and adjust the position and orientation of your 360 photos — to ensure a realistic, connected viewing experience

In summary, you're photos can be connected to other photos on the Street View platforms, in addition to the connections defined in the script.

