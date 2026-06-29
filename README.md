# Distributed Music Streaming Application

A distributed **music streaming** system built on a **publish–subscribe**
architecture, with a Java backend and an Android client. Developed for the
*Distributed Systems* course at the Athens University of Economics and Business
(AUEB).

## Architecture

The system is split into independent nodes that communicate over the network:

- **Publishers** — hold the actual songs. Each Publisher is responsible for a range
  of artists (e.g. one for A–M, one for N–Z), reads the audio files, extracts their
  metadata (artist, etc.) using **Apache Tika**, and splits each track into byte
  **chunks** for streaming.
- **Brokers** — act as intermediaries between clients and Publishers. Clients connect
  only to Brokers; Brokers know which Publisher holds which artists and route requests
  accordingly.
- **Client (Android app)** — a consumer that requests a song; the request travels to
  the responsible Broker, which fetches it from the appropriate Publisher.

Communication between nodes uses **TCP sockets**, **multithreading** and Java
**object serialization**.

```
Android Client  ->  Broker(s)  ->  Publisher(s)  ->  song files
```

## Playback modes

- **Streaming (ON)** — the song arrives in chunks and playback starts as soon as the
  first chunk is received; the rest loads progressively.
- **Download (OFF)** — the whole song is downloaded first, then played, and saved to a
  list of downloaded songs for offline listening later.

## Project structure

```
ProjectKatanem/   Java backend (Brokers and Publishers)
MyMusicApp/       Android client (Android Studio project)
```

## How to run

1. Open `ProjectKatanem` in a Java IDE (e.g. IntelliJ IDEA) and `MyMusicApp` in Android Studio.
2. Start the **Brokers** first, then the **Publishers** (which read the song dataset and
   connect to the Brokers). Brokers and Publishers must stay running throughout.
3. Launch the Android app and connect to a Broker to search for and play songs.

> Brokers and Publishers are designed to run on separate machines. To test on a single
> machine, use the same local address with different ports for each node (the addresses
> and ports are listed in `data/brokers.txt`, which Publishers read to find the Brokers).
> Some paths and addresses (the dataset path, the local IP) need to be adjusted for your
> own setup.

## Tech stack

- Java
- Android Studio (mobile client)
- TCP Sockets, Multithreading, Object Serialization
- Apache Tika (audio metadata extraction)

## Notes

Academic group project. The song dataset is not included and must be provided separately.
