
There are two versions of QUIC: the Google version and the IETF.

Google worked on their own implimentation, which is currently in use in the Chrome Browser.  It is imcompatible with the IETF standard, and as such I don't think we should use it.

If we changed our testing plan to compare implimentations of the protocol to each other, then it might be useful.
As of now, 

To make things more complicated, there are really two types of QUIC: QUIC as a transmission protocol, and HTTP-Over-Quic

# Implimentations to use
There are quite a few.  
The ones I think we should focus on testing are the IETF implimentations, though it may be useful to compare the IETF implimentation to the google gQUIC.
If we are testing one IETF version, then it doesn't matter what draft version of the protocol we use, though I think later might be better.
Wehn choosing an implimentation, it is important that it contains both a client and server.  
Also including a library might also be useful if we want to take advantage of it.


|Name|Draft Version|Roles|Language|Notes|Link|
|---|---|---|---|---|---|---|
|ats|17|Server/Client|C|Runs as a apache traffic server|https://cwiki.apache.org/confluence/display/TS/QUIC|
|ngtcp2|17|Server/Clinet/Library|C||https://github.com/ngtcp2/ngtcp2|
|picoquic|17,18|Server/Client/Libary|C|Includes test tools|https://github.com/private-octopus/picoquic|
|quant|18|Server/Client/Library|C11||https://github.com/NTAP/quant|
|quiche|17|Server/Client/Library|Rust||https://github.com/cloudflare/quiche|

We will need to investigate each more before choosing an implimentation.



# Tools we'll probably use
The reference for this list is located here: https://github.com/quicwg/base-drafts/wiki/Tools

## Wireshark
We can use wireshark to measure and record transmissions so we can look at them later.
In order to use wireshark, we will need to match the writeshark version to the QUIC draft version of the implimentation we're using.

Version requirements are detailed here: https://github.com/quicwg/base-drafts/wiki/Tools

## Quic Tracker
Instead of making test ourselves, it may also be useful to use this tool.
It is currently used to test the implimnetations of quic.
It can be compiled as a docker container, making it portable and easy to use.

The code for it is located here: https://github.com/QUIC-Tracker/quic-tracker

Here's a public instance of it: https://quic-tracker.info.ucl.ac.be/about

## qvalve
To test imperfect packet flows, this tool can be used.
It introduces errors into a quic stream such as dropping, reordering, or duplicating packets and sequences of packets.
It can easily be configured with rule sets so we can control what errors it introduces.
It acts as aproxy in between the client and server.

https://github.com/NTAP/qvalve

## spindump
This may be the most useful tool, as it is used to monitor traffic latency passing through an interface.
It doesn't monitor content, just information like round trip times etc.  It supports both TCP and QUIC, so we could easily get similar measurements for both protocols.

https://github.com/EricssonResearch/spindump


