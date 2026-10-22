# Boarding passes

A static train-trip snapshot app. No PNR or trip details are committed to the repository. A traveler receives a private snapshot link; its data lives in the URL fragment until the page imports it into that browser's localStorage.

## Import-link format

Root URL with a fragment (fragments are not sent in HTTP requests):

`https://5hu3hendu.github.io/pnr-tracker/#add=<percent-encoded-JSON>`

The decoded JSON has this shape:

```json
{
  "version": 1,
  "trip": {
    "id": "unique-trip-id",
    "pnr": "1234567890",
    "train": "19489",
    "name": "ADI GKP EXP",
    "cls": "3A · AC 3 Tier",
    "from": "Ahmedabad Junction",
    "to": "Varanasi Junction",
    "date": "Saturday, 24 October 2026",
    "time": "9:10 AM",
    "arrdate": "Sunday, 25 October 2026",
    "arrtime": "12:20 PM",
    "duration": "27 hr 10 min",
    "distance": "≈ 1,454 km",
    "platform": "8/9",
    "platformState": "Tentative · schedules differ; recheck at station",
    "coach": "B3",
    "berths": ["24 Side Upper", "23 Side Lower"],
    "cancel": "",
    "checked": "2026-10-22T20:17:00+05:30",
    "addedAt": "2026-10-22T20:17:00+05:30",
    "coaches": ["SLR", "GS", "A1", "B1", "B2", "B3", "S1"],
    "endAt": "2026-10-25T12:20:00+05:30"
  }
}
```

To generate the fragment in JavaScript:

```js
const encoded = encodeURIComponent(JSON.stringify({version: 1, trip}));
const importUrl = 'https://5hu3hendu.github.io/pnr-tracker/#add=' + encoded;
```

The app validates the envelope, escapes text before rendering, adds the trip to localStorage, de-duplicates the same PNR/train/journey date, sorts upcoming trips by arrival time, then removes the fragment from the address bar. Do not include passenger names. Keep the PNR inside the fragment; the page never sends it to a server. Anyone who receives an import link can read that snapshot, so share it privately. Snapshot details do not refresh automatically; the Refresh button asks the owner to send a new snapshot.

The first-load demo is clearly labeled fictional and contains no real PNR. Deletion appears after the scheduled arrival; ended trips are pruned at 48 hours on the next page open (a static app cannot run cleanup while the browser is closed).
