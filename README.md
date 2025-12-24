# imports: tkinter, os, sys

import tkinter as tk
from tkinter import filedialog
from tkinter import messagebox
import os, sys

# code to add the .ico icon file. uses absolute path to the file, and a special folder MEIPASS that pyinstaller creates. 

def resource_path(relative):
    try:
        base_path = sys._MEIPASS
    except Exception:
        base_path = os.path.abspath(".")
    return os.path.join(base_path, relative)
    
# code to give access to file system and save the note in the user folder.

def save_doc():
    doc_contents = entry_field.get("1.0", tk.END)
    if not doc_contents.strip():
        messagebox.showwarning("file content cannot be empty")
        return
    
    doc_path = filedialog.asksaveasfilename(
        defaultextension=".txt",
        filetypes=[("Text Documents", "*.txt")]
        
    )

    if doc_path:
        try:
            with open(doc_path, 'w') as file:
                file.write(doc_contents)
            messagebox.showinfo("Success", f"File saved successfully at: {doc_path}")
        except Exception as e:
            messagebox.showerror(f"An error occurred, try again: {e}")

# code for the gui itself. 

win = tk.Tk()
win.iconbitmap(resource_path("driveicon.ico"))
win.title("Notebook")
win.geometry("400x400")
win.resizable(width=True, height=True)
win.config(bg="#B3AC63")

# uses .update to constantly update the variable that makes the text box bigger or smaller depending on the window size.

win.update()
win_x = win.winfo_width()
win_y = win.winfo_height()

# Button to save the note.

entry_create = tk.Button(win, text="Save note", borderwidth=3, background="#B9B468", command=save_doc)
entry_create.pack(pady=10)

# text box, using win_x and win_y variables to make the box as big as you like.

entry_field = tk.Text(win, height=win_x, width=win_y, background="#DAD48E", borderwidth=6)
entry_field.pack(pady=10)


win.mainloop()
