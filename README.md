# Currency Converter

A simple desktop currency converter built with **Python**, **CustomTkinter**, and **SQLite**.

The application allows users to convert between several currencies, swap currencies, and save their conversion history.

## Features

* Convert between multiple currencies
* Supports RUB, USD, EUR, CNY, and KZT
* Swap the source and target currencies
* Accepts both `.` and `,` as decimal separators
* Displays the conversion result
* Saves conversion history in SQLite
* Shows the last 15 conversions
* Allows users to clear the conversion history
* Dark mode interface

## Technologies

* Python
* CustomTkinter
* SQLite
* datetime

## Installation

Clone the repository:

```bash
git clone https://github.com/your-username/currency-converter.git
```

Go to the project folder:

```bash
cd currency-converter
```

Install the required library:

```bash
pip install customtkinter
```

Run the application:

```bash
python main.py
```

## Project Structure

```text
currency-converter/
│
├── main.py
├── cur.db
└── README.md
```

The `cur.db` database is created automatically when the application starts.

## How It Works

The program stores exchange rates in the `RATES` dictionary.

For example:

```python
RATES = {
    "RUB": 1,
    "USD": 90,
    "EUR": 98,
    "CNY": 12.5,
    "KZT": 0.18,
}
```

The application first converts the entered amount to Russian rubles and then converts the ruble value into the selected currency.

Conversion history is stored in an SQLite database.

## Important Note

The exchange rates in this project are **fixed example rates** and are not automatically updated from the internet.

For a real-world application, an exchange-rate API could be connected to provide current rates.

## Author

Created as a Python learning project.
