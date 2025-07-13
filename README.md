python install - https://www.python.org/downloads/

vc code - https://code.visualstudio.com/

расширение питона

новый файл main.py

PS: Get-ExecutionPolicy

Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

Y

python -m venv .venv # Или python3 -m venv .venv - создание виртуального окружения

.venv\Scripts\Activate.ps1 - ps .\venv\Scripts\activate.bat - cmd

deactivate - выйти

пакеты:

pip install PyQt5 pip show PyQt5

мб это pip install PyQt5 opencv-python numpy если несколько версий питона python3 -m pip install PyQt5 opencv-python numpy

проверка версии питона python --version pip --version

мб в дебагинг меню потыкать состандартным интерпритатором


```

import tkinter as tk
from tkinter import ttk, filedialog, messagebox

class RoboticArmGUI:
    def __init__(self, root):
        self.root = root
        self.root.title("Robotic Arm Control Panel")

        # Main frame
        self.mainframe = ttk.Frame(root, padding="10")
        self.mainframe.grid(column=0, row=0, sticky=(tk.N, tk.W, tk.E, tk.S))
        self.root.columnconfigure(0, weight=1)
        self.root.rowconfigure(0, weight=1)

        # Buttons for control
        self.create_control_buttons()

        # Joystick controls
        self.create_joystick_controls()

        # Gripper control
        self.create_gripper_control()

        # Motor status display
        self.create_motor_status_display()

        # System status indicator (traffic light)
        self.create_status_indicator()

        # Log display and save options
        self.create_log_display()

        # Program input for automatic execution
        self.create_program_controls()

        # Video feed display (placeholder)
        self.create_video_feed_display()

    def create_control_buttons(self):
        control_frame = ttk.LabelFrame(self.mainframe, text="Control Buttons", padding="10")
        control_frame.grid(column=0, row=0, sticky=(tk.W, tk.E))

        ttk.Button(control_frame, text="On", command=self.on_button_click).grid(column=0, row=0, padx=5, pady=5)
        ttk.Button(control_frame, text="Off", command=self.off_button_click).grid(column=1, row=0, padx=5, pady=5)
        ttk.Button(control_frame, text="Pause", command=self.pause_button_click).grid(column=2, row=0, padx=5, pady=5)
        ttk.Button(control_frame, text="Emergency Stop", command=self.emergency_stop_click).grid(column=3, row=0, padx=5, pady=5)

    def create_joystick_controls(self):
        joystick_frame = ttk.LabelFrame(self.mainframe, text="Joystick Controls", padding="10")
        joystick_frame.grid(column=0, row=1, sticky=(tk.W, tk.E))

        self.mode_var = tk.StringVar(value="Move L")
        ttk.Radiobutton(joystick_frame, text="Move L (Linear)", variable=self.mode_var, value="Move L").grid(column=0, row=0, sticky=tk.W)
        ttk.Radiobutton(joystick_frame, text="Move J (Joints)", variable=self.mode_var, value="Move J").grid(column=1, row=0, sticky=tk.W)

        # Placeholder for joystick visualization
        ttk.Label(joystick_frame, text="Joystick Visualization Placeholder").grid(column=0, row=1, columnspan=2, pady=5)

    def create_gripper_control(self):
        gripper_frame = ttk.LabelFrame(self.mainframe, text="Gripper Control", padding="10")
        gripper_frame.grid(column=0, row=2, sticky=(tk.W, tk.E))

        ttk.Button(gripper_frame, text="Open Gripper", command=self.open_gripper_click).grid(column=0, row=0, padx=5, pady=5)
        ttk.Button(gripper_frame, text="Close Gripper", command=self.close_gripper_click).grid(column=1, row=0, padx=5, pady=5)

    def create_motor_status_display(self):
        motor_frame = ttk.LabelFrame(self.mainframe, text="Motor Status", padding="10")
        motor_frame.grid(column=0, row=3, sticky=(tk.W, tk.E))

        # Placeholder for motor status display
        ttk.Label(motor_frame, text="Temperature: ").grid(column=0, row=0, sticky=tk.W, padx=5, pady=2)
        ttk.Label(motor_frame, text="Position: ").grid(column=0, row=1, sticky=tk.W, padx=5, pady=2)

        # Placeholder values
        self.temp_label = ttk.Label(motor_frame, text="--")
        self.temp_label.grid(column=1, row=0, sticky=tk.W, padx=5, pady=2)
        self.pos_label = ttk.Label(motor_frame, text="--")
        self.pos_label.grid(column=1, row=1, sticky=tk.W, padx=5, pady=2)

    def create_status_indicator(self):
        status_frame = ttk.LabelFrame(self.mainframe, text="Status Indicator", padding="10")
        status_frame.grid(column=0, row=4, sticky=(tk.W, tk.E))

        self.status_canvas = tk.Canvas(status_frame, width=100, height=100, bg="white")
        self.status_canvas.grid(column=0, row=0, padx=5, pady=5)
        self.update_status_indicator("blue")  # Default status: waiting

    def update_status_indicator(self, color):
        self.status_canvas.delete("all")
        if color == "green":
            self.status_canvas.create_oval(10, 10, 90, 90, fill="green", outline="black")
        elif color == "blue":
            self.status_canvas.create_oval(10, 10, 90, 90, fill="blue", outline="black")
        elif color == "yellow":
            self.status_canvas.create_oval(10, 10, 90, 90, fill="yellow", outline="black")
        elif color == "red":
            self.status_canvas.create_oval(10, 10, 90, 90, fill="red", outline="black")

    def create_log_display(self):
        log_frame = ttk.LabelFrame(self.mainframe, text="System Logs", padding="10")
        log_frame.grid(column=1, row=0, rowspan=5, sticky=(tk.N, tk.S, tk.W, tk.E))

        self.log_text = tk.Text(log_frame, height=20, width=40)
        self.log_text.grid(column=0, row=0, sticky=(tk.N, tk.S, tk.W, tk.E))

        log_control_frame = ttk.Frame(log_frame)
        log_control_frame.grid(column=0, row=1, sticky=(tk.W, tk.E))

        ttk.Button(log_control_frame, text="Save Logs", command=self.save_logs_click).grid(column=0, row=0, padx=5, pady=5)
        ttk.Button(log_control_frame, text="Clear Logs", command=self.clear_logs_click).grid(column=1, row=0, padx=5, pady=5)

    def create_program_controls(self):
        program_frame = ttk.LabelFrame(self.mainframe, text="Program Controls", padding="10")
        program_frame.grid(column=0, row=5, sticky=(tk.W, tk.E))

        self.program_text = tk.Text(program_frame, height=5, width=40)
        self.program_text.grid(column=0, row=0, columnspan=3, sticky=(tk.W, tk.E))

        ttk.Button(program_frame, text="Load Program", command=self.load_program_click).grid(column=0, row=1, padx=5, pady=5)
        ttk.Button(program_frame, text="Run Program", command=self.run_program_click).grid(column=1, row=1, padx=5, pady=5)
        ttk.Button(program_frame, text="Loop Program", command=self.loop_program_click).grid(column=2, row=1, padx=5, pady=5)

        ttk.Label(program_frame, text="Number of cycles:").grid(column=0, row=2, sticky=tk.E, padx=5, pady=2)
        self.cycle_entry = ttk.Entry(program_frame, width=5)
        self.cycle_entry.grid(column=1, row=2, sticky=tk.W, padx=5, pady=2)

    def create_video_feed_display(self):
        video_frame = ttk.LabelFrame(self.mainframe, text="Video Feed", padding="10")
        video_frame.grid(column=1, row=5, sticky=(tk.W, tk.E))

        # Placeholder for video feed
        ttk.Label(video_frame, text="Video Feed Placeholder").grid(column=0, row=0, padx=5, pady=5)
        ttk.Label(video_frame, text="Object Detection Visualization Placeholder").grid(column=0, row=1, padx=5, pady=5)

    # Placeholder functions for button commands
    def on_button_click(self):
        self.update_status_indicator("green")
        self.log_message("Робот включен")

    def off_button_click(self):
        self.update_status_indicator("blue")
        self.log_message("Робот на стартовой позиции")

    def pause_button_click(self):
        self.update_status_indicator("yellow")
        self.log_message("Робот в режиме паузы")

    def emergency_stop_click(self):
        self.update_status_indicator("red")
        self.log_message("Аварийная остановка")

    def open_gripper_click(self):
        self.log_message("Гриппер открыт")

    def close_gripper_click(self):
        self.log_message("Гриппер закрыт")

    def save_logs_click(self):
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt")])
        if file_path:
            with open(file_path, 'w') as file:
                file.write(self.log_text.get(1.0, tk.END))
            messagebox.showinfo("Info", "Логи сохранены")

    def clear_logs_click(self):
        self.log_text.delete(1.0, tk.END)

    def load_program_click(self):
        file_path = filedialog.askopenfilename(filetypes=[("Text files", "*.txt")])
        if file_path:
            with open(file_path, 'r') as file:
                self.program_text.delete(1.0, tk.END)
                self.program_text.insert(1.0, file.read())

    def run_program_click(self):
        self.log_message("Робот начал выполнение программы")
        self.log_message("Робот закончил выполнение программы")

    def loop_program_click(self):
        cycles = self.cycle_entry.get()
        if cycles.isdigit():
            self.log_message(f"Программа запущена в цикле на {cycles} раз(а)")
        else:
            messagebox.showerror("Ошибка", "Введите корректное количество циклов")

    def log_message(self, message):
        self.log_text.insert(tk.END, message + "\n")
        self.log_text.see(tk.END)

if __name__ == "__main__":
    root = tk.Tk()
    RoboticArmGUI(root)
    root.mainloop()
```




```

# Перешли в папку проекта
cd путь к проекту



# Инициализировали Git
git init

# Добавили все файлы
git add .

# Сделали первый коммит
git commit -m "Начальная версия"

# Работаем над кодом...

# Когда внесли изменения:
git add .
git commit -m "Улучшил интерфейс управления"

# Проверяем историю:
git log

Создание ветки на гит хабе
git push -u origin main или git push -u origin master

git status

git log

Создание новой ветки:
git checkout -b main     # создаём и переходим на ветку main
git add .
git commit -m "Начальный коммит"
git push -u origin main

Если репозиторий не пустой и есть какие то файлы:
git pull origin main --allow-unrelated-histories
git push -u origin main

Если ветка называется мастер, то использовать это
git push -u origin master

Если переименовать ветку:
git branch -M main
git push -u origin main

```
