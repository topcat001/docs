# Software setup for the QTGMC deinterlacer:

## References:
[Youtube video](https://www.youtube.com/watch?v=S0rU_ZiC0hM)

[Arch forum thread](https://bbs.archlinux.org/viewtopic.php?id=297627)

deinterlacer script deinterlace.py:
```
from vapoursynth import core
import havsfunc as haf
clip = core.ffms2.Source('/tmp/anindya/download/interlaced.mkv')
clip = haf.QTGMC(clip, Preset='Slower', TFF=True)
clip = core.resize.Lanczos(clip, width=852, height=480)
clip.set_output()
```

ffmpeg commandline:
```
vspipe -c y4m deinterlace.py -|ffmpeg -hide_banner -i pipe: -i interlaced.mkv -map 0:v -map 1:a -c:v libx264 -preset slow -crf 18 -tune film -c:a copy -c:s copy out.mkv
```

[Markdown comment example]::
[ vim:set textwidth=140 spell:]::
