Command line DFT and tone generator. See live demo on
[YouTube](https://www.youtube.com/watch?v=hwsOKpBg6zo).

# Example of three tones interacting
![](fourier.gif)

# Build
```bash
git clone https://github.com/deanturpin/fourier
cd fourier/
make demo
```

# Analyse mic
```bash
make live
```
# Files
The main file is [fourier.cpp](fourier.cpp). There's also a tone generator
[tonegen.cpp](tonegen.cpp), a common include file [riff.h](riff.h) where the
WAV header is defined and [notes.h](notes.h) which is an associative array of
frequencies to note letters.

```fourier.cpp``` takes a WAV on stdin and prints the Fourier transform as
ASCII art.

```tonegen.cpp``` takes up to three frequencies in Hertz as parameters and
writes a WAV to stdout. This can be piped to a player such as ```aplay``` or to
```fourier``` for analysis.

```bash
# Generate major chord and send it to the speaker
./tonegen 440 550 660 | aplay

# Generate chord, analyse it and display the first 100 bins
./tonegen 440 550 660 | ./fourier | head -100
```
# C++ standards
Initially I used the default C++ standard for ```clang``` (C++03) and then
bumped the standard as I used newer features. I moved to C++11 to make use of
```auto``` and range-based ```for``` loops. And then C++14 for imaginary
constants and lambdas with ```auto``` types.

```bash
$ clang++ --version
clang version 3.8.0-2ubuntu4 (tags/RELEASE_380/final)
Target: x86_64-pc-linux-gnu
Thread model: posix
InstalledDir: /usr/bin
```

# Coding standard and linter
There's a make rule to run ```cppcheck```. And I periodically run
```clang-format``` with the default settings and apply the results. I'm not
sure where this fits in the workflow yet but I like that I don't have to think
about it. And it can be useful to swap in ```iwyu``` to weed out any left over
includes. But I don't really like the output so it's used purely as a guide.
```bash
make CC=iwyu
```

# Command line examples
```bash
# Analyse a series of frequencies
for freq in {200..1000..1}; do ./tonegen $freq | ./fourier | head -48; done

# Play C Major scale
for f in 261.6 293.7 329.6 349.2 392.0 440.0 493.9 523.3; do ./tonegen $f | aplay -q; done
```

The animated GIF was created with a bash script.
```bash
for freq in {1..200..5}; do ./tonegen $(( 50 + freq )) 100 $(( 350 - freq ))| ./fourier | head -48; done
```

The screen was captured using the window ID and converted to an animated GIF.
```bash
recordmydesktop
ffmpeg -i out.ogv -loop 0 fourier.gif
```
# See also
* [Fourier transform on Wikipedia](https://en.wikipedia.org/wiki/Fourier_transform#Example)
* [ANSI control codes](http://misc.flogisoft.com/bash/tip_colors_and_formatting)
* [Piano frequencies](https://en.wikipedia.org/wiki/Piano_key_frequencies)
* [Just intonation](https://en.wikipedia.org/wiki/Just_intonation)
