# Import the time module
import time
from tkinter import *
from PIL import ImageTk, Image
import PIL
import multiprocessing
from tkinter import ttk, messagebox
from playsound import playsound
from threading import *
from openpyxl.workbook import Workbook
from openpyxl import load_workbook
import customtkinter, tkinter
import matplotlib.pyplot as plt
import sys
import os
from email.message import EmailMessage
import ssl
import smtplib

#set appearence mode to dark
customtkinter.set_appearance_mode("dark")
#set theme for widget - buttons etc
customtkinter.set_default_color_theme("blue")

# Creating a CounDown Class
class CountDown(customtkinter.CTk):
    def __init__(self, root):
        super().__init__()
        self.window = root
        self.window.geometry(f"{850}x{600}")
        self.window.title('System PPOŻ')
        self.window.iconbitmap('C:/Users/milos/Automatyzacja-projekt/fire_image.ico')
        # Tkinter window background color
        self.window.configure(bg='gray35')

        # Declaring a variable to pause the countdown time
        self.pause = False

        # This frame is used to show the countdown time label
        self.time_frame = customtkinter.CTkFrame(self.window, corner_radius=10,width=1, height=1, bg='gray35').grid(row=4, column=2)

        # Tkinter Labels
        #czas T1
        time_label = customtkinter.CTkLabel(self.window, text="Ustaw czas T1", corner_radius=8,
                         text_font=("times new roman", 16, "bold"), text_color="White", fg_color=("gray35"))
        time_label.grid(row=0, column=2)

        hour_label = customtkinter.CTkLabel(self.window, text="Godziny", corner_radius=8,
                           text_color="White", fg_color=("gray35"))
        hour_label.grid(row=1, column=1)

        minute_label = customtkinter.CTkLabel(self.window, text="Minuty", corner_radius=8,
                           text_color="White", fg_color=("gray35"))
        minute_label.grid(row=1, column=2)

        second_label = customtkinter.CTkLabel(self.window, text="Sekundy", corner_radius=8,
                           text_color="White", fg_color=("gray35"))
        second_label.grid(row=1, column=3)

        # czas T2 - Labels
        time2_label = customtkinter.CTkLabel(self.window, text="Ustaw czas T2", corner_radius=8,
                                             text_font=("times new roman", 16, "bold"), text_color="White",
                                             fg_color=("gray35"))
        time2_label.grid(row=8, column=2)

        hour2_label = customtkinter.CTkLabel(self.window, text="Godziny", corner_radius=8,
                                             text_color="White", fg_color=("gray35"))
        hour2_label.grid(row=9, column=1)

        minute2_label = customtkinter.CTkLabel(self.window, text="Minuty", corner_radius=8,
                                               text_color="White", fg_color=("gray35"))
        minute2_label.grid(row=9, column=2)

        second2_label = customtkinter.CTkLabel(self.window, text="Sekundy", corner_radius=8,
                                               text_color="White", fg_color=("gray35"))
        second2_label.grid(row=9, column=3)

        my_label = customtkinter.CTkLabel(self.window, text="Wybierz alarmujący element", corner_radius=8,
                                          text_font=("times new roman", 14), text_color="White", fg_color=("gray35"))
        my_label.grid(row=10, column=6)
        # ===========================================
        # Tkinter Comboboxes
        # Combobox for hours
        def combobox_callback(choice):
            print("Wybór wartości z ComboBox:", choice)

        self.hour_combobox = customtkinter.CTkComboBox(master=self.window,
                                     values=['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14',
             '15', '16', '17', '18', '19', '20', '21', '22', '23', '24'],
                                     command=combobox_callback)
        self.hour_combobox.grid(row=2, column=1)

        # Combobox for minutes
        self.minute_combobox = customtkinter.CTkComboBox(master=self.window,
                                     values=['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14',
             '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '22', '23', '24', '25', '26', '27', '28', '29', '30'
                                             , '31', '32', '33', '34', '35', '36', '37', '38', '39', '40', '41', '42', '43'
                                             , '44', '45', '46', '47', '48', '49', '50', '51', '52', '53', '54', '55', '56'
                                             , '57', '58', '59', '60'],
                                                         command=combobox_callback)
        self.minute_combobox.grid(row=2, column=2)

        # Combobox for seconds
        self.second_combobox = customtkinter.CTkComboBox(master=self.window,
                                     values=['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14',
             '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '22', '23', '24', '25', '26', '27', '28', '29', '30'
                                             , '31', '32', '33', '34', '35', '36', '37', '38', '39', '40', '41', '42', '43'
                                             , '44', '45', '46', '47', '48', '49', '50', '51', '52', '53', '54', '55', '56'
                                             , '57', '58', '59', '60'],
                                     command=combobox_callback)
        self.second_combobox.grid(row=2, column=3)
        # ===========================================
        # Tkinter Comboboxes - T2
        # Combobox for hours - T2
        self.hour2 = IntVar()
        self.hour2_combobox = customtkinter.CTkComboBox(master=self.window,
                                     values=['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14',
             '15', '16', '17', '18', '19', '20', '21', '22', '23', '24'],
                                     command=combobox_callback)
        self.hour2_combobox.grid(row=10, column=1)

        # Combobox for minutes - T2
        self.minute2 = IntVar()
        self.minute2_combobox = customtkinter.CTkComboBox(master=self.window,
                                     values=['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14',
             '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '22', '23', '24', '25', '26', '27', '28', '29', '30'
                                             , '31', '32', '33', '34', '35', '36', '37', '38', '39', '40', '41', '42', '43'
                                             , '44', '45', '46', '47', '48', '49', '50', '51', '52', '53', '54', '55', '56'
                                             , '57', '58', '59', '60'],
                                                         command=combobox_callback)
        self.minute2_combobox.grid(row=10, column=2)


        # Combobox for seconds - T2
        self.second2 = IntVar()
        self.second2_combobox = customtkinter.CTkComboBox(master=self.window,
                                     values=['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14',
             '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '22', '23', '24', '25', '26', '27', '28', '29', '30'
                                             , '31', '32', '33', '34', '35', '36', '37', '38', '39', '40', '41', '42', '43'
                                             , '44', '45', '46', '47', '48', '49', '50', '51', '52', '53', '54', '55', '56'
                                             , '57', '58', '59', '60'],
                                     command=combobox_callback)
        self.second2_combobox.grid(row=10, column=3)
        # ===========================================
        # Set Time Button
        # When the user will press this button
        # the 'Start' and 'Pause' button will
        # show inside the 'self.button_frame' frame
        set_button = customtkinter.CTkButton(self.window, text='Zatwierdź czas T1', fg_color="White",
                                             text_color="Black", border_color="Black", command=self.Get_Time)
        set_button.grid(row=3, column=2)

        exit_button = customtkinter.CTkButton(self.window, text='Wyjście', fg_color="White",
                                             text_color="Black", border_color="Black", command=self.quit)
        exit_button.grid(row=1, column=6)

        manual_button = customtkinter.CTkButton(self.window, text='Instrukcja', fg_color="White",
                                              text_color="Black", border_color="Black")
        manual_button.grid(row=2, column=6)

        radio_var = tkinter.IntVar(self)

        radiobutton_1 = customtkinter.CTkRadioButton(master=self.window, text="Alarmowanie 1 stopniowe",
                                                     command=self.radiobutton, variable=radio_var, value=1)
        radiobutton_1.grid(row=11, column=6)

        def quit(self):
            root.destroy()

        # select function
        def select(self):
            my_label.config(text=my_listbox.get(ANCHOR))

        # create frame and scrollbar
        frame1 = Frame(self.window)
        frame1.grid(row=8, column=6)
        scrollbar1 = Scrollbar(frame1, orient=HORIZONTAL)

        # create listbox
        my_listbox = Listbox(frame1, yscrollcommand=scrollbar1)
        my_listbox.grid()

        # configure scrollbar
        scrollbar1.config(command=my_listbox.xview)
        scrollbar1.grid(sticky=EW)

        # create wb - workbook (xls file)
        wb = load_workbook('C:\\Users\\milos\\Automatyzacja-projekt\\Arkusz Konfiguracyjny_6000_wer_7.075.xlsm')
        # create ws - work sheet
        ws = wb['Tabela Linii dozorowych']

        my_listbox.insert(END, f'{ws["B3"].value}: {ws["C3"].value}: {ws["D3"].value}', f'{ws["B4"].value}: {ws["C4"].value}: {ws["D4"].value}', f'{ws["B5"].value}: {ws["C5"].value}: {ws["D5"].value}'
                          , f'{ws["B6"].value}: {ws["C6"].value}: {ws["D6"].value}', f'{ws["B7"].value}: {ws["C7"].value}: {ws["D7"].value}', f'{ws["B8"].value}: {ws["C8"].value}: {ws["D8"].value}'
                          , f'{ws["B9"].value}: {ws["C9"].value}: {ws["D9"].value}', f'{ws["B10"].value}: {ws["C10"].value}: {ws["D10"].value}', f'{ws["B11"].value}: {ws["C11"].value}: {ws["D11"].value}'
                          , f'{ws["B12"].value}: {ws["C12"].value}: {ws["D12"].value}', f'{ws["B13"].value}: {ws["C13"].value}: {ws["D13"].value}', f'{ws["B14"].value}: {ws["C14"].value}: {ws["D14"].value}'
                          , f'{ws["B15"].value}: {ws["C15"].value}: {ws["D15"].value}', f'{ws["B16"].value}: {ws["C16"].value}: {ws["D16"].value}', f'{ws["B17"].value}: {ws["C17"].value}: {ws["D17"].value}'
                          , f'{ws["B18"].value}: {ws["C18"].value}: {ws["D18"].value}', f'{ws["B19"].value}: {ws["C19"].value}: {ws["D19"].value}', f'{ws["B20"].value}: {ws["C20"].value}: {ws["D20"].value}'
                          , f'{ws["B21"].value}: {ws["C21"].value}: {ws["D21"].value}', f'{ws["B22"].value}: {ws["C22"].value}: {ws["D22"].value}', f'{ws["B23"].value}: {ws["C23"].value}: {ws["D23"].value}'
                          , f'{ws["B24"].value}: {ws["C24"].value}: {ws["D24"].value}', f'{ws["B25"].value}: {ws["C25"].value}: {ws["D25"].value}', f'{ws["B26"].value}: {ws["C26"].value}: {ws["D26"].value}'
                          , f'{ws["B27"].value}: {ws["C27"].value}: {ws["D27"].value}', f'{ws["B28"].value}: {ws["C28"].value}: {ws["D28"].value}', f'{ws["B29"].value}: {ws["C29"].value}: {ws["D29"].value}'
                          , f'{ws["B30"].value}: {ws["C30"].value}: {ws["D30"].value}', f'{ws["B31"].value}: {ws["C31"].value}: {ws["D31"].value}', f'{ws["B32"].value}: {ws["C32"].value}: {ws["D32"].value}'
                          , f'{ws["B33"].value}: {ws["C33"].value}: {ws["D33"].value}', f'{ws["B34"].value}: {ws["C34"].value}: {ws["D34"].value}', f'{ws["B35"].value}: {ws["C35"].value}: {ws["D35"].value}'
                          , f'{ws["B36"].value}: {ws["C36"].value}: {ws["D36"].value}',f'{ws["B37"].value}: {ws["C37"].value}: {ws["D37"].value}', f'{ws["B38"].value}: {ws["C38"].value}: {ws["D38"].value}'
                          , f'{ws["B39"].value}: {ws["C39"].value}: {ws["D39"].value}',f'{ws["B40"].value}: {ws["C40"].value}: {ws["D40"].value}',f'{ws["B41"].value}: {ws["C41"].value}: {ws["D41"].value}'
                          , f'{ws["B42"].value}: {ws["C42"].value}: {ws["D42"].value}', f'{ws["B43"].value}: {ws["C43"].value}: {ws["D43"].value}', f'{ws["B44"].value}: {ws["C44"].value}: {ws["D44"].value}'
                          , f'{ws["B45"].value}: {ws["C45"].value}: {ws["D45"].value}', f'{ws["B46"].value}: {ws["C46"].value}: {ws["D46"].value}', f'{ws["B47"].value}: {ws["C47"].value}: {ws["D47"].value}'
                          , f'{ws["B48"].value}: {ws["C48"].value}: {ws["D48"].value}', f'{ws["B49"].value}: {ws["C49"].value}: {ws["D49"].value}', f'{ws["B50"].value}: {ws["C50"].value}: {ws["D50"].value}'
                          , f'{ws["B51"].value}: {ws["C51"].value}: {ws["D51"].value}', f'{ws["B52"].value}: {ws["C52"].value}: {ws["D52"].value}', f'{ws["B53"].value}: {ws["C53"].value}: {ws["D53"].value}'
                          , f'{ws["B54"].value}: {ws["C54"].value}: {ws["D54"].value}', f'{ws["B55"].value}: {ws["C55"].value}: {ws["D55"].value}', f'{ws["B56"].value}: {ws["C56"].value}: {ws["D56"].value}'
                          , f'{ws["B57"].value}: {ws["C57"].value}: {ws["D57"].value}', f'{ws["B58"].value}: {ws["C58"].value}: {ws["D58"].value}', f'{ws["B59"].value}: {ws["C59"].value}: {ws["D59"].value}'
                          , f'{ws["B59"].value}: {ws["C59"].value}: {ws["D59"].value}', f'{ws["B60"].value}: {ws["C60"].value}: {ws["D60"].value}', f'{ws["B61"].value}: {ws["C61"].value}: {ws["D61"].value}'
                          , f'{ws["B62"].value}: {ws["C62"].value}: {ws["D62"].value}', f'{ws["B63"].value}: {ws["C63"].value}: {ws["D63"].value}', f'{ws["B64"].value}: {ws["C64"].value}: {ws["D64"].value}'
                          , f'{ws["B65"].value}: {ws["C65"].value}: {ws["D65"].value}', f'{ws["B66"].value}: {ws["C66"].value}: {ws["D66"].value}', f'{ws["B67"].value}: {ws["C67"].value}: {ws["D67"].value}'
                          , f'{ws["B68"].value}: {ws["C68"].value}: {ws["D68"].value}', f'{ws["B69"].value}: {ws["C69"].value}: {ws["D69"].value}', f'{ws["B70"].value}: {ws["C70"].value}: {ws["D70"].value}'
                          , f'{ws["B71"].value}: {ws["C71"].value}: {ws["D71"].value}', f'{ws["B72"].value}: {ws["C72"].value}: {ws["D72"].value}', f'{ws["B73"].value}: {ws["C73"].value}: {ws["D73"].value}'
                          , f'{ws["B74"].value}: {ws["C74"].value}: {ws["D74"].value}', f'{ws["B75"].value}: {ws["C75"].value}: {ws["D75"].value}', f'{ws["B76"].value}: {ws["C76"].value}: {ws["D76"].value}'
                          , f'{ws["B77"].value}: {ws["C77"].value}: {ws["D77"].value}', f'{ws["B78"].value}: {ws["C78"].value}: {ws["D78"].value}', f'{ws["B79"].value}: {ws["C79"].value}: {ws["D79"].value}'
                          , f'{ws["B80"].value}: {ws["C80"].value}: {ws["D80"].value}', f'{ws["B81"].value}: {ws["C81"].value}: {ws["D81"].value}', f'{ws["B82"].value}: {ws["C82"].value}: {ws["D82"].value}'
                          , f'{ws["B83"].value}: {ws["C83"].value}: {ws["D83"].value}', f'{ws["B84"].value}: {ws["C84"].value}: {ws["D84"].value}', f'{ws["B85"].value}: {ws["C85"].value}: {ws["D85"].value}'
                          , f'{ws["B86"].value}: {ws["C86"].value}: {ws["D86"].value}', f'{ws["B87"].value}: {ws["C87"].value}: {ws["D87"].value}', f'{ws["B88"].value}: {ws["C88"].value}: {ws["D88"].value}'
                          , f'{ws["B89"].value}: {ws["C89"].value}: {ws["D89"].value}', f'{ws["B90"].value}: {ws["C90"].value}: {ws["D90"].value}', f'{ws["B91"].value}: {ws["C91"].value}: {ws["D91"].value}'
                          , f'{ws["B92"].value}: {ws["C92"].value}: {ws["D92"].value}', f'{ws["B93"].value}: {ws["C93"].value}: {ws["D93"].value}', f'{ws["B94"].value}: {ws["C94"].value}: {ws["D94"].value}'
                          , f'{ws["B95"].value}: {ws["C95"].value}: {ws["D95"].value}', f'{ws["B96"].value}: {ws["C96"].value}: {ws["D96"].value}', f'{ws["B97"].value}: {ws["C97"].value}: {ws["D97"].value}'
                          , f'{ws["B98"].value}: {ws["C98"].value}: {ws["D98"].value}', f'{ws["B99"].value}: {ws["C99"].value}: {ws["D99"].value}', f'{ws["B100"].value}: {ws["C100"].value}: {ws["D100"].value}'
                          , f'{ws["B101"].value}: {ws["C101"].value}: {ws["D101"].value}', f'{ws["B102"].value}: {ws["C102"].value}: {ws["D102"].value}', f'{ws["B103"].value}: {ws["C103"].value}: {ws["D103"].value}'
                          , f'{ws["B104"].value}: {ws["C104"].value}: {ws["D104"].value}', f'{ws["B105"].value}: {ws["C105"].value}: {ws["D105"].value}', f'{ws["B106"].value}: {ws["C106"].value}: {ws["D106"].value}'
                          , f'{ws["B107"].value}: {ws["C107"].value}: {ws["D107"].value}', f'{ws["B108"].value}: {ws["C108"].value}: {ws["D108"].value}', f'{ws["B109"].value}: {ws["C109"].value}: {ws["D109"].value}'
                          , f'{ws["B110"].value}: {ws["C110"].value}: {ws["D110"].value}', f'{ws["B111"].value}: {ws["C111"].value}: {ws["D111"].value}', f'{ws["B112"].value}: {ws["C112"].value}: {ws["D112"].value}'
                          , f'{ws["B113"].value}: {ws["C113"].value}: {ws["D113"].value}', f'{ws["B114"].value}: {ws["C114"].value}: {ws["D114"].value}', f'{ws["B115"].value}: {ws["C115"].value}: {ws["D115"].value}'
                          , f'{ws["B116"].value}: {ws["C116"].value}: {ws["D116"].value}', f'{ws["B117"].value}: {ws["C117"].value}: {ws["D117"].value}', f'{ws["B118"].value}: {ws["C118"].value}: {ws["D118"].value}'
                          , f'{ws["B119"].value}: {ws["C119"].value}: {ws["D119"].value}', f'{ws["B120"].value}: {ws["C120"].value}: {ws["D120"].value}', f'{ws["B121"].value}: {ws["C121"].value}: {ws["D121"].value}'
                          , f'{ws["B122"].value}: {ws["C122"].value}: {ws["D122"].value}', f'{ws["B123"].value}: {ws["C123"].value}: {ws["D123"].value}', f'{ws["B124"].value}: {ws["C124"].value}: {ws["D124"].value}'
                          , f'{ws["B125"].value}: {ws["C125"].value}: {ws["D125"].value}', f'{ws["B126"].value}: {ws["C126"].value}: {ws["D126"].value}', f'{ws["B127"].value}: {ws["C127"].value}: {ws["D127"].value}')

        my_listbox.bind("<ButtonRelease-1>", select)

    # It will destroy the window
    def Cancel(self):
        self.pause = True
        self.hour_combobox.set(0)
        self.minute_combobox.set(0)
        self.second_combobox.set(0)
        self.hour2_combobox.set(0)
        self.minute2_combobox.set(0)
        self.second2_combobox.set(0)

        self.time_display.config(text=f"Pozostały czas: {0}: {0}: {0}")
        self.time_display.update()

    # When the set button is pressed, this
    # function gets called

    def radiobutton(self):
        self.hour2_combobox = customtkinter.CTkComboBox(values=[""], state = "readonly").grid(row=10, column=1)
        self.minute2_combobox = customtkinter.CTkComboBox(values=[""], state="readonly").grid(row=10, column=2)
        self.second2_combobox = customtkinter.CTkComboBox(values=[""], state="readonly").grid(row=10, column=3)
        radio_var = tkinter.IntVar(self)
        radiobutton_2 = customtkinter.CTkRadioButton(master=self.window, text="Alarmowanie 2 stopniowe",
                                                     command=self.radiobutton2, variable=radio_var, value=2)
        radiobutton_2.grid(row=12, column=6)

    def radiobutton2(self):
        root.destroy()
        os.startfile("test.py")

    def Get_Time(self):
        self.time_display = Label(self.time_frame,
                                  font=('Helvetica', 16, "bold"),
                                  bg='gray35', fg='yellow')
        self.time_display.grid(row=4, column=2)

        try:
            # Total amount of time in seconds
            h = (int(self.hour_combobox.get()) * 3600)
            m = (int(self.minute_combobox.get()) * 60)
            s = (int(self.second_combobox.get()))
            self.time_left = h + m + s

            # If the user try to set the default time(0:0:0) then
            # a warning message will display
            if s == 0 and m == 0 and h == 0:
                messagebox.showwarning('Uwaga!', \
                                       'Ustaw prawidłową wartość')
            else:
                # Start Button
                start_button = customtkinter.CTkButton(self.window, text='Wywołanie alarmu',fg_color="White",
                                      text_color="Black", border_color="Black", command=self.Threading)
                start_button.grid(row=5, column=2)

                # Pause Button
                pause_button = customtkinter.CTkButton(self.window, text='Potwierdzenie', fg_color="Green",
                                      text_color="White", border_color="Black", command=self.pause_time)
                pause_button.grid(row=6, column=1)

                # ROP Button
                ROP_button = customtkinter.CTkButton(self.window, text='ROP', fg_color="White",
                                      text_color="Red", border_color="Black",command=self.ROP)
                ROP_button.grid(row=6, column=3)

                # Cancel button
                cancel_button = customtkinter.CTkButton(self.window, text='Kasowanie',fg_color="Red",
                                      text_color="White", border_color="Black", command=self.Cancel)
                cancel_button.grid(row=6, column=2)


        except (RuntimeError, TypeError, NameError):
            pass

    # Creating a thread to run the show_time function
    def Threading(self):
        # Killing a thread through "daemon=True" isn't a good idea
        self.x = Thread(target=self.start_time, daemon=True)
        self.x.start()

    def ROP(self):
        self.pause = True
        self.hour_combobox.set(0)
        self.minute_combobox.set(0)
        self.second_combobox.set(0)
        self.hour2_combobox.set(0)
        self.minute2_combobox.set(0)
        self.second2_combobox.set(0)
        process = multiprocessing.Process(target=playsound,
                                          args=('C:/Users/milos/Automatyzacja-projekt/wariant1.mp3',))
        process.start()
        self.time_display.config(text=f"Pozostały czas: {0}: {0}: {0}")
        messagebox.showinfo('Odliczanie zakończone', 'Wciśnij OK aby zakończyć alarmowanie')
        # Email sender function
        email_sender = 'arturnowak.test@gmail.com'
        # there is 2 more option to declare a password - 1. get password or 2. enter password in brackets
        # first environment variable was created in user os
        email_password = os.environ.get("Email_Password")
        email_receiver = 'miloszwojtko@gmail.com'

        subject = "Uwaga Pożar!"
        body = """Uwaga! W budynku wykryto pożar! Wciśnięty ROP"""
        email = EmailMessage()
        email['From'] = email_sender
        email['To'] = email_receiver
        email['Subject'] = subject
        email.set_content(body)

        context = ssl.create_default_context()

        with smtplib.SMTP_SSL('smtp.gmail.com', 465, context=context) as smtp:
            smtp.login(email_sender, email_password)
            smtp.sendmail(email_sender, email_receiver, email.as_string())
        process.terminate()


    # It wil clear all the widgets inside the
    # 'self.button_frame' frame(Start and Pause buttons)
    def Clear_Screen(self):
        for widget in self.button_frame.winfo_children():
            widget.destroy()

    def pause_time(self):
        self.pause = True

        mins, secs = divmod(self.time_left, 60)
        hours = 0
        if mins > 60:
            # hour minute
            hours, mins = divmod(mins, 60)

        self.time_display.config(text=f"Pozostały czas: {hours}: {mins}: {secs}")
        self.time_display.update()

        # Total amount of time in seconds
        h = (int(self.hour2_combobox.get()) * 3600)
        m = (int(self.minute2_combobox.get()) * 60)
        s = (int(self.second2_combobox.get()))
        #dodano 2 sek ze zwględu na opóźnienie około 2 sek od rozpoczęcia odliczania czasu T2
        self.time_left = h + m + (s + 2)

        # sleep function: for 1 second
        time.sleep(1)
        self.time_left = self.time_left - 1

        self.x = Thread(target=self.start_time, daemon=True)
        self.x.start()

    # When the Start button will be pressed then,
    # this "show_time" function will get called.
    def start_time(self):
        self.pause = False
        while self.time_left > 0:
            mins, secs = divmod(self.time_left, 60)

            hours = 0
            if mins > 60:
                # hour minute
                hours, mins = divmod(mins, 60)

            self.time_display.config(text=f"Pozostały czas: {hours}: {mins}: {secs}")
            self.time_display.update()
            # sleep function: for 1 second
            time.sleep(1)
            self.time_left = self.time_left - 1
            # When the time is over, a piece of music will
            # play in the background
            if self.time_left <= 0:
                process = multiprocessing.Process(target=playsound,
                                                  args=('C:/Users/milos/Automatyzacja-projekt/wariant1.mp3',))
                process.start()
                self.time_display.config(text=f"Pozostały czas: {0}: {0}: {0}")
                messagebox.showinfo('Odliczanie zakończone', 'Wciśnij OK aby zakończyć alarmowanie')
                # Email sender function
                email_sender = 'arturnowak.test@gmail.com'
                # there is 2 more option to declare a password - 1. get password or 2. enter password in brackets
                # first environment variable was created in user os
                email_password = os.environ.get("Email_Password")
                email_receiver = 'miloszwojtko@gmail.com'

                subject = "Uwaga Pożar!"
                body = """Uwaga! W budynku wykryto pożar!"""
                email = EmailMessage()
                email['From'] = email_sender
                email['To'] = email_receiver
                email['Subject'] = subject
                email.set_content(body)

                context = ssl.create_default_context()

                with smtplib.SMTP_SSL('smtp.gmail.com', 465, context=context) as smtp:
                    smtp.login(email_sender, email_password)
                    smtp.sendmail(email_sender, email_receiver, email.as_string())
                process.terminate()
                # Clearing the 'self.button_frame' frame
                self.Clear_Screen()
            # if the pause button is pressed,
            # the while loop will break
            if self.pause == True:
                break

if __name__ == "__main__":
    root = Tk()
    obj = CountDown(root)
    root.mainloop()