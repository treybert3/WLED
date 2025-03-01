<p align="center">
  <img src="/images/wled_logo_akemi.png">
  <a href="https://github.com/wled-dev/WLED/releases"><img src="https://img.shields.io/github/release/wled-dev/WLED.svg?style=flat-square"></a>
  <a href="https://raw.githubusercontent.com/wled-dev/WLED/main/LICENSE"><img src="https://img.shields.io/github/license/wled-dev/wled?color=blue&style=flat-square"></a>
  <a href="https://wled.discourse.group"><img src="https://img.shields.io/discourse/topics?colorB=blue&label=forum&server=https%3A%2F%2Fwled.discourse.group%2F&style=flat-square"></a>
  <a href="https://discord.gg/QAh7wJHrRM"><img src="https://img.shields.io/discord/473448917040758787.svg?colorB=blue&label=discord&style=flat-square"></a>
  <a href="https://kno.wled.ge"><img src="https://img.shields.io/badge/quick_start-wiki-blue.svg?style=flat-square"></a>
  <a href="https://github.com/Aircoookie/WLED-App"><img src="https://img.shields.io/badge/app-wled-blue.svg?style=flat-square"></a>
  <a href="https://gitpod.io/#https://github.com/wled-dev/WLED"><img src="https://img.shields.io/badge/Gitpod-ready--to--code-blue?style=flat-square&logo=gitpod"></a>

  </p>

This is a modification to the color_wipe function and the Wipe Random effect to add a delay slider.

The delay adds X-seconds to the end of the color wipe. I'm a coding noob, so use at your own risk.

What took me a while to understand is that most effects are basically a function that runs around 20 times a second with a timestamp as its only input. The function then determines based on that timeline what to do. In the case of the Wipe, it:

Takes the current time
Breaks the time down into a cycle
Determines where in the cycle it currently is. Ie if my current time is 30 seconds and my cycles operate at 20 seconds, I'm therefore 10 seconds (out of 20, 50%) into the second cycle.
Light up the string based on where it should be at 50%.

What I therefore ended up doing is modifying that cycleTime to also include a delay portion, which I called totalTime. So if my cycleTime was 20 seconds and my delayTime is 10 seconds, my totalTime is 30 seconds. So if it determines its at 25 of 30 seconds, the function ultimately should be doing nothing. I would have ultimately used aux0 and aux1 to record a time and simply wait until it elapsed, but these variables were already used to track next color and current color.

Then 1 more complication to wipe overall is its actually built in 2 stages: light on then turn off in reverse. If the reverse portion isnt used, it simply changes color after the first half so it looks like 2 cycles but its actually 1. So I'm actually adding a delay after each of the 2 wipes.

Hope that helps the next person.

The edits are exclusive to fx.cpp in the color_wipe function and the meta data for mode_color_wipe_random (to add the custom1 slider)

