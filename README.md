# Vinyl Audio Visualizer (Max/MSP + Jitter)

A real-time audio visualizer built in Max/MSP and Jitter. It listens to the microphone, analyses the incoming sound with an FFT, and draws the frequency content as a rotating 3D "vinyl record". Waveforms rise from the surface of the vinyl as sound is heard: **low frequencies sit towards the centre and high frequencies towards the outer edge**, peaking in 3D space in real time.

![Presentation mode](screenshots/presentation-mode.png)

## Inspiration

Inspired by the video for *The Oracle*, a theme song by A.R. Rahman, one of my favourite musicians.
Reference: [The Oracle – A.R. Rahman](PASTE-YOUTUBE-LINK-HERE)

## How it works

1. **Audio input** – `ezadc~` takes the microphone signal. A `spectroscope~` and `meter~` show the raw input.
2. **Frequency analysis** – `pfft~ FreqAnalysis.pfft 512` runs an FFT. Inside the subpatch, `cartopol~` converts each bin to amplitude, and `jit.poke~` writes the amplitudes into a shared Jitter matrix called `analysis`.
3. **Spectrum history** – `jit.dimmap` and `jit.matrix spectrum` (250 × 256) build a scrolling history of the spectrum, so each new frame pushes older frames along.
4. **3D geometry** – a `jit.gen` patch maps the spectrum onto a sphere-based surface, turning amplitude into height (z), which gives the vinyl its peaks.
5. **Rendering** – `jit.gl.mesh` (draw mode `line_strip`) draws the geometry inside `jit.world`. `jit.gl.handle` lets you rotate and zoom the vinyl with the mouse.

## Requirements

- Max 8.1 or later (built with Max 8.1.11)
- A microphone or audio input device

## How to run

1. Download or clone this repository.
2. Open `Atharva_Bankar_Assesment_01.maxproj` in Max (this opens the main patch).
3. Turn on audio by clicking the microphone (`ezadc~`) button.
4. Click the large toggle to start `jit.world` – the render window opens.
5. Make some sound and watch the vinyl react. Drag in the render window to rotate it.

## Project structure

```
Atharva_Bankar_Assesment_01/
├── Atharva_Bankar_Assesment_01.maxproj   # Max project file – open this
├── patchers/
│   ├── Atharva_Bankar_Assesment_01.maxpat   # Main visualizer patch
│   └── FreqAnalysis.pfft.maxpat             # FFT subpatch used by pfft~ (required)
├── screenshots/                             # Patch and presentation-mode images
└── README.md
```

## Acknowledgements

- Lectures by Francesc at university, which introduced me to Max/MSP and Jitter.
- Online tutorials (YouTube) on Jitter, FFT analysis and OpenGL rendering in Max.

## Author

Atharva Bankar
