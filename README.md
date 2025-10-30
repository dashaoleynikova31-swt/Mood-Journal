# Mood Journal 
A simple Python program to track your daily moods.
## Features:
- Add your current mood
- View all recorded moods
- Exit the program
- Provide feedback at the end

print("\nMood Journal\n")

moods = []

while True:  # Display menu options 
    print("1 - Add the mood")
    print("2 - Show all moods")
    print("3 - Exit")

    choice = input("Choose the option: ")

    if choice == "1":
        mood = input("Enter your mood: ")
        moods.append(mood)
        print("Mood saved!\n")

    elif choice == "2":
        if moods:
            print(f"Here are all your moods today: {moods}\n")
        else:
            print("You haven't added any moods yet.\n")

    elif choice == "3":
        print("Bye!")
        break

    else:
        print("Please, enter the correct number!\n")

test = input("Did you like this Mood Journal? ")
if test.lower() in ["yes", "sure", "y", "yeah"]:
    print("You're welcome!")
else:
    print("OK! No problem! I'll practice much better!")

