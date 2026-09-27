import customtkinter as ctk
import sqlite3
from datetime import datetime

RATES = {
    "RUB": 1,
    "USD": 90,
    "EUR": 98,
    "CNY": 12.5,
    "KZT": 0.18,
}

conn = sqlite3.connect('cur.db')
cursor = conn.cursor()

cursor.execute("""
    CREATE TABLE IF NOT EXISTS history (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        text TEXT,
        date TEXT
    )
""")
conn.commit()


def convert():
    text = amount_entry.get()
    text = text.replace(",", ".")

    try:
        amount = float(text)
    except ValueError:
        result_label.configure(text="Enter a number")
        return

    money_from = from_menu.get()
    money_to = to_menu.get()

    rubles = amount * RATES[money_from]
    result = rubles / RATES[money_to]

    line = f"{amount} {money_from} = {round(result, 2)} {money_to}"
    result_label.configure(text=line)

    date = datetime.now().strftime("%d.%m %H:%M")

    cursor.execute(
        "INSERT INTO history (text, date) VALUES (?, ?)",
        (line, date)
    )
    conn.commit()

    show_history()


def swap():
    a = from_menu.get()
    b = to_menu.get()

    from_menu.set(b)
    to_menu.set(a)


def show_history():
    cursor.execute(
        "SELECT date, text FROM history ORDER BY id DESC LIMIT 15"
    )
    rows = cursor.fetchall()

    history_box.delete("1.0", "end")

    for date, text in rows:
        history_box.insert("end", f"{date} {text}\n")


def clear_history():
    cursor.execute("DELETE FROM history")
    conn.commit()
    show_history()


ctk.set_appearance_mode("dark")

window = ctk.CTk()
window.title("Currency Converter")
window.geometry("420x520")

ctk.CTkLabel(
    window,
    text="Currency Converter",
    font=("Arial", 22, "bold")
).pack(pady=20)

amount_entry = ctk.CTkEntry(
    window,
    placeholder_text="Amount",
    font=("Arial", 18),
    height=45,
    justify="center"
)
amount_entry.pack(padx=40, fill="x")

row = ctk.CTkFrame(window, fg_color="transparent")
row.pack(pady=15)

from_menu = ctk.CTkOptionMenu(
    row,
    values=list(RATES),
    width=100
)
from_menu.set("USD")
from_menu.pack(side="left", padx=5)

to_menu = ctk.CTkOptionMenu(
    row,
    values=list(RATES),
    width=100
)
to_menu.set("RUB")
to_menu.pack(side="left", padx=5)

ctk.CTkButton(
    window,
    text="Convert",
    height=40,
    command=convert
).pack(padx=40, fill="x")

result_label = ctk.CTkLabel(
    window,
    text="Result: ?",
    font=("Arial", 18)
)
result_label.pack()

history_box = ctk.CTkTextbox(
    window,
    height=200
)
history_box.pack(
    pady=10,
    padx=20,
    fill="both",
    expand=True
)

clear_button = ctk.CTkButton(
    window,
    text="Clear History",
    command=clear_history
)
clear_button.pack(
    padx=20,
    pady=10,
    fill="x"
)

ctk.CTkButton(
    row,
    text="⇄",
    width=40,
    command=swap
).pack(side="left", padx=5)

show_history()

window.mainloop()
conn.close()
