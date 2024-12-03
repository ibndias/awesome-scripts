# How to Scrap Youtube Captions/Subtitle as Dataset

```python
import google.generativeai as genai
import os

def rewrite_youtube_transcript(text):
    genai.configure(api_key=os.environ["GOOGLE_AI_API_KEY"])
    model = genai.GenerativeModel("gemini-1.5-flash")
    response = model.generate_content("Rewrite the following raw youtube transcript with correct structure: " + text)
    return response.text

# For each video in the playlist, get the transcript and rewrite it and save it to a file with the same name as the video id
with open("playlist.txt") as f:
    for line in f:
        video_id = line.strip()
        transcript = YouTubeTranscriptApi.get_transcript(video_id)
        text = ""
        for i in transcript:
            text += ' ' + i['text']
        rewritten_text = rewrite_youtube_transcript(text)
        with open(video_id + ".txt", "w") as f:
            f.write(rewritten_text)
```
