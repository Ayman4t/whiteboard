# Inkroom: presentation page with the whiteboard built in

`index.html` is one file. It contains the presentation page AND the whiteboard.

- "Create a room" opens the whiteboard inside the page with a new room code.
- "Join" opens the whiteboard in an existing room.
- Invite links look like `https://YOUR-SITE/?room=CODE` and open the page straight into that room.

To use it, upload `index.html` to any static host (for example GitHub Pages) over https.
Rooms and video need an internet connection and https (or localhost).

## How users connect

The first person to open a room becomes the host (their browser is the hub). Everyone else
connects to the host directly (WebRTC through PeerJS), and the host approves each newcomer.
If the host leaves, one of the remaining guests takes over automatically.
Room ids on the signalling server look like `inkroom-<code>`.

## Notes

- PeerJS's free public signalling server and relay servers are best effort. For a classroom or
  company deployment, run your own PeerServer and TURN server and pass them to `new Peer(...)`.
- When a host hands over, the new host has to approve everyone again (approvals are stored in the
  old host's browser).
- Boards saved by the old name (SketchMesh) are carried over automatically on first open.

## Changes from SketchMesh

- Renamed to Inkroom (page, whiteboard, room ids, saved-data keys, exported file names).
- Large messages (boards with many shapes, photos) are split into small pieces. Before, a guest
  joining a busy board could end up with an empty one.
- Two tabs of the same browser no longer kick each other out in a loop. A duplicated tab replaces
  the older one cleanly.
- A stray network or signalling error no longer throws a connected guest out of the room, and no
  longer shows the host as "Disconnected".
- When the host's link to the signalling server reconnects (laptop wake, mobile network switch),
  the guest list and chat are kept instead of being reset or duplicated.
- Starting a call while the signalling link is down no longer breaks the people list.

- Removing a guest no longer bans their device. If they open the room again they appear as a new
  request and the host can allow or deny them. Bans saved by older versions are cleared.
- New voice-only button next to the camera button: talk with the microphone only, no camera. Voice-only
  people show as a small name tile. Switching between voice and camera is one click.
