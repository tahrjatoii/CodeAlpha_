import random

# ─── Hangman stages ──
HANGMAN_STAGES = [
    # 0 wrong guesses
    """
       -----
       |   |
           |
           |
           |
           |
    =========
    """,
    # 1 wrong guess
    """
       -----
       |   |
       O   |
           |
           |
           |
    =========
    """,
    # 2 wrong guesses
    """
       -----
       |   |
       O   |
       |   |
           |
           |
    =========
    """,
    # 3 wrong guesses
    """
       -----
       |   |
       O   |
      /|   |
           |
           |
    =========
    """,
    # 4 wrong guesses
    """
       -----
       |   |
       O   |
      /|\\  |
           |
           |
    =========
    """,
    # 5 wrong guesses
    """
       -----
       |   |
       O   |
      /|\\  |
      /    |
           |
    =========
    """,
    # 6 wrong guesses — game over
    """
       -----
       |   |
       O   |
      /|\\  |
      / \\  |
           |
    =========
    """,
]

# ─── Word list ───────────────────────────────────────────────────────────────
WORDS = ["python", "rocket", "jungle", "puzzle", "bridge"]

# ─── Helper: display current word progress ───────────────────────────────────
def display_word(word, guessed_letters):
    return " ".join(letter if letter in guessed_letters else "_" for letter in word)

# ─── Main game logic ─────────────────────────────────────────────────────────
def play_hangman():
    secret_word   = random.choice(WORDS)
    guessed       = set()          # all guessed letters (correct + wrong)
    wrong_letters = []             # only the incorrect ones
    max_wrong     = 6

    print("\n" + "=" * 40)
    print("       Welcome to HANGMAN!")
    print("=" * 40)
    print(f"The word has {len(secret_word)} letters. Good luck!\n")

    while True:
        # ── Draw gallows ──────────────────────────────────────────────────
        print(HANGMAN_STAGES[len(wrong_letters)])

        # ── Show word progress ────────────────────────────────────────────
        progress = display_word(secret_word, guessed)
        print(f"  Word   : {progress}")
        print(f"  Wrong  : {', '.join(wrong_letters) if wrong_letters else 'None'}")
        print(f"  Remaining guesses: {max_wrong - len(wrong_letters)}\n")

        # ── Check win ─────────────────────────────────────────────────────
        if "_" not in progress:
            print("  Congratulations! You guessed the word:", secret_word.upper())
            break

        # ── Check lose ────────────────────────────────────────────────────
        if len(wrong_letters) >= max_wrong:
            print(f"  Game over! The word was: {secret_word.upper()}")
            break

        # ── Get player input ──────────────────────────────────────────────
        guess = input("  Guess a letter: ").strip().lower()

        if len(guess) != 1 or not guess.isalpha():
            print("    Please enter a single letter.\n")
            continue

        if guess in guessed:
            print(f"    You already guessed '{guess}'. Try a different letter.\n")
            continue

        guessed.add(guess)

        if guess in secret_word:
            print(f"    Nice! '{guess}' is in the word.\n")
        else:
            wrong_letters.append(guess)
            print(f"   '{guess}' is not in the word.\n")

    # ── Play again? ───────────────────────────────────────────────────────────
    again = input("\nPlay again? (y/n): ").strip().lower()
    if again == "y":
        play_hangman()
    else:
        print("\nThanks for playing! Goodbye \n")

# ─── Entry point ─────────────────────────────────────────────────────────────
if __name__ == "__main__":
    play_hangman()
