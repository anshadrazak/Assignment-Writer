Assignment Writer

Code:


import PyPDF2
from reportlab.pdfgen import canvas
from reportlab.lib.pagesizes import letter
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont
import random

# Register the fonts
font_names = ['Fonty 1', 'Fonty', 'text1']
font_paths = ['./Fonty 1.ttf', './Fonty.ttf', './text1.ttf']
for font_name, font_path in zip(font_names, font_paths):
    pdfmetrics.registerFont(TTFont(font_name, font_path))

# Define a function to split the input text into separate lines
def split_text(text, max_width, font_names, font_size=24):
    lines = text.split("\n")
    result_lines = []
    for line in lines:
        words = line.split(' ')
        current_line = ''
        for word in words:
            for letter in word:
                current_font = random.choice(font_names)
                width = pdfmetrics.stringWidth(current_line + letter, current_font, font_size)
                if width <= max_width:
                    current_line += letter
                else:
                    result_lines.append(current_line)
                    current_line = letter
            current_line += ' '
        result_lines.append(current_line.strip())
    return '\n'.join(result_lines)

# Define the maximum width of each line
max_width = 610

# Split the input text into separate lines
text = "This is written by PC"
# Split the input text into separate lines
lines = split_text(text, max_width, font_names)

# Create a PDF file with the text
filename = 'output.pdf'
c = canvas.Canvas(filename, pagesize=letter)

# Calculate the maximum height available for the text on the page
max_text_height = letter[1] - 50

# Set the initial y-coordinate for the text
y = max_text_height

# Draw each line of text on the canvas
for line in lines.split('\n'):
    x = 50
    for letter in line:
        font_name = random.choice(font_names)
        c.setFont(font_name, 24)
        c.drawString(x, y, letter)
        x += pdfmetrics.stringWidth(letter, font_name, 24) - 2  # decrease x-coordinate to add less space between letters
    y -= 30  # decrease y-coordinate to add space between lines
    if y < 50:
        break  # stop drawing when there is no more space on the page

# Save the PDF file
c.save()

print(f"PDF file '{filename}' has been created with the text on separate lines.")
