# Number-Guess
!! Number guess !! 


import random
import tkinter as tk
from tkinter import messagebox

class NumberGuessGame:
    def __init__(self, root):
        self.root = root
        self.root.title("🎯 Number Guessing Game 🎯 - Rishi's Game")
        self.root.geometry("420x320")
        self.root.configure(bg="#1e1e2e")
        
        self.number = random.randint(1, 20)
        self.attempts = 0
        self.max_attempts = 5

        self.label = tk.Label(root, text="Guess a number between 1 and 20!", font=("Arial", 14, "bold"), bg="#1e1e2e", fg="#ffcc00")
        self.label.pack(pady=10)
        self.corner_label = tk.Label(root, text="Rishi's Game", font=("Arial", 10, "italic"), bg="#1e1e2e", fg="#ffffff", anchor='e')
        self.corner_label.pack(fill='both', padx=10, pady=2)

        self.entry = tk.Entry(root, font=("Arial", 14))
        self.entry.pack(pady=5)

        self.button = tk.Button(root, text="Guess", font=("Arial", 12, "bold"), command=self.check_guess, bg="#ff5733", fg="white", relief="raised", bd=3)
        self.button.pack(pady=10)

        self.result_label = tk.Label(root, text="", font=("Arial", 12), bg="#1e1e2e", fg="white")
        self.result_label.pack(pady=5)
        self.timer_label = tk.Label(root, text="Time left: 30s", font=("Arial", 12, "bold"), bg="#1e1e2e", fg="yellow")
        self.timer_label.pack(pady=5)
        self.time_left = 30
        self.update_timer()

    def check_guess(self):
        try:
            guess = int(self.entry.get())
            self.attempts += 1
            
            if self.attempts >= self.max_attempts:
                messagebox.showerror("Game Over", f"❌ You've used all {self.max_attempts} attempts! The correct number was {self.number}.")
                self.reset_game()
                return
            
            if guess < self.number:
                self.result_label.config(text=f"📉 Too Low! Attempts left: {self.max_attempts - self.attempts}", fg="blue")
            elif guess > self.number:
                self.result_label.config(text=f"📈 Too High! Attempts left: {self.max_attempts - self.attempts}", fg="red")
            else:
                messagebox.showinfo("Congratulations!", f"🎉 You guessed it right in {self.attempts} attempts! 🎯")
                self.reset_game()
        except ValueError:
            self.result_label.config(text="⚠️ Enter a valid number!", fg="black")
    
    def update_timer(self):
        if self.time_left > 0:
            self.time_left -= 1
            self.timer_label.config(text=f"Time left: {self.time_left}s")
            self.root.after(1000, self.update_timer)
        else:
            messagebox.showerror("Time Up!", "⏳ Time is up! Restarting game.")
            self.reset_game()

    def reset_game(self):
        self.number = random.randint(1, 20)
        self.attempts = 0
        self.entry.delete(0, tk.END)
        self.result_label.config(text="")

if __name__ == "__main__":
    root = tk.Tk()
    game = NumberGuessGame(root)
    root.mainloop()
