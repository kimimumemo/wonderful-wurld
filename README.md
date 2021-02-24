# wonderful-wurld
#!/usr/bin/env python
# coding: utf-8

# Importing Pyo
# In[52]:
from pyo import *


# Starting the pyo server
# In[53]:
s = Server(audio='pa', duplex=0)
s.setMidiInputDevice(2)
s.boot()
s.start()


# Configuring our beat
# In[54]:
##signal shape(in this case, a square wave)
wav = SquareTable()

##beat timing
#beat = Metro(time=1, poly=1).play()
beat = Metro(time=0.25, poly=16).play()

##amplitude envelope shape
envelope = CurveTable([(0,0), (100,1), (450,.3), (8191,0)])
#envelope = CurveTable([(0,0), (2048,5), (4096,.2), (6164,0.5)], 0, 20)

##amplitude
amplitude = TrigEnv(beat, table=envelope, dur=.25, mul=.7)

##random notes
pitch = TrigXnoiseMidi(beat, dist=3, scale=5, mrange=(84,54))


# Trigetting an oscillator
# In[55]:
oscillator = Osc(table=wav, freq=pitch, mul=amplitude).out()


# We ❤️ 🎹
# In[56]:
##synth signal shape
sig = SawTable(order=12).normalize()
#sig1 = LinTable([(0,20), (200,5), (1000,2), (8191,1)])

##tempo
metro_synth = Metro(time=0.125, poly=2).play()

##LFO Filter
lfo = LFO(freq=2.2, sharp=.2, type=4, mul=110, add=220)

##synth envelope
envelope_synth = TrigEnv(metro_synth, table=sig, dur=0.5)


# In[57]:
synth = FM(carrier=[220.5,220], ratio=[.298,.2503], index=envelope_synth, mul=0.2).out()


# Who Doesn't Like The 80's?
# In[58]:
lfd = Sine([.4,.2], mul=.2, add=.5)
synth_80 = SuperSaw(freq=450, detune=lfd, bal=.13, mul=.1).out()


# Put em' some lyrics!

# In[59]:
s = Server(audio='pa', duplex=0)
s.setMidiInputDevice(2)
s.boot()
s.start()
sf = SfPlayer("/Users/Kimberley/Desktop/Musik/alive.wav", speed=1, loop=False).out()


# In[60]:
oscillator.stop()
synth.stop()
synth_80.stop()


# In[50]:
sf.stop()
