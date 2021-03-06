- logging (configgy)
  - feeds
  - peers
  - trackers
  - webfetch
  - comet functionality
  - per page basis?
  - need separate logs for "errors" and "standard operation"

- httpclient
  - gzip compression
  - etag
  - last-modified
  - custom timeout
  - retries?
  - move to WebFetch
    - Feeds
  - use multi-thread fetching on a per-peerwatcher basis? (maintain one
    connection indefinitely)
  - disable stale connection checks
  - instrument with log4j via. commons-logging
  - disable cookie processing by default (allow enabling easily)
  - allow fetching from udp as well as tcp

- FeedWatcher
  - rename to FeedWatcher (both object and class?)
  - don't silently ignore duplicates, check the hostname. If it's different,
    log that somehow.
  - allow udp torrent sources
  - when new sources are discovered, how does the info object for the torrent
    get updated? Or, should the torrent objects periodically check the db for
    new sources?
  - When a bencodedecoder.decode fails, save the torrent to disk for looking
    at in the future.

- PeerWatcher
  - need to globally disable trackers that aren't up
  - periodically check globally disabled trackers and enable if they're up

- make sure that httpclient dependencies get downloaded/built correctly by maven
  - Is there a way to have dynamic versions for this based off a dynamic
    httpclient version? no reason to stick on a snapshot.

- models
  - PKs should be UUIDs and not something else
  - Peer needs to have no PK
  - TorrentState doesn't need a PK
  - info_hash should be actual binary data to save space
  - use info_hash as the PK instead of an actual PK
  - Peer needs a link to tracker
  - add valdiation inside the models
  - setup the indexes correctly
  - add UUIDKeyedMapper and UUIDKeyedMetaMapper (and allow UUID to be mixed in
    just like IdPK).
  - move new_*_? methods to somewhere common to ensure they're not sprinkled
    everywhere randomly.
  - create a mapped ip field that does the conversion from/to int for me

- tracker communication
  - for trackers that support it, add multiple info_hash/request
  - handle a dictionary model for peers
  - obey min interval if listed
  - use scrape for trackers that support it to fetch "downloaded" information

- ActorManagement
  - actively remove watchers that are dead.

- State
  - This needs help all over the place.
  - Auto-fail trackers that aren't valid via. the Info object passed in (and
    modify the state object.
  - Group scrape requests ..... somehow, probably pass a scraper into the state
  - fold the fetch for both Announce and Scrape into a parent trait.

- Lift bugs
  - Unique key unusable
  - MappedStringForeignKey creates the db as if it's a BIGINT
    (gogo cut and paste?)

- info
  - go through and get the default "None"s removed. Should be raising an
    exception.
  - handle port additions as the same tracker.

- nginx
  - setup bots and spambots config
  - make sure access_logs are working right

- encodings
  - do transcoding of torrent names to utf-8. Is there a way to do this in java?