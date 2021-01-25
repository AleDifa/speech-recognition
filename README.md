# speech-recognition
This program recognize multiple audio track and convert in text with Google API

put the audio where Py. is running 
```python
myaudio = AudioSegment.from_wav("REMARKETING  EPISODIO 1 - Cosè il remarketing.wav" , "wav")
```

for work in audio more longher than 1 minute, we need to pass more little file, 
split the audio every 50 seconds,  pydub calculates in millisec 50000 = 50 seconds
```python
chunk_length_ms = 50000
chunks = make_chunks(myaudio, chunk_length_ms)
for i,chunk in enumerate(chunks):
    chunk_name = "traccia{0}.wav".format(i)
    print ("exporting", chunk_name)
    chunk.export(chunk_name, format="wav")
```
>exporting traccia0.wav <br>
>exporting traccia1.wav <br> 
>exporting traccia2.wav <br>

# Recognize text from audio with google API
create a istance of recognize speech
```python
r = speech_recognition.Recognizer()

traccia0 = speech_recognition.AudioFile("traccia0.wav")
traccia1 = speech_recognition.AudioFile("traccia1.wav")
traccia2 = speech_recognition.AudioFile("traccia2.wav")
```
calibrate the audio for adjust ambient noise levels
```python
with traccia0 as source:
    r.adjust_for_ambient_noise(source, duration=1)
    audio0 = r.record(source)
```
use G API to convert audio in text
```python
r.recognize_google(audio0, language="it-IT")
```
>"per la rubrica marketing di radio.it parliamo di remarketing con la più grande esperta in Italia che è Alessandra Maggio radio.it soluzioni idee novità per chi acquista >utilizza o vende tecnologia tecnologia e studio Ma Alessandra Che cos'è remarketing farò due tipi di...
```python
with traccia1 as source:
    audio1 = r.record(source)
r.recognize_google(audio1, language="it_IT")
```
>"rilasciano ho la mia pagina Facebook di lasciare mi serve per intercettare fare il pollo Apps utenti che in qualche modo Già hanno avuto un interazione con me il sollecito >questo perché Perché il percorso D'Acquisto oggi o comunque di conversione perché parliamo anche di regeneration....

```python
with traccia2 as source:
    audio2 = r.record(source)

r.recognize_google(audio2, language="it_IT")
```
>"pensiamo messaggio ripetuto nel tempo quindi le due caratteristiche importanti sono la frequenza Quindi quante volte vedo questo messaggio all'inte...

### More words are incorrect, a better way would be to work with software to reduce the level of background noise.
