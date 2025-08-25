print("Mood Journal")

moods = []

while True:
    print("1 - Add the mood: ")
    print("2 - Show all moods")
    print("3 - Exit")
    
    choise = input("Choise the option: ")
    if choise == "1":
        mood = input("Enter your mood: ")
        moods.append(mood)
        print("Mood saved!")
    elif choise == "2":
        print(f"Here are all your moods today: {moods}")
    elif choise == "3":
        print("Bye!")
        break
    else:
        print("Please, enter the correct number!")

test = input("Did you like this Mood Journal?")
if test.lower() == "yes":
    print("You're welcome!")
else:
    print("OK! No problem! I'll to practice much better!")

# Mood Journal 📝💖

A simple Python program to track your daily moods.

## Features:
- Add your current mood
- View all recorded moods
- Exit the program
- Provide feedback at the end

## How to use:
1. Run the program.
2. Choose an option: add mood, show all moods, or exit.
3. Your moods are saved in a list and displayed whenever you want.
4. Give feedback at the end to make the program even more fun!

Feel free to fork, improve, and share your Mood Journal!