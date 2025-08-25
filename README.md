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
if test.lower() = "yes":
    print("You're welcome!")
else:
    print("OK! No problem! I'll to practice much better!")
