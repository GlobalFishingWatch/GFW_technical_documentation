The satellite timing offset table contains estimates of how far off the satellite clocks are for each Spire satellite. It's only Spire at present, because that is the only set of satellites we have IDs for. Exact Earth satellite's are reputed to have very stable clocks so it may not be necessary there, but might be useful for shore based AIS if we ever get IDs for individual stations.


## Key Tables

+ `pipe_ais_v3_alpha_published.satellite_timing_offsets` 

### Source Tables
 * `pipe_ais_sources_v20220628.normalized_spire_*`

## Data Description

For Spire's satellite clocks there are two effects that we've seen. One is a steady, slow drift over time. This is rarely an issue since the clocks are periodically reset. The second effect is a sudden, large jump in clock's time which I suspect is due to bit flips in the clocks due to cosmic rays or similar unfriendly particles. This can offset the clock anywhere from minutes to days (about two days is the largest I've seen) and results in some very confusing tracks.

In order to estimate the clocks offset we compare the times from clocks on all of the satellites we have IDs for by looking at signals from vessels that are broadcast closely in time and and received by different satellites. By using the vessels location, course and speed we can estimate the time between when each signal was broadcast. Comparing this to the difference in the message timestamps gives an estimate of how far off the timestamps are for a pair of satellites. By looking across many vessels and pairs of satellites we derive estimate for how far off the clock for each satellites is from the consensus timestamp, that is the median of all the satellite clocks.

This table is derived directly from the `normalized_spire` sources, e.g., `pipe_ais_sources_v20220628.normalized_spire_*`

This table is used to automatically drop all the messages from satellites with clocks that are off by more than 30 s (I need to check that), this happens on an hour by hour basis. This only applies to Spire. 

The messages are dropped because they really mess up the tracks particularly when the clocks get really off. When you really notice is when two receivers are getting messages and one is a few minutes off, the boat periodically appears to move backward and forward in space when you map it, although it’s really the timestamps are off and the position information is fine. Often the segmenter will drop these into their own bad segment, but not always.




