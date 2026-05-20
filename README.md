# BirdNetDisplay
A wall-display front-end for BirdNet-Go, built to feel like a naturalist's field journal rather than a dashboard. It rotates through the birds heard on your property today — photo, when and where each was calling, a small drawn activity chart. This consists of a single self-contained HTML file.


# The Garden Register

A wall-display front-end for [BirdNET-Go](https://github.com/tphakala/birdnet-go), built to look like a naturalist's field journal rather than a dashboard. It rotates through the birds heard on your property today — each one shown with its photo, the time it was heard, where on the property it was picked up, and a small drawn activity chart.

Page one:

<img width="1445" height="827" alt="Screenshot 2026-05-20 at 4 17 51 PM" src="https://github.com/user-attachments/assets/2fa09b1d-a1cc-4364-ad4e-d29f45b7282b" />

Page two:

<img width="1457" height="840" alt="Screenshot 2026-05-20 at 4 18 22 PM" src="https://github.com/user-attachments/assets/55610d99-4054-4378-b7a8-3eb50319c95d" />


## How this happened

I have a BirdNet-Go installation on my home server and an old iPad I wasn't using. I wanted something I could mount on the wall that would quietly cycle through the day's birds and feel more like a journal entry than a Grafana panel. I worked on it over a long stretch of evenings until it felt right, posted a screenshot to Reddit, and people asked if they could have the code. So here it is.

It's one self-contained HTML file. It just talks to your BirdNet-Go server's API.  You WILL need a place to 'serve' this file.  I use a local home automation server.  There may be a way to roll it into BirdNet-Go, but I haven't explored that.

## What it does

- Rotates through the birds heard on your property today, freshest first
- Pulls each bird's photo and a short description from Wikipedia
- Marks new arrivals — when something is detected for the first time today, it jumps to the front of the rotation
- Shows whether a bird is "Calling now" or "Heard earlier"
- Tap the photo to play the actual recording from your BirdNET-Go server
- Tap "learn more" to turn the page to a detail view with the full description and (when Wikipedia has one) a range map
- Goes fully dark at night so it doesn't light up the room
- Refreshes the date on its own at the start of each day

## Setup

1. Drop `bird-dashboard.html` somewhere a browser can reach it (any static file host — I used HomeSeer's `html` folder; a Raspberry Pi running a web server works fine, or any device with a way to serve a file)
2. Open the file in an editor and set `BIRDNET_GO_URL` near the top to your BirdNet<img width="1448" height="832" alt="Screenshot 2026-05-20 at 4 15 55 PM" src="https://github.com/user-attachments/assets/c273df33-0f11-4e9d-a59e-1659ec964e0e" />
-Go server's address, including the port (e.g. `"http://192.168.1.50:8080"`)
3. Adjust the other values in the `CONFIG` block to taste — station name, the "Birds heard here" line, blackout hours, etc.
4. Point a browser (or an iPad) at the file

If you leave `BIRDNET_GO_URL` blank, it runs in demo mode with sample birds, which is useful for previewing.

## Notes on the design

A few things that look like quirks but are deliberate:

- The bars on the activity chart are intentionally uneven.  They are drawn each turn with a small deterministic wobble so the chart looks hand-sketched rather than computer-plotted
- The frame around the photo is an arch because that's what plates in old field guides looked like
- "Heard earlier" and "Calling now" use different colors so you can tell at a glance whether the bird is actively around or just on today's list
- The page only shows one stat ("Heard today") at a glance.  The rest was moved to the detail page because they're not legible from across a room
- It picks fresh birds first but won't bury anything.  Every species gets seen before any repeats

## Known limitations

- **Range maps depend on Wikipedia.** Many bird articles have a range map; some don't. When there isn't one, the page two just shows the photo and the description.
- **Wikipedia range maps clash a bit.** They come in wildly different styles. There's a sepia tint to pull them toward the page palette, but it can't make an ugly map elegant.
- **Audio playback needs the detection's clip to still exist.** BirdNet-Go prunes old audio on its retention schedule, so very old detections may have no recording to play. In practice today's birds are fine.
- **Tested on iPad Safari and desktop Chrome.** Older iPads with very old iOS may render the arch shape oddly — iOS 15+ is fine.

## Credits

Photos and descriptions: [Wikipedia](https://www.wikipedia.org). Audio detection and the data this dashboard runs on: [BirdNET-Go](https://github.com/tphakala/birdnet-go) by Tomi Phakala.

Built collaboratively with [Claude](https://www.anthropic.com/claude) (Anthropic) over a series of long design conversations.

## License

MIT — see [LICENSE](LICENSE).
