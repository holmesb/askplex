# AskPlex

AskPlex is an Alexa skill that allows you to play music hosted by your Plex Media Server (PMS).
The official Plex skill is not available in all regions, so this skill serves as an alternative.

> ***Disclaimer:*** AskPlex does not provide any media content or sources. Users must provide their own content from a Plex Media Server. The AskPlex project does not support bootleg content or other illegally sourced material.

# Infra Prerequisites
Plex Media Server with your music library.
1. Audio files must be in MP3 format with bit rates between 16 - 384 kB/s.
2. DDNS service for your network (e.g., Duck DNS + IP update client).
3. Internet service and router must be able to open and forward port 443.
4. A reverse proxy between your router and Plex Media Server, so that it can be accessed via HTTPS on port 443.
5. The reverse proxy must present a valid and trusted SSL certificate. Self-signed certificates are not allowed.
6. Your Plex HTTPS URL must be accessible from the Echo devices network. If they are on the same network as the Plex server, your router must support NAT loopback.
7. An Amazon user account (must be the same as the one used in the Alexa app and on Echo devices).


# Install
1. Sign in to https://developer.amazon.com/ with your Amazon user account.
2. Go to the Alexa Developer Console at https://developer.amazon.com/alexa/console/ask and then create the skill.
3. Enter a name for your skill (e.g., AskPlex), select your primary locale, and enable Locale Sync.
4. Choose the "Music & Audio" experience type, a custom model, and Alexa-hosted (Python) service. Select the hosting region closest to your location to reduce latency.
5. In the "Templates" tab, click on "Import skill" and enter the AskPlex repository URL: (https://github.com/andresponte/askplex.git).
6. Wait until the installation finishes, then go to the CUSTOM menu and open Invocations -> Skill Invocation Name.
7. Set an invocation name for the skill (i.e. plex server).
8. Click on "Build skill". This will take some time.
9. Open the Code tab and edit the file "Skill Code/lambda/askplex/config.py".
10. Set the PMS_SERVER_URL, PMS_SERVER_TOKEN, and PMS_DEFAULT_SECTION_NAME. You can get your access token by following these instructions. For this step, please use a private browsing window to get a new access token; otherwise the AskPlex session will be closed when you end the session in your browser.
11. Click on 'Save' and then 'Build skill'. This will take some time.
12. When the deployment finishes, you can go to the Test tab.
13. Select Skill testing is enabled in Development.
14. Now you can type "open plex server".
15. If everything goes well, you should hear the reply: "Welcome to AskPlex. What would you like to do?"
16. Type: play music.
17. If the music starts playing, congratulations! AskPlex is now ready.

# Use
## One-step voice commands
### Basic playback
- Alexa, ask plex server to play music
- Alexa, ask plex server to play some music
- Alexa, ask plex server to play deep cuts
- Alexa, ask plex server to play popular artists
- Alexa, ask plex server to play new music / recently added

### By artist / song / album / genre / playlist
- Alexa, ask plex server to play music by Moonspell
- Alexa, ask plex server to play Full Moon Madness by Moonspell
- Alexa, ask plex server to play the album Irreligious by Moonspell
- Alexa, ask plex server to play Moonspell radio
- Alexa, ask plex server to play Moonspell newest
- Alexa, ask plex server to play Moonspell most recent
- Alexa, ask plex server to play Moonspell most recently added
- Alexa, ask plex server to play the metal music
- Alexa, ask plex server to play the playlist Recently Added

### Named tracks
- Alexa, ask plex server to play Brown Noise
- Alexa, ask plex server to play song Brown Noise
- Alexa, ask plex server to play White Noise

## What the newer commands do
- **play some music**: starts a Library Radio-style mix from a larger random candidate pool.
- **play deep cuts**: similar to play some music, but uses a smaller candidate pool so it is more likely to surface less obvious tracks.
- **play popular artists**: picks a small random set of artists, prefers each artist's popular tracks, then builds a mix from those.
- **play recently added** / **play recently added music** / **play recently added albums**: takes the 12 most recently added albums, shuffles the album order, then plays each album from first track to last.
- **play <artist> radio**: starts with tracks from the requested artist, then expands into similar artists to create an artist-radio style mix.
- **play <artist> newest** / **play <artist> most recent** / **play <artist> most recently added**: plays the most recently added album by that artist.
- **play Brown Noise** / **play song Brown Noise**: uses the named-track path for tracks you have added to the custom `track_names` slot list.

## Two-step voice commands
### Example

1. Alexa, open plex server
2. Welcome to AskPlex. What would you like to do?
3. play music by Moonspell

### Resume example
1. Alexa, open plex server
2. Welcome to AskPlex, you were listening to music by Moonspell. Would you like to resume?
3. yes

## Playback controls
Invocation name is not needed for playback control:
- Alexa, pause
- Alexa, stop
- Alexa, resume
- Alexa, next
- Alexa, previous
- Alexa, next album
- Alexa, previous album
- Alexa, shuffle on
- Alexa, shuffle off
- Alexa, loop on
- Alexa, loop off

## Notes
- For song-name-only playback to work reliably, the track title in Plex metadata should closely match what you say.
- The named tracks examples only work for tracks included in the custom `track_names` slot list.
- In the recently added albums mode, **next** and **previous** still move by track; use **next album** and **previous album** to jump between albums.
- The **newest** / **most recent** artist commands use the most recently **added** album in Plex, not necessarily the newest release by date.
- If a phrase builds successfully but Alexa still does not route it correctly, check the interaction model and rebuild the skill model.

