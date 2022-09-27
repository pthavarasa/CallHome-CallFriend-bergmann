# CallHome-CallFriend-bergmann
Call Home, Call Friend, Bergmann datasets annotations as Rich Transcription Time Marked (RTTM) for speaker diarization purpose.

### Details
- length = 924
- training set = 646
- testing set = 278
- Regex = `\*(\S+):[^\*]*?(\d*)_(\d*)(?:[^\*]*?(\d*)_(\d*))*`

### Audio Directory
audio url format is `dataUrl[name]+id+".wav"` except ***Bergmann*** and ***CallFriendSpanish***
```json
dataUrl = {
  "CallFriendFrench": "https://media.talkbank.org/ca/CallFriend/fra-q/0wav/",
  "CallFriendJapanese" : "https://media.talkbank.org/ca/CallFriend/jpn/0wav/",
  "CallFriendNEnglish" : "https://media.talkbank.org/ca/CallFriend/eng-n/0wav/",
  "CallFriendSEnglish" : "https://media.talkbank.org/ca/CallFriend/eng-s/0wav/",
  "CallFriendSpanish" : "https://media.talkbank.org/ca/CallFriend/spa/0wav/",
  "CallHomeChinese" : "https://media.talkbank.org/ca/CallHome/zho/0wav/",
  "CallHomeEnglish" : "https://media.talkbank.org/ca/CallHome/eng/0wav/",
  "CallHomeGerman" : "https://media.talkbank.org/ca/CallHome/deu/0wav/",
  "CallHomeJapanese" : "https://media.talkbank.org/ca/CallHome/jpn/0wav/",
  "CallHomeSpanish" : "https://media.talkbank.org/ca/CallHome/spa/0wav/",
  "Bergmann" : "https://media.talkbank.org/ca/Bergmann/0wav/"
}
```
### Note
* It containe noise like : https://sla.talkbank.org/TBB/ca/CallHome/spa/1341.cha
```
ca/CallHome/spa/1341.cha
ca/CallHome/spa/0981.cha
ca/CallHome/spa/0836.cha
ca/CallHome/spa/0992.cha
ca/CallHome/spa/1062.cha
ca/CallHome/spa/1379.cha
ca/CallHome/spa/0997.cha
ca/CallHome/spa/1140.cha
ca/CallHome/spa/1058.cha
ca/CallHome/spa/1333.cha
ca/CallHome/spa/1552.cha
ca/CallHome/spa/1849.cha
ca/CallHome/spa/2054.cha
ca/CallHome/spa/2057.cha
ca/CallHome/spa/2109.cha
ca/CallHome/spa/2144.cha
```
* some audios are not annotated start to end 
### Download audio
```python3
import bs4
import requests
from pathlib import Path

def dowload_audio_from_apache_server(url, audioIDs, dest):
    Path(dest).mkdir(parents=True, exist_ok=True)
    r = requests.get(url)
    data = bs4.BeautifulSoup(r.text, "html.parser")
    for l in data.find_all("a")[5:]:
        print(url + l["href"])
        if l["href"].split(".")[0] in audioIDs:
            r = requests.get(url + l["href"])
            with open(dest + "/" + l["href"], 'wb') as f:
                f.write(r.content)
                
dowload_audio_from_apache_server("https://media.talkbank.org/ca/CallHome/zho/0wav/", ["0695", "0718"], "audio")
```
exception : Bergmann and CallFriendSpanish
```python3
def getCallFriendSpanish(url, id):
  r = requests.get(url)
  data = bs4.BeautifulSoup(r.text, "html.parser")
  for l in data.find_all("a")[5:]:
    if id in l["href"]:
      return url + l["href"]
      
def getBergman(url, id):
  r = requests.get(url)
  data = bs4.BeautifulSoup(r.text, "html.parser")
  for l in data.find_all("a")[5:]:
    if l["href"].split("_")[0] == id:
      return url + l["href"]
```
