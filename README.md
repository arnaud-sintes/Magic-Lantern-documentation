# RAW video technical overview

## Purpose

## Disclaimer

## Raw data

Raw data / Bayer pattern / 14bits, image resolution

## Raw images

Raw photo, CR2 format / LJ92 lossless compression

## The LJ92 compression algorithm

LJ92 compression rate in different situations (low light etc.)

## Jpeg images

Jpeg photo / 8 bits per RGB channel conversion / YUV color space conversion / chroma subsampling (4:4:4, 4:2:2, 4:2:0) / 4:2:2 chroma subsampling YCbCr / compression depends of quality parameter (fine: ~90%, normal: ~75%) / quality Large/Medium/Small affects image resolution (downscaling)

## Native video

MOV / H.264 video / 4:2:0 chroma subsampling internal (can deal with 4:2:2 with external HDMI recorder) / native 1080p resolution , bitrate (91MBps ALL-I - Intra-frame compression, 30Mbps IPB - Inter-frame compression) / note Magic Lantern can crank-up the H.264 bitrate

## Video profiles

recording profiles and LOG video

## Raw video

ML RAW video / MLV / LJ92 algorithm (true lossless), no YUV conversion like RAW photo (keep Bayer pattern)

## Bit-depth decimation

ML lower bitdepth (12 & 10 bits) / decimation / dynamic range, incidence on LJ92 compression rate (entropy)

## Bus bandwidth

why it's important: camera bandwidth (e.g: 5D3, SD & CF limits), stability / limit / recording buffer saturation (compression, write, flush) / drop, required bandwidth computation depending of resolution & bitrate

## Workarounds

workarounds: SD overclock (link to compatible cards), SD+CF card spanning

## Post-processing

post-process workflow, constraints, MLV App, MLVFS, Resolve