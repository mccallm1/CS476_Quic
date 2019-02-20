# Create and plan out Simulation tests

## Data Speed Performance
Record time it takes to send n bytes of data.
Perform this test multiple times, for different sizes of files.
Probably file sizes along the lines of 1kb, 10kb, 100kb, 1mb, 5mb, 10mb, 100mb, 1gb.
If we don't want to do that many tests, just doing 10kb, 10mb, and 1gb would probably suffice.
Measure: bytes/second

## Data Overhead
Record amount of data sent (n+overhead) when sending n bytes of data
This can be done while testing data speed performance.
While performing teh tests, we can take packet captures, and use them to measure total bits sent over the wire.
Measure: data size in bits/bits over wire

## Connection Establishment
Record time it takes to establish, and amount of back and forth communication needed to establish.
Simplly a packet capture and analysis.
We probably want to note how  many frames are exchanged, the time it took to exchange them, and the size of each frame.
Measure: # Frames sent, Size of frames sent, time to send frames, time to establish connection

## Failure Recovery
Introduce errors at a rate of errors/pkts, record amount of extra communication or time it takes to fix the error(s).
To test this we would tranfser large files, and use the qvalve tool to introduce errors in a deterministic way.
Measure: time to transfer with errors vs time without errors based on error rate


## Congestion Control
?
Maybe only analyze?
## Connection Migration
?
Maybe only analyze?

# Create and plan out Analytical tests
Much of the analysis can be done by analyzing and calculating values based on the draft specification.
The quic client I've been able to build so far uses draft 18, so we would want to reference that draft.
The text of the draft can be found here: https://datatracker.ietf.org/doc/draft-ietf-quic-transport/18/ .

## Data Speed Performance
Analyze code/spec to calculate speed performance for connection of n bits/s

## Data Overhead
Analyze header sizes and amount of communication used to establish connection.
This should be pretty simple, just need to reference the spec.
Look at section 19: Frame Types and Formats (Page 97 of draft-18).

### Connection Migration
Look at section 9: Frame Types and Formats (Page 44 of draft-18).

## Security
What sort of inherint security controls are present in the spec.
Look at section 21: Frame Types and Formats (Page 116 of draft-18).

## Error rate handling
Look at how it deals with errors and recovers from them.
