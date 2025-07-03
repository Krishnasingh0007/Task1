# Task1
TO-DO LIST

from tkinter import *
from tkinter import messagebox
import sqlite3 as sql

def add_task():  
    task_string = task_field.get().strip()  
    if len(task_string) == 0:  
        messagebox.showinfo('Error', 'Field is Empty.')  
    elif task_string in tasks:
        messagebox.showinfo('Duplicate', 'Task already exists.')
    else:    
        tasks.append(task_string)   
        the_cursor.execute('insert into tasks values (?)', (task_string ,))    
        the_connection.commit()
        list_update()    
        task_field.delete(0, 'end')  
    
def list_update():    
    clear_list()    
    for task in tasks:    
        task_listbox.insert('end', task)  
  
def delete_task():  
    try:  
        the_value = task_listbox.get(task_listbox.curselection())    
        if the_value in tasks:  
            tasks.remove(the_value)    
            the_cursor.execute('delete from tasks where title = ?', (the_value,))
            the_connection.commit()
            list_update()   
    except IndexError:   
        messagebox.showinfo('Error', 'No Task Selected. Cannot Delete.')        
  
def delete_all_tasks():  
    message_box = messagebox.askyesno('Delete All', 'Are you sure?')  
    if message_box == True:    
        while(len(tasks) != 0):    
            tasks.pop()    
        the_cursor.execute('delete from tasks')   
        the_connection.commit()
        list_update()  
   
def clear_list():   
    task_listbox.delete(0, 'end')  
  
def close():    
    the_connection.commit()
    the_cursor.close()
    the_connection.close()
    guiWindow.destroy()
    
def retrieve_database():    
    while(len(tasks) != 0):    
        tasks.pop()    
    for row in the_cursor.execute('select title from tasks'):    
        tasks.append(row[0])  
   
if __name__ == "__main__":   
    guiWindow = Tk()   
    guiWindow.title("To-Do List")  
    guiWindow.geometry("665x400+550+250")   
    guiWindow.resizable(0, 0)  
    guiWindow.configure(bg = "#B5E5CF")  
   
    the_connection = sql.connect('listOfTasks.db')   
    the_cursor = the_connection.cursor()   
    the_cursor.execute('create table if not exists tasks (title text)')  
    
    tasks = []  
        
    functions_frame = Frame(guiWindow, bg = "#8EE5EE") 
    functions_frame.pack(side = "top", expand = True, fill = "both")  
  
    task_label = Label(functions_frame, text="TO-DO-LIST \n Enter the Task Title:",
        font=("arial", 14, "bold"), bg="#8EE5EE", fg="#FF6103")    
    task_label.place(x=20, y=30)  
        
    task_field = Entry(functions_frame, font=("Arial", 14), width=42,
        fg="black", bg="white")    
    task_field.place(x=180, y=30)  
    
    add_button = Button(functions_frame, text="Add", width=15, bg='#D4AC0D',
        font=("arial", 14, "bold"), command=add_task)  
    
    del_button = Button(functions_frame, text="Remove", width=15, bg='#D4AC0D',
        font=("arial", 14, "bold"), command=delete_task)  
    
    del_all_button = Button(functions_frame, text="Delete All", width=15,
        font=("arial", 14, "bold"), bg='#D4AC0D', command=delete_all_tasks)  
    
    exit_button = Button(functions_frame, text="Exit / Close", width=52,
        bg='#D4AC0D', font=("arial", 14, "bold"), command=close)    
    
    add_button.place(x=18, y=80)  
    del_button.place(x=240, y=80)  
    del_all_button.place(x=460, y=80)  
    exit_button.place(x=17, y=330)  
    
    task_listbox = Listbox(functions_frame, width=70, height=9, font="bold",
        selectmode='SINGLE', bg="WHITE", fg="BLACK",
        selectbackground="#FF8C00", selectforeground="BLACK")    
    task_listbox.place(x=17, y=140)  
    
    retrieve_database()  
    list_update()    
    guiWindow.mainloop()

# Task2 
Calculator 

def calculator():
    print("Simple Calculator")
    print("------------------")
    print("Operations: +, -, *, /")

    try:
        num1 = float(input("Enter first number: "))
        operator = input("Enter operation (+, -, *, /): ")
        num2 = float(input("Enter second number: "))

        if operator == '+':
            result = num1 + num2
            print(f"Result: {num1} + {num2} = {result}")
        elif operator == '-':
            result = num1 - num2
            print(f"Result: {num1} - {num2} = {result}")
        elif operator == '*':
            result = num1 * num2
            print(f"Result: {num1} * {num2} = {result}")
        elif operator == '/':
            if num2 == 0:
                print("Error: Division by zero is not allowed.")
            else:
                result = num1 / num2
                print(f"Result: {num1} / {num2} = {result}")
        else:
            print("Invalid operator. Please use +, -, *, or /.")
    
    except ValueError:
        print("Invalid input. Please enter numeric values.")

if _name_ == "_main_":
    calculator()

#  Task3
Password Generator 

import random
import string

def generate_password():
    print("Password Generator")
    print("------------------")

    try:
        length = int(input("Enter desired password length: "))
        if length < 4:
            print("Password length should be at least 4 characters.")
            return

        # Character sets
        all_chars = string.ascii_letters + string.digits + string.punctuation

        # Random password generation
        password = ''.join(random.choice(all_chars) for _ in range(length))
        print(f"Generated Password: {password}")

    except ValueError:
        print("Invalid input. Please enter a numeric value.")

if _name_ == "_main_":
    generate_password()



