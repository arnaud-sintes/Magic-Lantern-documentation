# ML raw video technical overview

## Purpose

This documentation is an attempt to centralize *medium-level* technical information about **raw video recording** on [*Canon EOS* cameras](https://en.wikipedia.org/wiki/Canon_EOS) supported by [*Magic Lantern*](https://www.magiclantern.fm/) (shortened *ML* in this document).

By *medium-level*, we mean to provide information that are not specifically intended to a *developer* audience (no direct pointers to any *ML*-related codebase) nor *ML*'s high-level consumers (no direct pointers to *ML*'s menus or specific distribution) but that may be interesting for both groups to get better understanding about how the whole thing works.

> [!NOTE]
>
> Even if most of the information provided in this document *may be* agnostic enough regarding *Canon* camera models, it's possible some are only applicable to the specific model used for reference, being the [*Canon EOS 5D Mark III*](https://en.wikipedia.org/wiki/Canon_EOS_5D_Mark_III) (shortened *5D3*).

## Raw data

Video being basically a *sequence of images*, it's important to first focus on a single **image** produced by the camera.

*Canon EOS* cameras captures the incoming light rays entering the camera through the [lens](https://en.wikipedia.org/wiki/Camera_lens) using a [digital image sensor](https://en.wikipedia.org/wiki/Image_sensor).

This image sensor is itself composed of multiple **red**, **green** and **blue** (*[RGB](https://en.wikipedia.org/wiki/RGB_color_model)*) individual [photosensors](https://en.wikipedia.org/wiki/Photodetector), whose numerical data goes through a **filtering layer** used to distribute the *RGB* information on a specific **square grid** storage named a **color filter array** (*[CFA](https://en.wikipedia.org/wiki/Color_filter_array)*).

The specific storage pattern used on the *Canon EOS* cameras is the [Bayer filter mosaic](https://en.wikipedia.org/wiki/Bayer_filter), allowing to store one *half green*, one *quarter red* and one *quarter blue* values per *cell* (see later for the definition of a cell).

To illustrate this, imagine first the image sensor of the camera as a rectangle with a specific dimension, e.g.: *35mm* "[full-frame](https://en.wikipedia.org/wiki/Full-frame_DSLR)" on the *5D3*, being approximatively *36mm* horizontal and *24mm* vertical, the surface of this rectangle being fed with multiple *RGB* photosensors, e.g.: *5760 x 3840* on the *5D3* (*22.1184* millions).

TODO use HTML with colors

```
 ----- ----- ----- ----- ----- ----- ----- -----  ... (5760 columns of photosensors on 36mm)
| RGB | RGB | RGB | RGB | RGB | RGB | RGB | RGB |
 ----- ----- ----- ----- ----- ----- ----- ----- 
| RGB | RGB | RGB | RGB | RGB | RGB | RGB | RGB |
 ----- ----- ----- ----- ----- ----- ----- ----- 
| RGB | RGB | RGB | RGB | RGB | RGB | RGB | RGB |
 ----- ----- ----- ----- ----- ----- ----- ----- 
| RGB | RGB | RGB | RGB | RGB | RGB | RGB | RGB |
 ----- ----- ----- ----- ----- ----- ----- -----
 ... (3840 rows of photosensors on 24mm)
```

The *RGB* data acquired by the sensors goes through the filtering layer to be stored following a organized *Bayer mosaic*: to do so, the photosensors *RGB* data on the rectangle surface are **grouped by four** following a ***2 x 2** pattern*, resulting also in a ***2 x 2*** ***RGGB*** space storage on the filtered mosaic (what's called previously a *cell*) where single **red** (*R*), **green** (*G*) and **blue** (*B*) information are organized the following way:

```
   ----- -----            --- ---
  | RGB | RGB |  filter  | R | G |
   ----- -----     ->     --- ---
  | RGB | RGB |          | G | B |
   ----- -----            --- ---
2x2 photosensors   2x2 Bayer mosaic (cell)
```

This is why it was said before the *red* and *blue* information represents *quarter* photosensors values (*single* individual *red* and *blue* storages on the mosaic corresponding to the averaging of *four* original *red* and *blue* sensor data) when stored on the *Bayer* mosaic but the *green* information is *half* photosensor value (*two* *green* storages on the mosaic corresponding to the averaging of *four* original *green* sensor data).

After this filtering operation, the *Bayer* pattern looks like this:

```
 --- --- --- --- --- --- --- --- --- --- --- ---  ... (5760 column)
| R | G | R | G | R | G | R | G | R | G | R | G |
 --- --- --- --- --- --- --- --- --- --- --- ---
| G | B | G | B | G | B | G | B | G | B | G | B |
 --- --- --- --- --- --- --- --- --- --- --- ---
| R | G | R | G | R | G | R | G | R | G | R | G |
 --- --- --- --- --- --- --- --- --- --- --- ---
| G | B | G | B | G | B | G | B | G | B | G | B |
 --- --- --- --- --- --- --- --- --- --- --- ---
 ... (3840 rows)
```

Each individual *red*, *green* and *blue* components in this *Bayer* pattern being encoded using an (*unsigned*) **14-bits** data numerical storage. 

> [!IMPORTANT]
>
> When we talk about **raw data** (image or video), we actually talk about this exact ***Bayer* pattern storage** content (or a *subset* of it with smaller horizontal and/or vertical dimensions), meaning basically here an array of *5760 x 3840 x 14-bits* data organized following this *RGGB* 2x2 pattern in memory.

Because of this specific *RGGB cell* storage, a [demosaicing](https://en.wikipedia.org/wiki/Demosaicing) post-process (also commonly named ***debayering*** or *CFA interpolation* process) is required to **reconstruct the colors** of the final image in order to obtain a regular *5760 x 3840* image with three  regular *RGB* channels per *pixel* (still *14-bits* data range per channel) on which we can work with

Multiple *debayering* algorithms (e.g.: *AHD*, *AMaZE*...) are available in photography or videography post-process software, computing different reconstruction results.

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