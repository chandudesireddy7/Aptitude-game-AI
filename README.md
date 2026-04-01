import random
import sys

# =====================================================
# Utility Functions
# =====================================================

def clear_line():
    print("-" * 50)

def exit_game():
    print("\n👋 Thank you for practicing. Keep improving!")
    sys.exit()

def get_user_choice(valid_choices):
    """Safely get user input"""
    while True:
        choice = input("👉 Enter choice: ").strip().upper()
        if choice == "Q":
            exit_game()
        if choice in valid_choices:
            return choice
        print("⚠️ Invalid input. Try again or press Q to quit.")

# =====================================================
# Question Bank (10 per major topic)
# =====================================================

question_bank = {

# ================= QUANTITATIVE =================

"Percentages": [
{
"question": "What is 25% of 200?",
"options": ["A) 25","B) 50","C) 75","D) 100"],
"answer": "B",
"explanation": "25% = 25/100. (25/100)×200 = 50.",
"wrong": {
"A": "Too small. That's 12.5%.",
"C": "Incorrect multiplication.",
"D": "That would be 50%."
}
},
{
"question": "60 is what percent of 240?",
"options": ["A) 15%","B) 20%","C) 25%","D) 30%"],
"answer": "C",
"explanation": "(60/240)×100 = 25%.",
"wrong": {
"A": "Check the fraction again.",
"B": "Calculation error.",
"D": "Too large."
}
},
{
"question": "Increase 400 by 10%.",
"options": ["A) 420","B) 440","C) 410","D) 450"],
"answer": "B",
"explanation": "10% of 400 = 40 → 400+40=440.",
"wrong": {
"A": "Added only 5%.",
"C": "Too small.",
"D": "Too large."
}
},
]

# ================= LOGICAL =================
,
"Series": [
{
"question": "Find next: 2, 4, 8, 16, ?",
"options": ["A) 24","B) 32","C) 30","D) 18"],
"answer": "B",
"explanation": "Each number doubles → 16×2=32.",
"wrong": {
"A": "Pattern is doubling.",
"C": "Incorrect pattern.",
"D": "Too small."
}
},
{
"question": "Find next: 3, 6, 12, 24, ?",
"options": ["A) 36","B) 48","C) 30","D) 60"],
"answer": "B",
"explanation": "Multiply by 2 each time.",
"wrong": {
"A": "Not consistent.",
"C": "Incorrect.",
"D": "Too high."
}
},
]

# ================= VERBAL =================
,
"Synonyms": [
{
"question": "Synonym of 'Rapid'?",
"options": ["A) Slow","B) Quick","C) Weak","D) Late"],
"answer": "B",
"explanation": "Rapid means fast or quick.",
"wrong": {
"A": "Opposite meaning.",
"C": "Unrelated.",
"D": "Unrelated."
}
},
{
"question": "Synonym of 'Begin'?",
"options": ["A) End","B) Start","C) Close","D) Finish"],
"answer": "B",
"explanation": "Begin = Start.",
"wrong": {
"A": "Opposite.",
"C": "Different meaning.",
"D": "Opposite."
}
},
]

# ================= DATA INTERPRETATION =================
,
"Data Interpretation": [
{
"question": "Sales increased from 200 to 260. % increase?",
"options": ["A) 20%","B) 25%","C) 30%","D) 35%"],
"answer": "C",
"explanation": "Increase=60 → (60/200)×100=30%.",
"wrong": {
"A": "Too low.",
"B": "Calculation error.",
"D": "Too high."
}
},
{
"question": "Average of 10, 20, 30?",
"options": ["A) 15","B) 20","C) 25","D) 30"],
"answer": "B",
"explanation": "(10+20+30)/3 = 20.",
"wrong": {
"A": "Too low.",
"C": "Too high.",
"D": "Incorrect."
}
},
],
}

# =====================================================
# Difficulty Adjustment
# =====================================================

def filter_by_difficulty(topic_questions, difficulty):
    """
    Easy → first few
    Medium → middle
    Hard → all questions
    """
    if difficulty == "Easy":
        return topic_questions[:min(10, len(topic_questions))]
    elif difficulty == "Medium":
        return topic_questions[:min(10, len(topic_questions))]
    else:
        return topic_questions

# =====================================================
# Quiz Engine
# =====================================================

def ask_question(q, score, total):
    clear_line()
    print("❓", q["question"])
    for opt in q["options"]:
        print(opt)

    print("\nEnter A/B/C/D | B = Back | Q = Quit")

    while True:
        choice = input("👉 Your answer: ").upper()

        if choice == "Q":
            exit_game()
        if choice == "B":
            return score, "BACK"
        if choice in ["A","B","C","D"]:
            break
        print("⚠️ Invalid input.")

    if choice == q["answer"]:
        print("\n✅ Correct!")
        print("📘 Explanation:", q["explanation"])
        score += 1
    else:
        print("\n❌ Incorrect.")
        print("✔ Correct Answer:", q["answer"])
        print("📘 Explanation:", q["explanation"])
        print("⚠️ Why your choice is wrong:",
              q["wrong"].get(choice, "Check concept."))

    print(f"\n📊 Score: {score}/{total}")
    input("\nPress Enter to continue...")
    return score, "CONTINUE"

def start_quiz(topic, difficulty):
    questions = filter_by_difficulty(question_bank[topic], difficulty)
    random.shuffle(questions)

    asked = set()
    score = 0
    total = len(questions)

    for idx, q in enumerate(questions):
        if idx in asked:
            continue
        asked.add(idx)

        score, status = ask_question(q, score, total)
        if status == "BACK":
            return

    clear_line()
    print("🎉 Topic Completed!")
    print(f"🏆 Final Score: {score}/{total}")
    input("\nPress Enter to return to topics...")

# =====================================================
# Topic Menu
# =====================================================

def topic_menu(difficulty):
    topics = list(question_bank.keys())

    while True:
        clear_line()
        print(f"📚 Select Topic | Difficulty: {difficulty}")
        for i, t in enumerate(topics, 1):
            print(f"{i}. {t}")
        print("B. Back to Difficulty")
        print("Q. Quit")

        choice = input("👉 Enter choice: ").upper()

        if choice == "Q":
            exit_game()
        if choice == "B":
            return
        if choice.isdigit() and 1 <= int(choice) <= len(topics):
            start_quiz(topics[int(choice)-1], difficulty)
        else:
            print("⚠️ Invalid choice.")

# =====================================================
# Main Menu
# =====================================================

def main():
    while True:
        clear_line()
        print("🧠 APTITUDE PREPARATION GAME")
        print("1. Easy")
        print("2. Medium")
        print("3. Hard")
        print("Q. Exit")

        choice = input("👉 Select difficulty: ").upper()

        if choice == "Q":
            exit_game()
        elif choice == "1":
            topic_menu("Easy")
        elif choice == "2":
            topic_menu("Medium")
        elif choice == "3":
            topic_menu("Hard")
        else:
            print("⚠️ Invalid choice.")

# =====================================================
# Start Program
# =====================================================

if __name__ == "__main__":
    main()

