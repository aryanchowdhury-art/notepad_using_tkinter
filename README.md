# notepad_using_tkinter
This Notepad application, built using Tkinter, features a modern dark theme and supports creating, opening, and saving files. It includes essential edit operations like cut, copy, paste, undo, and redo. A dynamic status bar shows the current line and column numbers, and keyboard shortcuts enhance user efficiency.


import tkinter as tk
from tkinter import filedialog, messagebox, scrolledtext
import os

def new_file():
    root.title("Untitled - Notepad")
    text_area.delete(1.0, tk.END)
    update_status_bar()

def open_file():
    file_path = filedialog.askopenfilename(defaultextension=".txt",
                                           filetypes=[("Text Documents", "*.txt"),
                                                      ("All Files", "*.*")])
    if file_path:
        root.title(os.path.basename(file_path) + " - Notepad")
        with open(file_path, "r") as file:
            text_area.delete(1.0, tk.END)
            text_area.insert(tk.INSERT, file.read())
        update_status_bar()

def save_file():
    file_path = filedialog.asksaveasfilename(defaultextension=".txt",
                                             filetypes=[("Text Documents", "*.txt"),
                                                        ("All Files", "*.*")])
    if file_path:
        with open(file_path, "w") as file:
            file.write(text_area.get(1.0, tk.END))
        root.title(os.path.basename(file_path) + " - Notepad")
        update_status_bar()

def exit_app():
    if messagebox.askokcancel("Quit", "Do you really want to quit?"):
        root.destroy()

def cut_text():
    text_area.event_generate("<<Cut>>")

def copy_text():
    text_area.event_generate("<<Copy>>")

def paste_text():
    text_area.event_generate("<<Paste>>")

def undo_action():
    text_area.event_generate("<<Undo>>")

def redo_action():
    text_area.event_generate("<<Redo>>")

def update_status_bar(event=None):
    line, column = text_area.index(tk.INSERT).split('.')
    status_bar.config(text=f"Line: {line}, Column: {column}")

# Create the main window
root = tk.Tk()
root.title("Untitled - Notepad")
root.geometry("800x600")

# Create a Text widget for the main text area
text_area = scrolledtext.ScrolledText(root, wrap='word', undo=True, font=("Helvetica", 12), bg="#f4f4f4", fg="#333")
text_area.pack(fill='both', expand=True)
text_area.bind("<KeyRelease>", update_status_bar)

# Create a Menu bar
menu_bar = tk.Menu(root)

# Create a File menu and add it to the menu bar
file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="New", command=new_file, accelerator="Ctrl+N")
file_menu.add_command(label="Open", command=open_file, accelerator="Ctrl+O")
file_menu.add_command(label="Save", command=save_file, accelerator="Ctrl+S")
file_menu.add_separator()
file_menu.add_command(label="Exit", command=exit_app, accelerator="Ctrl+Q")
menu_bar.add_cascade(label="File", menu=file_menu)

# Create an Edit menu and add it to the menu bar
edit_menu = tk.Menu(menu_bar, tearoff=0)
edit_menu.add_command(label="Cut", command=cut_text, accelerator="Ctrl+X")
edit_menu.add_command(label="Copy", command=copy_text, accelerator="Ctrl+C")
edit_menu.add_command(label="Paste", command=paste_text, accelerator="Ctrl+V")
edit_menu.add_separator()
edit_menu.add_command(label="Undo", command=undo_action, accelerator="Ctrl+Z")
edit_menu.add_command(label="Redo", command=redo_action, accelerator="Ctrl+Y")
menu_bar.add_cascade(label="Edit", menu=edit_menu)

# Attach the menu bar to the root window
root.config(menu=menu_bar)

# Create a status bar
status_bar = tk.Label(root, text="Line: 1, Column: 1", anchor='e')
status_bar.pack(fill='x', side='bottom')

# Bind keyboard shortcuts
root.bind("<Control-n>", lambda event: new_file())
root.bind("<Control-o>", lambda event: open_file())
root.bind("<Control-s>", lambda event: save_file())
root.bind("<Control-q>", lambda event: exit_app())
root.bind("<Control-x>", lambda event: cut_text())
root.bind("<Control-c>", lambda event: copy_text())
root.bind("<Control-v>", lambda event: paste_text())
root.bind("<Control-z>", lambda event: undo_action())
root.bind("<Control-y>", lambda event: redo_action())

# Start the main event loop
root.mainloop()
