# Mood Journal - a simple program to track your daily moods

# Create an empty list to store moods
moods = []

# Start an infinite loop to show the menu until the user exits
while True:
    # Display menu options
    print("1 - Add the mood")
    print("2 - Show all moods")
    print("3 - Exit")
    
    # Ask the user to choose an option
    choice = input("Choose the option: ")
    
    if choice == "1":
        # Add a new mood to the list
        mood = input("Enter your mood: ")
        moods.append(mood)
        print("Mood saved!")
    elif choice == "2":
        # Show all recorded moods
        print(f"Here are all your moods today: {moods}")
    elif choice == "3":
        # Exit the program
        print("Bye!")
        break
    else:
        # Handle invalid menu option
        print("Please, enter the correct number!")

# Ask for feedback after exiting the main menu
test = input("Did you like this Mood Journal? ")

# Respond based on user's feedback
if test.lower() == "yes":
    print("You're welcome!")
else:
    print("OK! No problem! I'll practice much better!")

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
