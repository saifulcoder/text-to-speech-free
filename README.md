# Text-to-Speech Demo

This is a simple web-based application that demonstrates how to use the Web Speech API's Text-to-Speech (TTS) feature. It allows users to input text and listen to it being spoken using different available voices in the browser.

## Features

- Input text for speech synthesis.
- Select from available system voices.
- Listen to the entered text using the selected voice.

## How to Use

1. Open the HTML file in a web browser that supports the Web Speech API.
2. Enter the text you want to be spoken in the textarea provided.
3. Choose a voice from the dropdown selection. The list is populated with all available voices on your system.
4. Click the "Listen" button to hear the text spoken in the selected voice.

## HTML Structure

The page consists of:
- A `<textarea>` element for entering the text to be spoken.
- A `<select>` dropdown for choosing a voice.
- A button labeled "Listen" to trigger the speech synthesis.

## JavaScript Logic

1. **populateVoiceList**: This function populates the dropdown with available voices from `speechSynthesis.getVoices()`.
   
   It updates the list every time voices are loaded or changed (`speechSynthesis.onvoiceschanged`).

2. **speakText**: This function retrieves the text from the textarea and the selected voice. It then uses the `SpeechSynthesisUtterance` API to generate speech and play it.

## Browser Support

Ensure that your browser supports the [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis), which is currently supported in most modern browsers like:
- Google Chrome
- Microsoft Edge
- Safari

## Example Usage

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text-to-Speech</title>
</head>
<body>
    <h1>Text-to-Speech Demo</h1>
    <textarea id="text-to-speech-input" rows="4" cols="50">Enter the text you want to speak here...</textarea><br>
    <select id="voice-selection"></select><br>
    <button onclick="speakText()">Listen</button>

    <script>
        function populateVoiceList() {
            var voices = window.speechSynthesis.getVoices();
            var select = document.getElementById('voice-selection');
            select.innerHTML = '';
            voices.forEach(function(voice) {
                var option = document.createElement('option');
                option.textContent = voice.name + ' (' + voice.lang + ')';
                option.setAttribute('data-lang', voice.lang);
                option.setAttribute('data-name', voice.name);
                option.value = voice.name;
                select.appendChild(option);
            });
        }

        populateVoiceList();
        if (speechSynthesis.onvoiceschanged !== undefined) {
            speechSynthesis.onvoiceschanged = populateVoiceList;
        }

        function speakText() {
            var text = document.getElementById('text-to-speech-input').value;
            var selectedVoiceName = document.getElementById('voice-selection').value;
            var voices = window.speechSynthesis.getVoices();
            var utterance = new SpeechSynthesisUtterance(text);
            for (var i = 0; i < voices.length; i++) {
                if (voices[i].name === selectedVoiceName) {
                    utterance.voice = voices[i];
                    break;
                }
            }
            window.speechSynthesis.speak(utterance);
        }
    </script>
</body>
</html>
```
