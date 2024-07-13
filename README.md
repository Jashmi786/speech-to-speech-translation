
import speech_recognition as sr
from googletrans import Translator

def get_language_code(language_name):
    # Dictionary mapping some common language names to language codes
    language_codes = {
        'english': 'en',
        'spanish': 'es',
        'french': 'fr',
        'german': 'de',
        'italian': 'it',
        'chinese': 'zh',
        'hindi': 'hi',
        'japanese': 'ja',
        'korean': 'ko',
        'portuguese': 'pt',
        'russian': 'ru',
        'bengali': 'bn',
        'marathi': 'mr',
        'gujarati': 'gu',
        'kannada': 'kn'
    }
    # Return the language code if found, otherwise return None
    return language_codes.get(language_name.lower())

def transcribe_and_translate_realtime(source_language='en', target_language='en'):
    recognizer = sr.Recognizer()
    microphone = sr.Microphone()

    # Adjust for ambient noise levels
    with microphone as source:
        recognizer.adjust_for_ambient_noise(source)

    print("Listening...")

    try:
        while True:
            with microphone as source:
                audio_data = recognizer.listen(source)

            try:
                # Recognize speech using Google Web Speech API
                text = recognizer.recognize_google(audio_data, language=source_language)
                print(f"Recognized speech: {text}")

                # Translate the recognized text to the target language
                translator = Translator()
                translation = translator.translate(text, src=source_language, dest=target_language)
                print(f"Translated text: {translation.text}")

            except sr.UnknownValueError:
                print("Google Web Speech API could not understand the audio.")

            except sr.RequestError as e:
                print(f"Could not request results from Google Web Speech API; {e}")

    except KeyboardInterrupt:
        print("Stopped listening.")

if __name__ == '__main__':
    print("Welcome to Real-Time Speech Translation!")
    
    # Prompt user for source language
    while True:
        source_lang_name = input("Enter the source language (e.g., English, Spanish, French,German, Italian, Chinese, Hindi, Japenese, Korean, Portuguese, Russian, Bengali, Marathi, Gujarati, Kannada): ").strip().lower()
        source_lang_code = get_language_code(source_lang_name)
        
        if source_lang_code:
            break
        else:
            print("Language not recognized. Please try again.")

    # Run real-time transcription and translation
    transcribe_and_translate_realtime(source_language=source_lang_code)
