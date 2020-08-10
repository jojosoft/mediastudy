# mediastudy - a complete workflow for language learning through media

A complete pipeline to digest the masses of audio-visual media needed to gain proficiency in a foreign language more quickly.
In most cases, you will want to split up a series of videos (with badly timed subtitles in the foreign language you are interested in) into one [Anki](https://apps.ankiweb.net/) flashcard per spoken sentence, containing both the audio snippet, the sentence text and a still image.
In parallel, you will need one audio file per video that is pleasant to listen to and is therefore fit to be played in the background throughout your day.

## Features

- Bulk processing of series with any amount of episodes or generally split-up media
- Audio extraction for background listening in two versions, full (all audio except selected chapters or start and end trim times) and condensed (audio with speech—detected using subtitles—only within regions metioned above)
  - Simple, interactive selection of desired chapters and audio and subtitle streams
  - Adjustable padding per sentence for condensed audio for a less chopped up result
  - Dynamic audio normalization for better audibility (and a less annoying experience)
  - Automatic ID3 tagging of audio files for clean import into audio collections
- Anki deck creation from media files and subtitles (using [substudy by emk](https://github.com/emk/subtitles-rs/tree/master/substudy))
  - Automatic subtitle extraction from media file or import of external subtitle file
  - Linear, static time shift of subtitles not from within (or nearby) media file
  - Automatic alignment of mistimed subtitles to others [using alass by kaegi](https://github.com/kaegi/alass)
  - Custom field output for seamless Anki import with correct mapping on card type
  - Auto-import into Anki and audio file move e.g. into a cloud-synchronized folder
- Subtitle preprocessing (validation and repair) to ensure substudy will parse
- Clear intermediate folder structure and efficient file management using hardlinks if possible, makes previewing the final subtitles and debugging the process easier

## Requirements

This is a **Bash** script that is primarily developed for running on Debian Buster.
It will probably also run fine on most other modern Linux distributions, but please keep in mind that it was not tested with a wide variety of them.
It needs ffmpeg (and ffprobe), [substudy](https://github.com/emk/subtitles-rs/tree/master/substudy) and [alass](https://github.com/kaegi/alass) installed on the system.
As a quick solution, put the executables for substudy and alass into your /usr/bin/ directory and make them executable.
Other necessary (more standard) software: awk, sed, sort, uniq, tac, tar, and md5sum.
Install pigz for more performance while archiving.
And of course, install Anki if you want to use the automatic import feature.

## Usage

First of all, open the script in a text editor, read the help text and look at the configuration variables.
At least change the `ANKI_FIELD_ORDER` to match your Anki card type, adjust the `ANKI_MEDIA_PATH` to where your Anki collection is located and point `FULL_AUDIO_PATH` and `CONDENSED_AUDIO_PATH` to where each type of audio should be put.
(Since we are on Linux, you could do cool stuff like mounting your audio folders and Anki collection via SSHFS, which will put the results elsewhere after the processing was done on a machine that is more fit for the task hardware-wise.)

Navigate to a folder with all the video files you consider being part of one piece of media.
For example, the following command will extract condensed audio, copy it to the right location, generate one big Anki deck and import it into Anki:

`mediastudy -c -a -d -e`

To also get full audio in addition to everything stated above, you can just execute:

`mediastudy`

All necessary information that is not supplied as a parameter will be asked for interactively.
Once you specify at least one "phase switch", mediastudy will only execute explicitly requested phases.
This way, you can easily repeat phases or only execute some and then run the remaining ones later.

For a full explanation of all features offered by mediastudy, please consult the help text at the beginning of the script or by running `mediastudy --help`.

## Important

I developed this script to make my own language learning easier and more pleasant, and have been pretty happy with it over the last months.
However, due to the broad range of suckage available in the (subtitle) wild, there might be some situations in which mediastudy will not yield the expected results.
Please consider creating an issue or—if you do not usually run away screaming at the first sight of Bash code—a pull request.
If mediastudy eased your language learning experience or you want me to help you with a specific problem more quickly, consider [donating as much as you feel I deserve](https://www.paypal.me/jojosoft/5)!
I am also planning on creating a Docker container which would make mediastudy available on almost all operating systems.
Your support will be essential for this, since I am personally completely happy on Linux.
