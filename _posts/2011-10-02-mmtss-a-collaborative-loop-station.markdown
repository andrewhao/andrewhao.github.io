---
author: andrewhao
comments: true
date: 2011-10-02 10:53:45
layout: post
slug: mmtss-a-collaborative-loop-station
title: mmtss, a collaborative loop station
wordpress_id: 1113
categories:
- Art
- Design
- Geek
- Javascript
- programming
- Software
---

## mmtss is a loop station built for live performances.




Let's make music together! This project simplifies a traditional loop tracking station and is designed for interactive collaborative music performances.




The idea: Everybody adds or modifies one "part" of a 32-bar loop. The user gets to play an instrument over the existing mix and record the 32-bar phrase when she or he is ready. Once the person is finished, the project selects another instrument at random for the next viewer to record.







It's an Ableton Live controller serving a Webkit view, backed by node.js on the backend and socket.io + RaphaelJS on the front. Communication is done through a LiveOSC Live plugin via sockets.




Displayed at the Regeneration "We Collaborate" art show in Oakland, CA. 9/24/2011.




### Screenshots




![Practice mode](https://a248.e.akamai.net/assets.github.com/img/64abaefb0d10744dda42f362b6cd991522a88da4/687474703a2f2f6661726d372e7374617469632e666c69636b722e636f6d2f363136392f363138383336363537375f376261343864333864315f7a2e6a7067)




mmtss in practice/playback mode. Here the user is able to practice/mess around with the current instrument to prepare to record the next track.




![Cued mode](https://a248.e.akamai.net/assets.github.com/img/3f0c633b9b8d9d35970efe73f84c0f67bf68c6a8/687474703a2f2f6661726d372e7374617469632e666c69636b722e636f6d2f363132312f363138383838363131345f396436643531393937325f7a2e6a7067)




Pressing "record" puts the user in a wait state. They are prompted to begin recording when all the black boxes count down and disappear.




![Record mode](https://a248.e.akamai.net/assets.github.com/img/650dc62a52d8ca6441eff57e697307325390caaa/687474703a2f2f6661726d372e7374617469632e666c69636b722e636f6d2f363137372f363138383336373135315f636135623738323733355f7a2e6a7067)




mmtss in record mode.




More screenshots: http://www.flickr.com/photos/andrewhao/sets/72157627640840853/




### Source code




Github: http://www.github.com/andrewhao/mmtss.




MIT/GPL-sourced for your coding pleasure.




### Installation






  * Make sure you have npm installed: [http://www.npmjs.org](http://www.npmjs.org/)


  * Copy `lib/LiveOSC` into `/Applications/Live x.y.z OS X/Live.app/Contents/App-Resources/MIDI\ Remote\ Scripts/` folder


  * Set it as your MIDI remote in the Ableton Live Preferences pane, in the "MIDI Remote" tab.




### Running it






  * Open `Mmtss_0.als` as a sample Live project.


  * Install all project dependencies with `npm install` from the project root.


  * Start the Node server with `node app.js` from the root directory.


  * Open a Web browser and visit `localhost:3000`




### Modifying the sample project




You can modify this project to suit your own needs. Note that there are two sets of tracks; instrument (MIDI input) tracks and loop tracks that actually store clips.




For `n` tracks, you can add or remove your own instruments. Just make sure that instrument at track `x` corresponds to track `x` + `n`.




### Credits






  * Design and architectural inspiration taken from [vtouch](http://github.com/vnoise/vtouch), a HTML5/Node/Canvas Ableton controller.


  * Original LiveOSC source is found at: [http://monome.q3f.org/browser/trunk/LiveOSC](http://monome.q3f.org/browser/trunk/LiveOSC). We use a different fork of the project at:[http://github.com/vnoise/vtouch](http://github.com/vnoise/vtouch).


  * Super sweet CSS3 rocker widgets courtesy of [Simurai](http://www.simurai.com/): [UmbrUI](http://lab.simurai.com/css/umbrui/)




### License




[MIT](http://www.opensource.org/licenses/mit-license.php) and [GPLv3](http://www.gnu.org/copyleft/gpl.html) licensed. Go for it.




You will, however, need to get a license for [Ableton Live](http://www.ableton.com/live) yourself.




### The handsome collaborators






  * Andrew Hao: [http://www.g9labs.com](http://www.g9labs.com/)


  * David Luoh: [http://www.inkproj.com](http://www.inkproj.com/)




### 



