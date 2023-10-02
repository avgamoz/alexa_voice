import speech_recognition as sr
import pyttsx3
import pywhatkit
import datetime
import wikipedia
import pyjokes

def talk(text):
    engine.say(text)
    engine.runAndWait()

def take_command():
    try:
        with sr.Microphone() as source:
            print('Listening...')
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            if 'alexa' in command:
                command = command.replace('alexa', '')
                print(command)
    except Exception as e:
        print(e)
        return ""

    return command

def play_song(song):
    talk('Okay boss, trying to play your favorite song ' + song)
    print('Searching for your song: ' + song)
    pywhatkit.playonyt(song)

def get_current_time():
    time = datetime.datetime.now().strftime('%I:%M %p')
    print(time)
    talk('Current time is ' + time)

def get_wikipedia_info(person):
    try:
        info = wikipedia.summary(person, 5)
        print(info)
        talk(info)
    except wikipedia.exceptions.DisambiguationError as e:
        print(e)
        talk('There are multiple matches for that query. Please specify.')

def tell_joke():
    joke = pyjokes.get_joke()
    talk(joke)

listener = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)

while True:
    command = take_command()
    print(command)

    if 'play' in command:
        song = command.replace('play', '').strip()
        play_song(song)

    elif 'time' in command:
        get_current_time()

    elif 'who is' in command:
        person = command.replace('who is', '').strip()
        get_wikipedia_info(person)

    elif 'joke' in command:
        tell_joke()

    else:
        talk('Say that command again, please.')