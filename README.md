import secrets
import string
import tkinter as tk
from tkinter import Label, Entry, Button, Text, Scrollbar, END

def generate_password(length=12):
    characters = string.ascii_letters + string.digits + string.punctuation
    password = ''.join(secrets.choice(characters) for _ in range(length))
    return password

def generate_multiple_passwords(num_passwords=5, length=12):
    passwords = [generate_password(length) for _ in range(num_passwords)]
    return passwords

def generate_passwords_gui():
    window = tk.Tk()
    window.title("Password Generator")

    Label(window, text="Password Length:").grid(row=0, column=0, padx=10, pady=10)
    length_entry = Entry(window)
    length_entry.grid(row=0, column=1, padx=10, pady=10)

    Label(window, text="Number of Passwords:").grid(row=1, column=0, padx=10, pady=10)
    num_passwords_entry = Entry(window)
    num_passwords_entry.grid(row=1, column=1, padx=10, pady=10)

    result_text = Text(window, height=8, width=40)
    result_text.grid(row=2, column=0, columnspan=2, padx=10, pady=10)
    
    scrollbar = Scrollbar(window, command=result_text.yview)
    scrollbar.grid(row=2, column=2, sticky="nsew")
    result_text.config(yscrollcommand=scrollbar.set)

    def generate_passwords():
        try:
            length = int(length_entry.get())
            num_passwords = int(num_passwords_entry.get())
        except ValueError:
            result_text.delete(1.0, END)
            result_text.insert(END, "Please enter valid numbers.")
            return

        passwords = generate_multiple_passwords(num_passwords, length)

        result_text.delete(1.0, END)
        for i, password in enumerate(passwords, start=1):
            result_text.insert(END, f"Password {i}: {password}\n")

    generate_button = Button(window, text="Generate Passwords", command=generate_passwords)
    generate_button.grid(row=3, column=0, columnspan=2, pady=10)

    window.mainloop()

if __name__ == "__main__":
    generate_passwords_gui()
