# Software setup for the QTGMC deinterlacer:

## References:
[Youtube video](https://www.youtube.com/watch?v=S0rU_ZiC0hM)

[Arch forum thread](https://bbs.archlinux.org/viewtopic.php?id=297627)

deinterlacer script deinterlace.py:
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

ffmpeg commandline:
```
vspipe -c y4m deinterlace.py -|ffmpeg -hide_banner -colorspace 6 -color_primaries 6 -color_trc 6 -i pipe: -i interlaced.mkv -map 0:v -map 1:a -c:v libx264 -preset slow -crf 18 -tune film -c:a copy -c:s copy out.mkv
```
In the above, the output from vspipe is missing the YUV/RGB transformation matrix, the transformation characteristics, and the RGB/XYZ
matrix, which are needed for proper YUV->RGB conversion. This is fixed by manually setting these properties just before the input pipe.
Reference: [colorspace](https://trac.ffmpeg.org/wiki/colorspace)

[Markdown comment example]::
[ vim:set textwidth=140 spell:]::
