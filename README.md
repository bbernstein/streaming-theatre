# streaming-theatre

Sample streaming theatre using [Isadora][isadora], [Qlab][qlab], and [ZoomOSC][zoomosc]

[isadora]: https://troikatronix.com/
[qlab]: https://qlab.app/
[zoomosc]: https://www.liminalet.com/zoomosc

## What's here
This repository contains an example project. It is mostly complete and it sbased on a recent actual theatre production
that was produced on January 22, 2022 by [Newton Theatre Company][ntc].

[ntc]: https://www.newtontheatrecompany.com/

To take full advantage of this repo, you'll need to have all three pre-requisite software installed on
some combination of computers. I built and ran this show using two Macs:

1. MacBook Pro (16-inch 2019), 32 GB ram, 8-core i9, MacOS Monterey (12.1)
    1. Running [Isadora][isadora] and [ZoomOSC Pro][zoomosc]
2. MacBook Pro (13-inch, M1, 2020), 16 GB ram, M1, MacOS Monterey (12.1)
    2. Running [Qlab][qlab] and [ZoomOSC Essentials][zoomosc]

I did have a third machine running the sound meter. The sound meter machine ran [ZoomOSC Essentials][zoomosc], 
[Audio Hijack][hijack], and [Waves WLM][wlm].

You will also need to install something to allow easy audio routing. I used [BlackHole][blackhole] on all my
machines so that any sound can go to any app, a really useful tool.

This example project assumes everything is running on the same machine since host addresses are all set to 
`localhost` or `127.0.0.1`.

[hijack]: https://rogueamoeba.com/audiohijack/
[wlm]: https://www.waves.com/plugins/wlm-loudness-meter#how-to-set-loudness-levels-for-streaming-wlm
[blackhole]: https://github.com/ExistentialAudio/BlackHole

## What to install

You'll need to have the following software installed:

* [Isadora][isadora]
* [Qlab][qlab]
* [ZoomOSC Pro][zoomosc]
* [BlackHole][blackhole]
* [OBS][obs]

if you want a sound meter, there are several options. I've used [YouLean][youlean] in the past but 
more recently I've started using the combo [Audio Hijack][hijack] with [Waves WLM][wlm]. Either are fine
and allow you to get all your actors at the same audio level. Audio is another whole story, and a real
challenge when working with actors with huge dynamic ranges.

[youlean]: https://youlean.co/youlean-loudness-meter/
[obs]: https://obsproject.com/

In addition to the above apps, you'll need some add-on's to [Isadora][isadora] and [OBS][obs] to handle 
all the features used in this project and to stream it out to a streaming provider.

For [Isadora][isadora] you'll need these plugins:

* [Syphon Virtual Webcam][izzy-syphon] - follow instructions to install the needed OBS plugin *Syphon Virtual Camera*.
* [Screen Capture][izzy-screen]
* [JSON Parser][izzy-json]

[izzy-syphon]: https://troikatronix.com/add-ons/syphon-virtual-webcam/
[izzy-screen]: https://troikatronix.com/add-ons/screen-capture/
[izzy-json]: https://troikatronix.com/add-ons/json-parser-json-bundler/


## Getting started

Start by cloning this directory by clicking the green **code** button at the top of this page.

Now open the directory and launch the **theatre-project.izz** file.

To control this file from Qlab, you can also launch **theatre.qlab4** with the complementary setup.

From here you can walk through the scenes in **theatre-project.izz** and read the comments.

You should then select menu: **Output > Force Stage Preview** to see what's going on.

Here's what's happening in the scenes:

* BLANK 
  * This is just an empty placeholder so nothing happens when you start up Izzy.

* RESET
  * This resets a bunch of values to empty/zero.
    * Reset Everyone
      * With the names of all the actors, this clears out any values they may have had previously so they don't 
show up in the wrong places if they aren't online yet.
    * The Global Value **SetupMode** is set to zero here. That means it will be expecting to get all
user images from a screen-scrape of ZoomOSC. The alternative is when **SetupMode** is set to `1` and
images come from a static file so it's easy for you to set up your scenes offline.
    * A bunch of `/zoom/` calls are made to get ZoomOSC into the needed state.

* OSCListen
  * This is the workhorse scene and will need to stay active for the rest of the time you're using your project.
  * There are comments explaining what each of the seconds do, but clicking into each `user actor` will show much 
more detail.

* ActivateListen
  * This simply causes the previous scene to stay active for the rest of the time.

* OFFLINE
  * This scene sets up **SetupMode**. You should not go to this scene if you are in an actual rehearsal or are
using ZoomOSC. This scene sets you up so that it will use a static screenshot of a zoom gallery to create
your scenes. In this scene, you will hardcode the values in **Setup Offline Actor** actors to match the
gallery offsets of the actors in the photo.
  * The photo is displayed here for convenience, but the actual displayed photo is set up in scene:
**OSCListen > Video Input > Picture Player**

* Test
  * This is the first actual scene. This is how every other scene will look (more or less) when using this 
framework.
  * The main pieces here are:
    * Name To Video
      * Given a list of actor zoom names, it will output their video
    * Layout
      * Given a video output, it will display it in some location and zoom level
      * To control the layout, you need to click a number (matching the layout number: "Video 1" would be number "1").
        * After you type 1 (for the video for "Mary" in the example case), you can then:
          * hold the left mouse button and drag that user on the stage
          * press the right button to change the size based on vertical location in the stage
        * To get to higher numbers, video 10 is "0", video 11 is (shift 1) "!", video 12 is "@" (shift type the number)
    * Listen Scene Change
      * This **user actor** triggers the scene change in one of these ways:
        * receiving the trigger from the input
        * from an OSC message `/qlab/next` (this is how qlab controls scenes)
    * Background (just a little something)

* The rest of the scenes
  * they are all pretty much the same from the tech side. They all have actors, layouts, scene-changes, and background


I'll try to add more info as I get a chance.

Contact me if you have any questions.
