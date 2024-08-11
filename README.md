# Pomodoro-App-
This is a simple Pomodoro timer application built using Python's Tkinter library.The Pomodoro Technique is a time management method that breaks work into intervals, traditionally 25 minutes in length, separated by short breaks. This app automates the Pomodoro Technique, helping users stay focused and productive by alternating between work sessions and breaks.

Features
Work Timer: Set for 25 minutes (modifiable), representing a work session.
Short Break Timer: Set for 5 minutes (modifiable).
Long Break Timer: Set for 20 minutes (modifiable).
Automatic Session Tracking: The app automatically tracks the number of work sessions completed and provides a long break after four work sessions.
Visual Feedback: The timer updates visually on the app, showing the time remaining. Additionally, a checkmark is displayed after each work session is completed.
Reset Functionality: Easily reset the timer and session count with a click of a button.

How It Works
Start Timer: Clicking the "Start" button begins the timer. It will cycle through work sessions and breaks automatically.
Reset Timer: Clicking the "Reset" button stops the current timer and resets the session count, allowing the user to start fresh.

Code Explanation

Constants
The constants section defines color codes for the UI, font settings, and the duration for work, short break, and long break intervals.

PINK = "#e2979c"
RED = "#e7305b"
GREEN = "#9bdeac"
YELLOW = "#f7f5dd"
FONT_NAME = "Courier"
WORK_MIN = 0.2  # 12 seconds for demonstration purposes
SHORT_BREAK_MIN = 5
LONG_BREAK_MIN = 20

Timer Reset Function
The reset_timer function stops the timer, clears the session count, and resets the UI to its initial state.


def reset_timer():
    global timer, reps
    window.after_cancel(timer)
    count_label.config(text="")
    canvas.itemconfig(timer_text, text="00:00")
    timer_label.config(text="Timer")
    reps = 0


Timer Mechanism
The start_timer function increments the session count and decides whether to start a work session, short break, or long break based on the current session.

def start_timer():
    global reps
    reps += 1
    work_sec = WORK_MIN * 60
    short_break_sec = SHORT_BREAK_MIN * 60
    long_break_sec = LONG_BREAK_MIN * 60

    if reps % 8 == 0:
        count_down(long_break_sec)
        timer_label.config(text="Long break", fg=RED)
    elif reps % 2 == 0:
        count_down(short_break_sec)
        timer_label.config(text="Break", fg=PINK)
    else:
        count_down(work_sec)

        
Countdown Mechanism
The count_down function handles the countdown for each timer and updates the UI with the remaining time. It also triggers the next timer session once the current session ends.

def count_down(count):
    global timer
    count_min = math.floor(count / 60)
    count_sec = count % 60
    if count_sec < 10:
        count_sec = f"0{count_sec}"
    if count_min < 10:
        count_min = f"0{count_min}"

    canvas.itemconfig(timer_text, text=f"{count_min}:{count_sec}")
    if count > 0:
        timer = window.after(1000, count_down, count - 1)
    else:
        start_timer()
        mark = ""
        work_sessions = math.floor(reps / 2)
        for _ in range(work_sessions):
            mark += "✓"
        count_label.config(text=mark)

UI Setup
The UI is built using Tkinter's Canvas and Label widgets. The application has a simple interface with a tomato image in the center, a start button, a reset button, and labels to display the timer and session count.

window = Tk()
window.title("Pomodoro")
window.config(padx=100, pady=50, bg=YELLOW)

timer_label = Label(text="Timer", font=(FONT_NAME, 22, "bold"), fg=GREEN)
timer_label.config(bg=YELLOW)
timer_label.grid(column=1, row=0)

canvas = Canvas(width=200, height=224, bg=YELLOW, highlightthickness=0)
tomato_img = PhotoImage(file="tomato.png")
canvas.create_image(100, 112, image=tomato_img)
timer_text = canvas.create_text(103, 130, text="00:00", fill="white", font=(FONT_NAME, 35, "bold"))
canvas.grid(column=1, row=1)

start_button = Button(text="Start", highlightthickness=0, command=start_timer)
start_button.grid(column=0, row=2)

reset_button = Button(text="Reset", highlightthickness=0, command=reset_timer)
reset_button.grid(column=2, row=2)

count_label = Label(font=(FONT_NAME, 15, "bold"), fg=GREEN)
count_label.config(bg=YELLOW)
count_label.grid(column=1, row=3)

window.mainloop()

Customization
You can easily modify the work, short break, and long break durations by changing the values of WORK_MIN, SHORT_BREAK_MIN, and LONG_BREAK_MIN in the constants section.

Contributions
Feel free to fork this repository, submit issues, and send pull requests. Contributions are always welcome!
