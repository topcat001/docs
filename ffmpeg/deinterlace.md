# Software setup for the QTGMC deinterlacer:

## References:
[YouTube video](https://www.youtube.com/watch?v=S0rU_ZiC0hM)

[Arch forum thread](https://bbs.archlinux.org/viewtopic.php?id=297627)

## Instructions:
deinterlacer script `deinterlace.py`:
```
from vapoursynth import core
import havsfunc as haf
clip = core.ffms2.Source('/tmp/anindya/download/interlaced.mkv')

# Show clip properties.
#frame = clip.get_frame(0)
#clip = core.text.Text(clip, str(frame.props))
#print(frame.props)

clip = haf.QTGMC(clip, Preset='Slower', TFF=True)
clip = core.resize.Lanczos(clip, width=852, height=480)
clip.set_output()
```

`ffmpeg` command:
```
vspipe -c y4m deinterlace.py -|ffmpeg -hide_banner -colorspace 6 -color_primaries 6 -color_trc 6 -i pipe: -i interlaced.mkv -map 0:v -map 1:a -r '30000/1001' -c:v libx264 -preset slow -crf 18 -tune film -c:a copy -c:s copy out.mkv
```
In the above, the output from `vspipe` is missing the `YUV/RGB` transformation matrix, the transformation characteristics, and the `RGB/XYZ`
matrix, which are needed for proper `YUV->RGB` conversion. This is fixed by manually setting these properties just before the input pipe.
Reference: [`colorspace`](https://trac.ffmpeg.org/wiki/colorspace)

In addition, we need to set the output rate to 29.97 fps. `vspipe` outputs 59.97 progressive fps from the original 29.97 fps interlaced
input, and `ffmpeg` keeps that, resulting in duplicate frames. However, this does not increase file size because of `h264` compression.

## Keeping the output interlaced:
The following command only encodes to `h264`, preserving the original interlaced frames:
```
ffmpeg -i interlaced.mkv -c:v libx264 -preset slow -crf 18 -tune film -flags +ilme+ildct -alternate_scan 1 -c:a copy -c:s copy out.mkv
```

[Markdown comment example]::
[ vim:set textwidth=140 spell:]::
