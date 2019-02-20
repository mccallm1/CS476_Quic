# Instructions for setting up and testing QUIC protocol

## QUINN

https://github.com/djc/quinn

To build quinn, you will need to install rust version >= 1.32.
There are some changes to how rust handles const released in version 1.31, and updating to 1.32 fixed the issues I was having.
I performed my tests on the WSL running on windows 10.
Installing rustc from apt gave me only version 1.30, so to properly install is you need to uninstall cargo and rust from WSL, and use the command: `curl https://sh.rustup.rs -sSf | sh`.
Once that is complete and installed, you need to eithre restart the terminal or use the command `source $HOME/.cargo/bin` to add cargo and rustc to the PATH.
Once I had rust properly installed, I was able to build and run the client and server.
For recordingperformance metrics, it is important to build them in release mode.

### Server

To build the server, use the command `cargo build --release --example server`.
This will build the server code and place it in `./target/release/examples/`.

When starting the server, it is important to pass in both the address to listen to (`--listen` flag) and the folder from which to serve files.
Here's the command I used to run it: `./target/release/examples/server --listen 127.0.0.1:4433 ./`.
Within the current directory, I created a folder named `www`, which is where I placed the barious test files I generated.  
Look at the test files section to see how I created them.

### Client

Before building the client code, I made two modifications to the source.
when reading randomly generted files, it will trip up on weird character sequences.
This is an issue when the client code tries to output the fetched file to the screen.
to fix it, I commented out lines 149, and 150.
you will get a warning about line 7, but is ok to ignore.

To build the clinet, I used `cargo build --release --example client`.
This will build the client code and place it in `./target/release/examples/`.

When running the client, you only need to supply the address to request.
Even though `127.0.0.1` was supplied to the server, when running the client is important to use `localhost`.
This is because the client will to a DNS lookup on the address, and it will fail with the plain ip.

To run the client, I called `target/release/examples/client https://localhost:4433/www/250mb.file`.
I had previously created a folder named `www` and a 250mb file within it.  
The path after the port number is based on the path given to the server when you start it.

The client will download the file, then give a summary consisting of when it connected, requested, and recieved a response.
Here is some example output:

```()
root:quinn# ./target/release/examples/client https://localhost:4433/www/250mb.file
connected at 3.1152ms
request sent at 4.7796ms
response received in 18.3998206s - 13268.642 KiB/s
drained
```

## Test tools

### Wireshark

https://www.wireshark.org/download.html

To capture packets, I used wireshark.  In order to be able to capture quic packets, you will need to download version >= 3.0.0.
My copy of wireshark is at 3.0.0rc1 (v3.0.0rc1-0-g8ec01dea).

When capturing those packets, be sure to filter the traffice to quic only by typing in `quic` in the filter bar.
This is how I captured the pcaps.

### Spindump

https://github.com/EricssonResearch/spindump

This is used to statically analyze quic packets and get more performance metrics from them.
To install

1. First clone the repository: `git clone https://github.com/EricssonResearch/spindump`.
2. Install the necessary packages: `sudo apt install make gcc libpcap-dev libncurses5-dev`
3. `cd spindump`
4. Build it: `make`
5. Install it: `sudo make install`

Now spindump should be installed.
You can check if it is by running `spindump --version`.
This is what I got:

```()
root:spindump# spindump --version
version 0.11 January 22, 2019
```

Iwasn't able to get spindump to listen on the lo interface, but I believe it is because of how the WSL netowkring is setup.
Instead, you are able to feed in PCAP-format files for it to analyze through the `--input-file` flag.

## Test Files

To test the protocol, it is useful to have files of different sizes to test speeds etc.
To create a 250mb file full of random data, I used `head -c 250MB /dev/urandom > 250mb.file`.
To create a file full of zeros, you can use `head -c 2MB /dev/zero > 250mb.file`.

Be sure to place these files somewhere in the path of the directory passed in when starting the server.