# Artistry Design Guide

Artistry should be 100% open-source, decentralized, and scalable.


## OVERVIEW

(0) framework
daemon service or chrome extension? target market?

single point of failure?

(1) audio identification
audio fingerprinting or metadata extraction?  what is
the canonical source for ids - musicbrainz, gracenote, spotify, or our own blockchain
based hash lookup?

Homespun might need a blockchain solution for storing hashes and a
'distributed server' model for actually doing fast hash-scoring of audio files. 
Creating a 'distributed server' model (run by volunteers) creates a vulnerability
of having to store a lookup of distributed servers which may come on/off line.

(2) audio id database/blockchain solution


(3) artist identity verification

1 blockchain where anyone can push an artist id and an email string, store IP
or location of person that pushes?

1 blockchain that uses logic from mining the artist id and email string
blockchain to add artistid to payment info, only accepts emails from a verified
email source as determined by first blockchain

(4) payment




## 0. Pick a Framework and Target Audience

(metric of 'likely adoption/impact' first, mass-market scalability second)

Chrome Extension?
    -broadly appealing, most popular browser (32%)
    -does not allow direct access to audio, but we can build on last.fm scrobber
    -only captures audio played through browser

Firefox extension?
    -narrow appeal (9%)
    -users skew more privacy-conscious, tech-savy
    -allows direct access to audio, could directly translate to desktop version 

OSX/Linux compliant daemon/menulet?
    -narrow appeal (10% of OS)
    -users are tech-savy, appeal to open-source/tech crowd
    -able to listen to music played on any player including through browser

Other options?  Windows daemon?  Mediaplayer plugin?


My instinct is that OSX/Linux background app makes the most sense, mostly
because I think the type of people that will understand and appreciate this
type of technology, and who would use/appreciate digital currency (assuming we
don't entertain USD-payment service based solutions), don't listen to music
primarily through their browser.  I'm not sure most people do (most people I
see using spotify have downloaded the app and don't stream it through a browser).


I wonder about the distributed nature of this-- is it to eliminate a single
point of failure?  That could be hard.  I see two ways to go:

#1
(1)Chrome extension, using scrobbler scripts to pull metadata
(2)need to hit some central API *or* create our own metadata-to-songid
and metadata/songid-to-artistid distributed service>
(3)push songids to blockchain

more ubiquitous and less overhead to install, BUT how many people primarily use
browser for listening?  how many people that are interested in indie artists
and subverting the music industry listen to music in this way?  How seriously
do these people take chrome extensions?

#2
(1)daemon in background, write our own hash-based lookup similar to Shazam
(2)need to hit our distributed hash-to-songid/artistid service, requires a few
hundred gigs per hash table (real servers, potentially).
(3)push songids to blockchain

This seems more in line with the community I think will want to adopt this--
you can use any music streaming service and protect your privacy
(open/controled data crowd).  It's more ubiquitous, the tech is scalable and
future proof unlike metadata extraction.  It also decouples completely the
content from the metadata, and the content delivery service from the payment.
I could see the pirate/torrent community embracing something like this--
politically it ticks the important boxes, *and* it looks to complete their
ecosystem in a certain way.  They have an anonymous and distributed way to
distribute content, they need an anonymous and distributed way to support the
artists and creators that create/distribute that content.

I'm not sure how to structure distributed fast lookup without some central
reliable reference address table that 'always works' (like DNS).  Update shipping software to
have a list of most recent lookup nodes and cache locally?  Design each new one
to ping and share addresses among themselves so we have an up-to-date list of
fast hash lookups?



## 1. Identify the Audio

If we do a chrome tab, we can scrape the audio in a site specific way using the
last.fm features. These can live on the host computer.  No audio
fingerprinting, and reliability is subject to site metadata quality.

If we do a background app, we need to fingerprint the audio.  This requires
some sort of database of audio hashes and an audio fingerprinting algorithm
that can live locally.  Perhaps a blockchain based approach for storing
audio-fingerprint hash to id lookup?  (it certainly will continue to grow).

The other options are musicbrainz acoustID or gracenote's service (paid).

Hashing in blockchain?


### Shazam
[Here's the white paper](https://www.ee.columbia.edu/~dpwe/papers/Wang03-shazam.pdf)
```
Shazam works by taking constellations on the spectrogram (chosen based on
energy in a local region with some density constraint), then 'anchor points'
are chosen (middle of the spectrum, density constraint) and a 'target region'
(a region of the song offset from that anchor point in time and freq) for each
anchor point. 32 bit Hashes are built from the anchor point and each point in
the target region encoded with the time delay (deltaTIME, FREQanchor,
FREQtargetpoint).  Searching for all of these hashes, pulling the songs that
match, and 'scoring' each song based on the number of matching hashes is the
way we arrive at a winner.

This is done on every anchor point in a 15 or 30 second slice of audio, which
may be 3-4 anchor points with 7-8 fanout points from each (on the order of 40
or 50 points).  objective anchor time offsets are stored when encoding the hash,
and time offsets are saved when doing this with each anchor point (even though
we only have relative times). Time offsets are removed based on first anchor
point values, 'matches' are scored by matches hashes for # of songids with
expected anchor-point offsets.

Spotify has 20,000,000 songs+
20,000,000 songs * 4 min/song * 4 frames/min * 50 hashes/frame * 8 bytes/hash+id
= ~128 GB to store a fast hash audio-fingerprint database for all of spotify
(assumes 32 bit hash, 32 bit for time offset and song id)

one second resolution at 7 bits can encode first 2 min of every song, for
25 bits = 33,000,000 songs

```
Shazam (I assume) has legal protection on this for at least another decade or
so, but we could see how much we need to change in order to use it.  We can be
a little less robust since we aren't operating under the same tight constraints
as Shazam. (We're getting a direct copy of the audio stream).



## 2. Store it Locally

Store name/artist/songid locally

allow selective editing of payment
allow control over whether they want to share their listening history or not
UI decisions


## 3. Bulk Data Transfer to Blockchain



## 4. Listening History



## 5. Artist Identity Verification


## 6. Payment
