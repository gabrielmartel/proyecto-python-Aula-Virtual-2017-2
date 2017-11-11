# proyecto-python-musica-2017-2
# 
import sys
from aubio import source, notes

if len(sys.argv) < 2:
    print("Usage: %s <filename> [samplerate]" % sys.argv[0])
    sys.exit(1)

filename = sys.argv[1]

downsample = 1
samplerate = 44100 // downsample
if len( sys.argv ) > 2: samplerate = int(sys.argv[2])

win_s = 512 // downsample # fft size
hop_s = 256 // downsample # hop size

s = source(filename, samplerate, hop_s)
samplerate = s.samplerate

tolerance = 0.8

notes_o = notes("default", win_s, hop_s, samplerate)

print("%8s" % "time","[ start","vel","last ]")

# total number of frames read
total_frames = 0
while True:
    samples, read = s()
    new_note = notes_o(samples)
    if (new_note[0] != 0):
        note_str = ' '.join(["%.2f" % i for i in new_note])
        note_str = new_note[1]
        for i in new_note:
            if i==84:
                print("Do")
            if i==88:
                print("re")
            if i==91:
                print("mi")
            if i==97:
                print("fa")
            if i==101:
                print("Sol")
            if i==109:
                print("la")
            if i==110:
                print("si")
        #print(note_str)


        #print("%.6f" % (total_frames/float(samplerate)), new_note)
    total_frames += read
    if read < hop_s: break
