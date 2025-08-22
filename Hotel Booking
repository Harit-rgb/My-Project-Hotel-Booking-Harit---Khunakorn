import sqlite3
from tkinter import *
from tkinter import messagebox
import customtkinter as ctk
from PIL import Image, ImageTk
from customtkinter import CTkImage
from datetime import datetime
from tkinter import filedialog
import re

conn = sqlite3.connect('hotels_ALL_DATA.db')
c_hotels = conn.cursor()
c = conn.cursor()
c.execute('''CREATE TABLE IF NOT EXISTS users (
                username TEXT PRIMARY KEY, 
                password TEXT, 
                email TEXT, 
                role TEXT DEFAULT 'user',
                first_name TEXT,
                last_name TEXT,
                phone TEXT
            )''')
conn.commit()
c.execute('''CREATE TABLE IF NOT EXISTS bookings (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                username TEXT,
                hotel_id INTEGER,
                hotel_name TEXT,
                room_number INTEGER,
                price REAL,
                check_in_date TEXT,
                check_out_date TEXT,
                guest_count INTEGER,
                status TEXT DEFAULT 'unpaid',
                FOREIGN KEY (username) REFERENCES users (username)
            )''')
conn.commit()

def time(label):
    now = datetime.now()
    current_time = now.strftime("%Y-%m-%d %H:%M:%S")
    label.configure(text=current_time) 
    label.after(1000, time, label)

def add_admin_account():
    c.execute("SELECT * FROM users WHERE username = 'admin'")
    admin = c.fetchone()
    if not admin:
        c.execute("INSERT INTO users (username, password, email, role) VALUES (?, ?, ?, ?)",
                  ('admin', '1234', 'admin@example.com', 'admin'))
        conn.commit()
        print("Admin account created successfully.")

def login():
    username = entry_user.get()
    password = entry_pass.get()

    if not username or not password:
        messagebox.showerror("Error", "Please fill in both Username and Password!")
        return

    c.execute("SELECT * FROM users WHERE username = ?", (username,))
    user = c.fetchone()

    if not user:
        messagebox.showerror("Error", "Username not found!")
        return

    c.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))
    user_with_password = c.fetchone()

    if not user_with_password:
        messagebox.showerror("Error", "Incorrect password!")
        return

    role = user_with_password[3]
    print(f"User: {username}, Role: {role}")

    messagebox.showinfo("Login Success", "Welcome, " + username + "!")
    login_window.destroy()

    if role == 'admin':
        print("Opening Admin Window...")
        open_admin_menu(username)
    else:
        print("Opening Main Menu for User...")
        open_main_menu(username)

def open_login_window():
    global login_window, entry_user, entry_pass, bg_image, bg_photo

    ctk.set_appearance_mode("light")
    ctk.set_default_color_theme("blue")

    login_window = ctk.CTk()
    login_window.title("Login")
    login_window.attributes('-fullscreen', True)

    screen_width = login_window.winfo_screenwidth()
    screen_height = login_window.winfo_screenheight()

    canvas = ctk.CTkCanvas(login_window, width=screen_width, height=screen_height)
    canvas.pack(fill=BOTH, expand=True)

    bg_image = Image.open("C:/Users/asus/Desktop/BG/Hotellogin.jpg")
    bg_image = bg_image.resize((screen_width, screen_height), Image.LANCZOS)
    bg_photo = ImageTk.PhotoImage(bg_image)
    canvas.create_image(0, 0, image=bg_photo, anchor='nw')

    Label_login = ctk.CTkLabel(login_window, text="Login", font=("Arial", 50))
    canvas.create_window(screen_width/2, 150, window=Label_login)

    Label_user = ctk.CTkLabel(login_window, text="Username", font=("Arial", 30))
    canvas.create_window(screen_width/2, 250, window=Label_user)

    entry_user = ctk.CTkEntry(login_window, width=300, height=50, font=("Arial", 24))
    canvas.create_window(screen_width/2, 320, window=entry_user)

    Label_pass = ctk.CTkLabel(login_window, text="Password", font=("Arial", 30))
    canvas.create_window(screen_width/2, 400, window=Label_pass)

    entry_pass = ctk.CTkEntry(login_window, show="*", width=300, height=50, font=("Arial", 24))
    canvas.create_window(screen_width/2, 470, window=entry_pass)

    btn_login = ctk.CTkButton(login_window, text="Login", command=login, corner_radius=15, 
                              font=("Arial", 24), width=150, height=60)
    canvas.create_window(screen_width/2 - 150, 550, window=btn_login)

    btn_register = ctk.CTkButton(login_window, text="Register", command=open_register_window, corner_radius=15, 
                                 font=("Arial", 24), width=150, height=60)
    canvas.create_window(screen_width/2 + 150, 550, window=btn_register)

    btn_close = ctk.CTkButton(login_window, text="Close", command=login_window.destroy, corner_radius=15, 
                              font=("Arial", 24), width=150, height=60)
    canvas.create_window(screen_width/2, screen_height - 150, window=btn_close)

    time_label = ctk.CTkLabel(login_window, font=("Arial", 18))
    time_label.place(relx=1.0, rely=1.0, anchor='se', x=-20, y=-20)
    time(time_label)     

    login_window.bind('<Return>', login)
    login_window.mainloop()

def open_register_window():   
    global register_window, entry_reg_user, entry_reg_email, entry_reg_pass, entry_reg_fname, entry_reg_lname, entry_reg_phone, bg_image, bg_photo

    if 'login_window' in globals():
        login_window.destroy()

    ctk.set_appearance_mode("System")
    ctk.set_default_color_theme("blue")

    register_window = ctk.CTk()
    register_window.title("Register")
    register_window.attributes('-fullscreen', True)

    screen_width = register_window.winfo_screenwidth()
    screen_height = register_window.winfo_screenheight()

    canvas = Canvas(register_window, width=screen_width, height=screen_height)
    canvas.pack(fill=BOTH, expand=True)

    bg_image = Image.open("C:/Users/asus/Desktop/BG/Hotelregister.jpg") 
    bg_image = bg_image.resize((screen_width, screen_height), Image.LANCZOS)
    bg_photo = ImageTk.PhotoImage(bg_image)
    canvas.create_image(0, 0, image=bg_photo, anchor='nw')

    Label(canvas, text="Register", font=("Arial", 50), bg='white').place(relx=0.5, rely=0.12, anchor=CENTER)

    Label(canvas, text="First Name", font=("Arial", 30), bg='white').place(relx=0.5, rely=0.2, anchor=CENTER)
    entry_reg_fname = ctk.CTkEntry(register_window, width=300, height=50, font=("Arial", 24))
    entry_reg_fname.place(relx=0.5, rely=0.25, anchor=CENTER)

    Label(canvas, text="Last Name", font=("Arial", 30), bg='white').place(relx=0.5, rely=0.3, anchor=CENTER)
    entry_reg_lname = ctk.CTkEntry(register_window, width=300, height=50, font=("Arial", 24))
    entry_reg_lname.place(relx=0.5, rely=0.35, anchor=CENTER)

    Label(canvas, text="Phone", font=("Arial", 30), bg='white').place(relx=0.5, rely=0.4, anchor=CENTER)
    entry_reg_phone = ctk.CTkEntry(register_window, width=300, height=50, font=("Arial", 24))
    entry_reg_phone.place(relx=0.5, rely=0.45, anchor=CENTER)

    Label(canvas, text="Username", font=("Arial", 30), bg='white').place(relx=0.5, rely=0.5, anchor=CENTER)
    entry_reg_user = ctk.CTkEntry(register_window, width=300, height=50, font=("Arial", 24))
    entry_reg_user.place(relx=0.5, rely=0.55, anchor=CENTER)

    Label(canvas, text="Email", font=("Arial", 30), bg='white').place(relx=0.5, rely=0.6, anchor=CENTER)
    entry_reg_email = ctk.CTkEntry(register_window, width=300, height=50, font=("Arial", 24))
    entry_reg_email.place(relx=0.5, rely=0.65, anchor=CENTER)

    Label(canvas, text="Password", font=("Arial", 30), bg='white').place(relx=0.5, rely=0.7, anchor=CENTER)
    entry_reg_pass = ctk.CTkEntry(register_window, width=300, height=50, font=("Arial", 24), show="*")
    entry_reg_pass.place(relx=0.5, rely=0.75, anchor=CENTER)

    confirm_button = ctk.CTkButton(register_window, text="Confirm", width=150, height=60, 
                                   font=("Arial", 24), command=register_user, corner_radius=10)
    confirm_button.place(relx=0.35, rely=0.85, anchor=CENTER)

    back_button = ctk.CTkButton(register_window, text="Back", width=150, height=60, 
                                font=("Arial", 24), command=lambda: (register_window.destroy(), open_login_window()), corner_radius=10)
    back_button.place(relx=0.65, rely=0.85, anchor=CENTER)

    register_window.mainloop()

def register_user(): 
    fname = entry_reg_fname.get()
    lname = entry_reg_lname.get()
    phone = entry_reg_phone.get()
    username = entry_reg_user.get()
    email = entry_reg_email.get()
    password = entry_reg_pass.get()

    error_messages = []

    if not fname or not lname or not phone or not username or not email or not password:
        error_messages.append("กรุณากรอกข้อมูลให้ครบถ้วน!")

    if not (phone.isdigit() and len(phone) == 10 and phone.startswith("0")):
        error_messages.append("เบอร์โทรศัพท์ต้องมี 10 หลัก และขึ้นต้นด้วย 0!")

    email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if not re.match(email_pattern, email):
        error_messages.append("รูปแบบอีเมลไม่ถูกต้อง!")

    if len(username) < 4:
        error_messages.append("Username ต้องมีความยาวอย่างน้อย 4 ตัวอักษร!")

    if len(password) < 6:
        error_messages.append("Password ต้องมีความยาวอย่างน้อย 6 ตัวอักษร!")

    if not any(char.islower() for char in password):
        error_messages.append("Password ต้องมีตัวอักษรพิมพ์เล็กอย่างน้อย 1 ตัว!")

    if not any(char.isupper() for char in password):
        error_messages.append("Password ต้องมีตัวอักษรพิมพ์ใหญ่อย่างน้อย 1 ตัว!")

    if error_messages:
        messagebox.showerror("Error", "\n".join(error_messages))
        return

    try:
        c.execute("INSERT INTO users (first_name, last_name, phone, username, email, password) VALUES (?, ?, ?, ?, ?, ?)",
                  (fname, lname, phone, username, email, password))
        conn.commit()
        messagebox.showinfo("Registration Success", "ลงทะเบียนสำเร็จ!")
        register_window.destroy()
        open_login_window()
    except sqlite3.IntegrityError:
        messagebox.showerror("Error", "Username นี้มีอยู่ในระบบแล้ว!")

def home(username):
    root.withdraw()

    hotel_window = Toplevel(root)
    hotel_window.title("Hotel Info")
    hotel_window.attributes("-fullscreen", True)

    canvas = Canvas(hotel_window)
    scrollbar = Scrollbar(hotel_window, orient="vertical", command=canvas.yview)
    scrollable_frame = Frame(canvas)

    for widget in scrollable_frame.winfo_children():
        widget.destroy()

    scrollable_frame.bind("<Configure>", lambda e: canvas.configure(scrollregion=canvas.bbox("all")))

    canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")

    canvas.pack(side="left", fill="both", expand=True)
    scrollbar.pack(side="right", fill="y")
    canvas.configure(yscrollcommand=scrollbar.set)

    hotels = load_hotel_details()

    for index, hotel in enumerate(hotels):
        row = index // 5
        col = index % 5

        hotel_frame = Frame(scrollable_frame, pady=10, padx=10, bd=1, relief="solid")
        hotel_frame.grid(row=row, column=col, padx=5, pady=5, sticky="nsew")

        scrollable_frame.grid_columnconfigure(col, weight=1)

        Label(hotel_frame, text=f"Name: {hotel['name']}", font=("Arial", 14, "bold")).pack(anchor='w', pady=2)
        Label(hotel_frame, text=f"Location: {hotel['location']}", font=("Arial", 12)).pack(anchor='w', pady=2)
        Label(hotel_frame, text=f"Average Price: {hotel['price']} THB", font=("Arial", 12)).pack(anchor='w', pady=2)

        try:
            img = Image.open(hotel['image'])
            img = img.resize((300, 200), Image.LANCZOS)
            photo = ImageTk.PhotoImage(img)

            label_img = Label(hotel_frame, image=photo)
            label_img.image = photo
            label_img.pack(pady=5)

        except Exception as e:
            print(f"Error loading image for {hotel['name']}: {e}")

        btn_details = ctk.CTkButton(hotel_frame, text="Details", command=lambda h=hotel: view_hotel_details(h), font=("Arial", 14), corner_radius=15, border_color="black", width=250)
        btn_details.pack(pady=2)

        btn_book = ctk.CTkButton(hotel_frame, text="Book Room", command=lambda h=hotel: book_room(h, username), font=("Arial", 14), corner_radius=15, border_color="black", width=250)
        btn_book.pack(pady=2)


    btn_back = ctk.CTkButton(hotel_window, text="Back", command=lambda: back_to_main(hotel_window, username), font=("Arial", 18), corner_radius=10, border_color="black", width=200)
    btn_back.pack(pady=5)

    time_label = Label(hotel_window, font=("Arial", 18), bg='white')
    time_label.place(relx=1.0, rely=1.0, anchor='se')
    time(time_label)

    hotel_window.mainloop()

def load_hotel_details():
    try:
        conn = sqlite3.connect('hotels_All_Data.db')
        cursor = conn.cursor()

        cursor.execute('''
            SELECT hotel_id, name, details, location, price, image, email, facebook, instagram, phone, status
            FROM hotels
            WHERE status = 'active'
        ''')
        hotels = cursor.fetchall()

        hotel_data = []

        for hotel in hotels:
            hotel_id, name, details, location, price, image, email, facebook, instagram, phone, status = hotel

            cursor.execute('''
                SELECT amenity
                FROM amenities
                WHERE hotel_id = ?
            ''', (hotel_id,))
            amenities = [row[0] for row in cursor.fetchall()]

            cursor.execute('''
                SELECT room_number
                FROM available_rooms
                WHERE hotel_id = ?
            ''', (hotel_id,))
            available_rooms = [row[0] for row in cursor.fetchall()]

            # แปลงรูปแบบราคาให้มี " , " หากเกินหลักพัน
            formatted_price = f"{price:,}"

            hotel_info = {
                "hotel_id": hotel_id,
                "name": name,
                "details": details,
                "amenities": amenities,
                "location": location,
                "price": formatted_price,  # ใช้ราคาแบบจัดรูปแบบ
                "image": image,
                "contact": {
                    "email": email,
                    "facebook": facebook,
                    "instagram": instagram,
                    "phone": phone
                },
                "available_rooms": available_rooms
            }
            hotel_data.append(hotel_info)

        conn.close()
        return hotel_data
    except Exception as e:
        print(f"Error loading hotel details from SQLite: {e}")
        return []

def view_hotel_details(hotel):
    hotel_data = load_hotel_details()
    
    hotel_info = next((item for item in hotel_data if item["name"] == hotel["name"]), None)
    
    details_window = Toplevel(root)
    details_window.title(f"Hotel Details - {hotel['name']}")
    
    window_width = 600
    window_height = 800
    details_window.geometry(f"{window_width}x{window_height}")
    
    try:
        img = Image.open(hotel['image'])
        img = img.resize((window_width, 300), Image.LANCZOS)
        photo = ImageTk.PhotoImage(img)

        label_img = Label(details_window, image=photo)
        label_img.image = photo
        label_img.pack(pady=10)
    except Exception as e:
        print(f"Error loading image for {hotel['name']}: {e}")

    if hotel_info:
        Label(details_window, text=f"Details: {hotel_info['details']}", wraplength=400, justify=LEFT, font=("Arial", 15)).pack(pady=10)
        Label(details_window, text="Amenities:", font=("Arial", 15, "bold")).pack(pady=5)
        for amenity in hotel_info["amenities"]:
            Label(details_window, text=f"- {amenity}", font=("Arial", 14)).pack(anchor="w", padx=50)
        Label(details_window, text=f"Location: {hotel_info['location']}", font=("Arial", 14)).pack(pady=5)
        Label(details_window, text=f"Average Price: {hotel_info['price']} THB", font=("Arial", 14)).pack(pady=5)
    else:
        Label(details_window, text="Details not available.", font=("Arial", 14)).pack(pady=5)

    close_button = ctk.CTkButton(details_window, text="Close", font=("Arial", 14), width=150, corner_radius=10, border_color="black", command=details_window.destroy)
    close_button.pack(pady=20)
  
def book_room(hotel, username):
    print(f"hotel: {hotel}")
    print(f"hotel_id: {hotel['hotel_id']}")

    booking_window = ctk.CTkToplevel(root)
    booking_window.title(f"จองห้องพัก - {hotel['name']}")

    screen_width = root.winfo_screenwidth()
    screen_height = root.winfo_screenheight()
    window_width = int(screen_width * 0.95)
    window_height = int(screen_height * 0.95)
    window_x = (screen_width - window_width) // 2
    window_y = (screen_height - window_height) // 2
    booking_window.geometry(f"{window_width}x{window_height}+{window_x}+{window_y}")

    canvas = ctk.CTkCanvas(booking_window, width=window_width, height=window_height)
    canvas.pack(fill="both", expand=True)

    bg_image = Image.open("C:/Users/asus/Desktop/BG/booking2.png").resize((window_width, window_height), Image.LANCZOS)
    bg_photo = ImageTk.PhotoImage(bg_image)
    canvas.create_image(0, 0, image=bg_photo, anchor='nw')
    canvas.bg_photo = bg_photo

    label_hotel_name = ctk.CTkLabel(booking_window, text=hotel['name'], font=("Arial", 30, "bold"), bg_color="white")
    label_hotel_name.place(relx=0.5, rely=0.2, anchor='center')  # Adjusted relative Y position

    c_hotels.execute("SELECT room_number FROM available_rooms WHERE hotel_id = ? AND status = 'ยังไม่จอง'", (hotel['hotel_id'],))
    available_rooms = c_hotels.fetchall()

    label_rooms = ctk.CTkLabel(booking_window, text="เลือกห้องที่ต้องการจอง:", font=("Arial", 25), bg_color="white")
    label_rooms.place(relx=0.5, rely=0.25, anchor='center')

    selected_room = ctk.StringVar()
    room_menu = ctk.CTkOptionMenu(booking_window, variable=selected_room, values=[str(room[0]) for room in available_rooms])
    room_menu.place(relx=0.5, rely=0.3, anchor='center')


    def focus_next(entry, next_entry, max_length):
        if len(entry.get()) >= max_length:
            next_entry.focus()

    label_check_in = ctk.CTkLabel(booking_window, text="วันที่เช็คอิน:", font=("Arial", 25), bg_color="white")
    label_check_in.place(relx=0.5, rely=0.35, anchor='center')

    entry_check_in_day = ctk.CTkEntry(booking_window, width=60)
    entry_check_in_day.place(relx=0.4, rely=0.4, anchor='center')
    label_slash1 = ctk.CTkLabel(booking_window, text="/", font=("Arial", 25), bg_color="white")
    label_slash1.place(relx=0.45, rely=0.4, anchor='center')
    entry_check_in_month = ctk.CTkEntry(booking_window, width=60)
    entry_check_in_month.place(relx=0.5, rely=0.4, anchor='center')
    label_slash2 = ctk.CTkLabel(booking_window, text="/", font=("Arial", 25), bg_color="white")
    label_slash2.place(relx=0.55, rely=0.4, anchor='center')
    entry_check_in_year = ctk.CTkEntry(booking_window, width=80)
    entry_check_in_year.place(relx=0.6, rely=0.4, anchor='center')

    entry_check_in_day.bind('<KeyRelease>', lambda e: focus_next(entry_check_in_day, entry_check_in_month, 2))
    entry_check_in_month.bind('<KeyRelease>', lambda e: focus_next(entry_check_in_month, entry_check_in_year, 2))

    label_check_out = ctk.CTkLabel(booking_window, text="วันที่เช็คเอาต์:", font=("Arial", 25), bg_color="white")
    label_check_out.place(relx=0.5, rely=0.5, anchor='center')

    entry_check_out_day = ctk.CTkEntry(booking_window, width=60)
    entry_check_out_day.place(relx=0.4, rely=0.55, anchor='center')
    label_slash3 = ctk.CTkLabel(booking_window, text="/", font=("Arial", 25), bg_color="white")
    label_slash3.place(relx=0.45, rely=0.55, anchor='center')
    entry_check_out_month = ctk.CTkEntry(booking_window, width=60)
    entry_check_out_month.place(relx=0.5, rely=0.55, anchor='center')
    label_slash4 = ctk.CTkLabel(booking_window, text="/", font=("Arial", 25), bg_color="white")
    label_slash4.place(relx=0.55, rely=0.55, anchor='center')
    entry_check_out_year = ctk.CTkEntry(booking_window, width=80)
    entry_check_out_year.place(relx=0.6, rely=0.55, anchor='center')

    entry_check_out_day.bind('<KeyRelease>', lambda e: focus_next(entry_check_out_day, entry_check_out_month, 2))
    entry_check_out_month.bind('<KeyRelease>', lambda e: focus_next(entry_check_out_month, entry_check_out_year, 2))

    label_guest_count = ctk.CTkLabel(booking_window, text="จำนวนผู้เข้าพัก:", font=("Arial", 25), bg_color="white")
    label_guest_count.place(relx=0.5, rely=0.65, anchor='center')
    entry_guest_count = ctk.CTkEntry(booking_window)
    entry_guest_count.place(relx=0.5, rely=0.7, anchor='center')

    def convert_year_th_to_ad(date_str):
        try:
            day, month, year = map(int, date_str.split('/'))
            if year > 2500:
                year -= 543
            return datetime(year, month, day)
        except ValueError:
            raise ValueError("รูปแบบวันที่ไม่ถูกต้อง")

    def confirm_booking():
        try:
            check_in_date = f"{entry_check_in_day.get()}/{entry_check_in_month.get()}/{entry_check_in_year.get()}"
            check_out_date = f"{entry_check_out_day.get()}/{entry_check_out_month.get()}/{entry_check_out_year.get()}"
            guest_count = entry_guest_count.get()
            room_number = selected_room.get()

            if check_in_date and check_out_date and guest_count and room_number:
                in_date = convert_year_th_to_ad(check_in_date)
                out_date = convert_year_th_to_ad(check_out_date)
                today = datetime.now()

                if in_date < today:
                    messagebox.showerror("Error", "ไม่สามารถจองได้ วันเช็คอินไม่ถูกต้อง")
                    return

                if in_date >= out_date:
                    messagebox.showerror("Error", "วันที่เช็คเอาต์ต้องหลังจากวันที่เช็คอิน!")
                    return

                if int(guest_count) > 5:
                    messagebox.showerror("Error", "จำนวนผู้เข้าพักต้องไม่เกิน 5 คน!")
                    return

                price = hotel['price']

                c_hotels.execute("INSERT INTO bookings (username, hotel_id, hotel_name, room_number, price, check_in_date, check_out_date, guest_count, status) VALUES (?, ?, ?, ?, ?, ?, ?, ?, 'unpaid')", 
                                 (username, hotel['hotel_id'], hotel['name'], room_number, price, check_in_date, check_out_date, guest_count))
            
                c_hotels.execute("UPDATE available_rooms SET status = 'จองแล้ว' WHERE hotel_id = ? AND room_number = ?", 
                                 (hotel['hotel_id'], room_number))
                conn.commit()

                messagebox.showinfo("Booking Success", "จองห้องสำเร็จ!")
                booking_window.destroy()

            else:
                messagebox.showerror("Error", "กรุณากรอกข้อมูลให้ครบถ้วน")

        except ValueError as e:
            messagebox.showerror("Error", f"รูปแบบวันที่ไม่ถูกต้อง: {e}")
        except Exception as e:
            messagebox.showerror("Error", f"Failed to book room: {e}")

    btn_confirm = ctk.CTkButton(booking_window, text="ยืนยันการจอง", command=confirm_booking, font=("Arial", 25), corner_radius=20)
    btn_confirm.place(relx=0.5, rely=0.8, anchor='center')

def back_to_main(hotel_window, username):
    global root
    hotel_window.destroy() 
    root.deiconify() 

def show_my_bookings(username, root):
    root.withdraw()

    bookings_window = ctk.CTkToplevel()
    bookings_window.title("รายการจองของคุณ")
    bookings_window.attributes("-fullscreen", True)  # หน้าต่างเต็มจอ

    inner_frame = ctk.CTkFrame(bookings_window)
    inner_frame.pack(expand=True, fill="both", padx=20, pady=20)

    c.execute(
        "SELECT b.id, b.username, b.room_number, b.price, b.check_in_date, b.check_out_date, b.guest_count, h.name AS hotel_name, h.hotel_id "
        "FROM bookings b JOIN hotels h ON b.hotel_id = h.hotel_id WHERE b.username = ?",
        (username,)
    )
    bookings = c.fetchall()

    if not bookings:
        no_booking_label = ctk.CTkLabel(inner_frame, text="ไม่มีการจอง", font=("Arial", 30), text_color="black")
        no_booking_label.pack(pady=20, expand=True)
    else:
        for booking in bookings:
            booking_id, username, room_number, price, check_in_date, check_out_date, guest_count, hotel_name, hotel_id = booking

            booking_info = f"รายการที่: {booking_id} โรงแรม: {hotel_name}, ห้อง: {room_number}, ราคา: {price} THB, เช็คอิน: {check_in_date}, เช็คเอาต์: {check_out_date}, จำนวนผู้เข้าพัก: {guest_count}"
        
            info_label = ctk.CTkLabel(inner_frame, text=booking_info, font=("Arial", 24), text_color="black", anchor="center", width=bookings_window.winfo_width() - 40)
            info_label.pack(pady=10, padx=10, fill="x", expand=True)

            def cancel_booking(booking_id, room_number, hotel_id):
                confirm = messagebox.askyesno("ยกเลิกการจอง", "คุณแน่ใจหรือไม่ว่าต้องการยกเลิกการจองนี้?")
                if confirm:
                    try:
                        c.execute("DELETE FROM bookings WHERE id = ?", (booking_id,))
                        
                        c.execute("UPDATE available_rooms SET status = 'ยังไม่จอง' WHERE hotel_id = ? AND room_number = ?", 
                                (hotel_id, room_number))
                    
                        conn.commit()
                        
                        messagebox.showinfo("การยกเลิกสำเร็จ", "การจองถูกยกเลิกเรียบร้อยแล้ว")
                        bookings_window.destroy()
                        show_my_bookings(username, root)

                    except Exception as e:
                        messagebox.showerror("Error", f"เกิดข้อผิดพลาดขณะยกเลิกการจอง: {e}")

            cancel_button = ctk.CTkButton(inner_frame, 
                                          text="ยกเลิกการจอง", 
                                          font=("Arial", 18), 
                                          command=lambda bid=booking_id, room=room_number, h_id=hotel_id: cancel_booking(bid, room, h_id), 
                                          corner_radius=10)
            cancel_button.pack(pady=10)

    def close_bookings_window():
        bookings_window.destroy()
        root.deiconify()  

    close_button = ctk.CTkButton(bookings_window, text="ปิด", command=close_bookings_window, font=("Arial", 18), corner_radius=10, fg_color="#f44336", text_color="white")
    close_button.place(relx=0.9, rely=0.95, anchor="center")



def payment_menu(username):
    payment_window = Toplevel(root)
    payment_window.title("เมนูการชำระเงิน")
    payment_window.state("zoomed")  

    ctk.set_appearance_mode("light")
    ctk.set_default_color_theme("blue")

    canvas_frame = ctk.CTkFrame(payment_window)
    canvas_frame.pack(fill="both", expand=True)

    canvas = ctk.CTkCanvas(canvas_frame)
    canvas.pack(side="left", fill="both", expand=True)

    scrollbar = ctk.CTkScrollbar(canvas_frame, command=canvas.yview, orientation="vertical")
    scrollbar.pack(side="right", fill="y")

    canvas.configure(yscrollcommand=scrollbar.set)
    canvas.bind("<Configure>", lambda e: canvas.configure(scrollregion=canvas.bbox("all")))

    inner_frame = ctk.CTkFrame(canvas, fg_color="transparent")
    canvas.create_window((0, 0), window=inner_frame, anchor="n", width=payment_window.winfo_screenwidth() - 50)

    c.execute("SELECT id, room_number, price, check_in_date, check_out_date, guest_count, status FROM bookings WHERE username = ?", (username,))
    bookings = c.fetchall()

    if not bookings:
        ctk.CTkLabel(inner_frame, text="ไม่มีรายการชำระเงิน", font=("Arial", 30)).pack(expand=True, pady=20)
    else:
        ctk.CTkLabel(inner_frame, text="รายการชำระเงินของคุณ", font=("Arial", 36, "bold")).pack(pady=20, anchor="center")

        for booking in bookings:
            booking_id, room_number, price, check_in_date, check_out_date, guest_count, status = booking

            # แปลงราคาด้วยคอมม่า
            formatted_price = f"{price:}"  

            button_text = (
                f"ห้อง: {room_number}\n"
                f"ราคา: {formatted_price} THB\n"
                f"เช็คอิน: {check_in_date}\n"
                f"เช็คเอาต์: {check_out_date}\n"
                f"จำนวนผู้เข้าพัก: {guest_count}\n"
            )

            if status == 'paid':
                button_text += "สถานะ: ชำระเงินแล้ว"
                button_color = "#4CAF50"  # สีเขียว
            else:
                button_text += "สถานะ: รอชำระเงิน"
                button_color = "#FF6347"  # สีแดง

            ctk.CTkButton(
                inner_frame,
                text=button_text,
                font=("Arial", 24),
                corner_radius=20,
                fg_color=button_color,  # กำหนดสีของปุ่มตามสถานะ
                text_color="black",
                command=lambda bid=booking_id, amt=price: show_payment_window(bid, amt),
                height=150,
                width=500,
            ).pack(pady=10, anchor="center")

    ctk.CTkButton(inner_frame, text="ปิด", command=payment_window.destroy, corner_radius=20).pack(pady=20, anchor="center")

def show_payment_window(booking_id, amount):
    payment_detail_window = Toplevel(root)
    payment_detail_window.title("รายละเอียดการชำระเงิน")

    window_width = 800
    window_height = 700
    payment_detail_window.geometry(f"{window_width}x{window_height}")

    canvas = ctk.CTkCanvas(payment_detail_window, width=window_width, height=window_height)
    canvas.pack(fill="both", expand=True)

    bg_image = Image.open("C:/Users/asus/Desktop/BG/cash.jpg") 
    bg_image = bg_image.resize((window_width, window_height), Image.LANCZOS)
    bg_photo = ImageTk.PhotoImage(bg_image)
    canvas.create_image(0, 0, image=bg_photo, anchor='nw')
    canvas.bg_photo = bg_photo 

    formatted_amount = f"{amount:}"

    amount_label = ctk.CTkLabel(
        payment_detail_window, 
        text=f"จำนวนเงินที่ต้องชำระ: {formatted_amount} THB", 
        font=("Arial", 24), 
        text_color="black"
    )
    amount_label.place(relx=0.5, rely=0.2, anchor='center')

    qr_code_image_path = "C:/Users/asus/Desktop/BG/QrcodePay.png"  
    qr_code_image = CTkImage(light_image=Image.open(qr_code_image_path), size=(200, 200))
    qr_code_label = ctk.CTkLabel(payment_detail_window, image=qr_code_image, text="")
    qr_code_label.place(relx=0.5, rely=0.4, anchor='center')  

    def confirm_payment():
        try:
            c.execute("UPDATE bookings SET status = 'paid' WHERE id = ?", (booking_id,))
            conn.commit()

            receipt_window = Toplevel(payment_detail_window)
            receipt_window.title("ใบเสร็จการชำระเงิน")
            receipt_window.geometry("400x300")

            canvas = ctk.CTkCanvas(receipt_window, width=400, height=300)
            canvas.pack(fill="both", expand=True)

            bg_image = Image.open("C:/Users/asus/Desktop/BG/bill.jpg")  
            bg_image = bg_image.resize((400, 300), Image.LANCZOS) 
            bg_photo = ImageTk.PhotoImage(bg_image)
            canvas.create_image(0, 0, image=bg_photo, anchor='nw')
            canvas.bg_photo = bg_photo  

            # แก้ไขข้อความในใบเสร็จ
            receipt_text = f"""
            ใบเสร็จการชำระเงิน

            หมายเลขการจอง: {booking_id}
            จำนวนเงินที่ชำระ: {formatted_amount} THB
            วันที่ชำระ: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}
            สถานะ: ชำระเงินเรียบร้อยแล้ว
            """

            receipt_label = ctk.CTkLabel(receipt_window, text=receipt_text, font=("Arial", 14), text_color="black", anchor='center')
            receipt_label.place(relx=0.5, rely=0.5, anchor='center') 

            close_button = ctk.CTkButton(receipt_window, text="ปิด", command=receipt_window.destroy)
            close_button.pack(pady=10)

        except sqlite3.OperationalError as e:
            ctk.CTkLabel(payment_detail_window, text=f"ข้อผิดพลาด: {e}", font=("Arial", 16), text_color="red").place(relx=0.5, rely=0.6, anchor='center')

    def on_payment_window_close():
        for child in payment_detail_window.winfo_children():
            if isinstance(child, Toplevel):
                child.destroy()
        payment_detail_window.destroy()

    payment_detail_window.protocol("WM_DELETE_WINDOW", on_payment_window_close)

    confirm_button = ctk.CTkButton(
        payment_detail_window, 
        text="ยืนยันการชำระเงิน", 
        command=confirm_payment, 
        corner_radius=20,
        fg_color="#4CAF50",
        text_color="black" 
    )
    confirm_button.place(relx=0.5, rely=0.75, anchor='center') 

    close_button = ctk.CTkButton(payment_detail_window, text="ปิด", command=on_payment_window_close, text_color="black")
    close_button.place(relx=0.5, rely=0.85, anchor='center')

def confirm_exit():
    """ฟังก์ชันยืนยันการออกจากโปรแกรม"""
    global root, admin_window
    try:
        if 'root' in globals() and root is not None and root.winfo_exists():
            answer = messagebox.askyesno("Exit Confirmation", "Are you sure you want to exit?")
            if answer:
                root.destroy()
                root = None  
        elif 'admin_window' in globals() and admin_window is not None and admin_window.winfo_exists():
            answer = messagebox.askyesno("Exit Confirmation", "Are you sure you want to exit?")
            if answer:
                admin_window.destroy()
                admin_window = None 
    except TclError:
        pass

def logout():
    """ฟังก์ชันสำหรับการออกจากระบบและกลับไปที่หน้าล็อกอิน"""
    global root, admin_window

    try:
        if 'root' in globals() and root.winfo_exists():
            root.destroy()
    except TclError:
        print("หน้าต่าง root ไม่มีอยู่แล้วหรือถูกทำลายไปแล้ว")

    try:
        if 'admin_window' in globals() and admin_window.winfo_exists():
            admin_window.destroy()
    except TclError:
        print("หน้าต่าง admin_window ไม่มีอยู่แล้วหรือถูกทำลายไปแล้ว")
    open_login_window()

def open_main_menu(username):
    global root, bg_image, bg_photo
    ctk.set_appearance_mode("light")
    ctk.set_default_color_theme("blue")

    root = ctk.CTk()
    root.title("Hotel Booking System")

    screen_width = root.winfo_screenwidth()
    screen_height = root.winfo_screenheight()
    window_width = int(screen_width * 0.95)
    window_height = int(screen_height * 0.95)
    window_x = (screen_width - window_width) // 2
    window_y = (screen_height - window_height) // 2
    root.geometry(f"{window_width}x{window_height}+{window_x}+{window_y}")

    canvas = ctk.CTkCanvas(root, width=window_width, height=window_height)
    canvas.pack(fill="both", expand=True)

    bg_image = Image.open("C:/Users/asus/Desktop/BG/E&N Hotel Booking.png").resize((window_width, window_height), Image.LANCZOS)
    bg_photo = ImageTk.PhotoImage(bg_image)
    canvas.create_image(0, 0, image=bg_photo, anchor='nw')

    canvas.image = bg_photo

    button_data = [
        ("ข้อมูลโรงแรม (Hotel Info)", lambda: home(username), 0.35),
        ("ดูการจอง (View Bookings)", lambda: show_my_bookings(username, root), 0.45),
        ("เมนูการชำระเงิน (Payment Menu)", lambda: payment_menu(username), 0.55),
        ("ออกจากระบบ (Logout)", logout, 0.65),
        ("Exit", confirm_exit, 0.75)
    ]

    button_width = 400
    button_height = 80

    for text, command, rel_y in button_data:
        frame = ctk.CTkFrame(root, corner_radius=20, width=button_width, height=button_height,
                             fg_color="transparent", border_color="white", border_width=2)
        frame.place(relx=0.5, rely=rel_y, anchor='center')

        button = ctk.CTkButton(frame, text=text, command=command,
                               corner_radius=20, font=("Arial", 24), width=button_width, height=button_height,
                               fg_color="#87CEFA", text_color="black", border_color="white", border_width=2)
        button.pack(expand=True)

    about_us_button = ctk.CTkButton(
        root, text="About Us", font=("Arial", 18),
        command=open_about_us_window, width=120, height=40,
        fg_color="#87CEFA", text_color="black", border_color="white", border_width=2
    )
    about_us_button.place(relx=0.5, rely=0.95, anchor='center')

    time_label = ctk.CTkLabel(root, font=("Arial", 24))
    time_label.place(relx=1.0, rely=1.0, anchor='se', x=-10, y=-10)
    time(time_label)

    root.mainloop()

def open_about_us_window():
    about_us_window = ctk.CTkToplevel(root)
    about_us_window.title("About Us")
    about_us_window.geometry("1320x660")

    about_us_image = Image.open("C:/Users/asus/Desktop/BG/2.png") 
    about_us_image = about_us_image.resize((1320,760), Image.LANCZOS)
    about_us_photo = ImageTk.PhotoImage(about_us_image)

    label = ctk.CTkLabel(about_us_window, image=about_us_photo, text="")
    label.image = about_us_photo  
    label.pack(fill="both", expand=True)

def open_admin_menu(username): 
    global admin_window
    ctk.set_appearance_mode("light")
    ctk.set_default_color_theme("blue") 

    admin_window = ctk.CTk()
    admin_window.title("Admin Menu")

    # คำนวณขนาดหน้าต่าง
    screen_width = admin_window.winfo_screenwidth()
    screen_height = admin_window.winfo_screenheight()
    window_width = int(screen_width * 0.95)
    window_height = int(screen_height * 0.95)
    window_x = (screen_width - window_width) // 2
    window_y = (screen_height - window_height) // 2
    admin_window.geometry(f"{window_width}x{window_height}+{window_x}+{window_y}")

    canvas = ctk.CTkCanvas(admin_window, width=window_width, height=window_height)
    canvas.pack(fill="both", expand=True)

    button_data = [
        ("เพิ่มโรงแรม (Add Hotel)", open_add_hotel_window, 0.25),  
        ("ซ่อนโรงแรม (Hide Hotel)", hide_hotel_window, 0.35),      
        ("แก้ไขข้อมูลโรงแรม (Edit Hotel)", open_edit_hotel_window, 0.45), 
        ("ดูข้อมูลผู้จองโรงแรม (View Hotel Bookings)", lambda: admin_view_bookings(admin_window, username), 0.55), 
        ("ดูข้อมูลผู้ใช้ (View Users)", show_user_details, 0.65),  
        ("ออกจากระบบ (Logout)", logout, 0.75),  
        ("Exit", confirm_exit, 0.85)  
    ]

    button_width = 450  
    button_height = 80  

    for text, command, rel_y in button_data:
        frame = ctk.CTkFrame(admin_window, corner_radius=20, width=button_width, height=button_height, 
                             fg_color="transparent", border_color="white", border_width=2)
        frame.place(relx=0.5, rely=rel_y, anchor='center')

        button = ctk.CTkButton(frame, text=text, command=command,
                               corner_radius=20, font=("Arial", 24), width=button_width, height=button_height,
                               fg_color="#87CEFA", text_color="black", border_color="white", border_width=2)  
        button.pack(expand=True)

    time_label = ctk.CTkLabel(admin_window, font=("Arial", 24)) 
    time_label.place(relx=1.0, rely=1.0, anchor='se', x=-10, y=-10)
    time(time_label)

    admin_window.mainloop()

def admin_view_bookings(admin_window, username):
    admin_window.withdraw()

    bookings_window = ctk.CTkToplevel()
    bookings_window.title("รายการจองทั้งหมด (ผู้ดูแลระบบ)")
    bookings_window.attributes("-fullscreen", True)  

    inner_frame = ctk.CTkFrame(bookings_window)
    inner_frame.pack(expand=True, fill="both", padx=20, pady=20)

    c.execute(
        "SELECT b.id, b.username, b.room_number, b.price, b.check_in_date, "
        "b.check_out_date, b.guest_count, h.name AS hotel_name "
        "FROM bookings b JOIN hotels h ON b.hotel_id = h.hotel_id"
    )
    bookings = c.fetchall()

    if not bookings:
        no_booking_label = ctk.CTkLabel(inner_frame, text="ไม่มีการจอง", font=("Arial", 30), text_color="black")
        no_booking_label.pack(pady=20, expand=True)
    else:
        for booking in bookings:
            booking_id, username, room_number, price, check_in_date, check_out_date, guest_count, hotel_name = booking

            booking_info = (
                f"ID: {booking_id}, ผู้ใช้: {username}, โรงแรม: {hotel_name}, "
                f"ห้อง: {room_number}, ราคา: {price} THB, เช็คอิน: {check_in_date}, "
                f"เช็คเอาต์: {check_out_date}, จำนวนผู้เข้าพัก: {guest_count}"
            )
            info_label = ctk.CTkLabel(inner_frame, text=booking_info, font=("Arial", 20), text_color="black", anchor="w")
            info_label.pack(pady=10, padx=10, fill="x")

            def cancel_booking(bid):
                confirm = messagebox.askyesno("ยกเลิกการจอง", "คุณต้องการยกเลิกการจองนี้หรือไม่?")
                if confirm:
                    c.execute("DELETE FROM bookings WHERE id = ?", (bid,))
                    conn.commit()
                    messagebox.showinfo("ยกเลิกสำเร็จ", f"การจอง {bid} ถูกยกเลิกเรียบร้อยแล้ว")
                    bookings_window.destroy()
                    admin_view_bookings(admin_window, username)

            cancel_button = ctk.CTkButton(inner_frame, text="ยกเลิกการจอง", font=("Arial", 16),
                                          command=lambda bid=booking_id: cancel_booking(bid),
                                          corner_radius=10, fg_color="red")
            cancel_button.pack(pady=5)

    def close_admin_view_bookings():
        bookings_window.destroy()
        admin_window.deiconify()

    close_button = ctk.CTkButton(bookings_window, text="ปิด", command=close_admin_view_bookings,
                                 font=("Arial", 18), corner_radius=10, fg_color="#f44336", text_color="white")
    close_button.place(relx=0.9, rely=0.95, anchor="center") 

def open_add_hotel_window():
    add_window = ctk.CTkToplevel()
    add_window.title("เพิ่มโรงแรมใหม่")

    window_width = 1250
    window_height = 750
    add_window.geometry(f"{window_width}x{window_height}")

    form_frame = ctk.CTkScrollableFrame(add_window)  
    form_frame.pack(side="left", fill="both", expand=True, padx=10, pady=10)

    image_frame = ctk.CTkFrame(add_window)
    image_frame.pack(side="right", fill="both", expand=True, padx=10, pady=10)

    hotel_name_label = ctk.CTkLabel(form_frame, text="ชื่อโรงแรม", font=("Arial", 22))
    hotel_name_label.pack(pady=5)
    hotel_name_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=200) 
    hotel_name_entry.pack(pady=5)

    location_label = ctk.CTkLabel(form_frame, text="ที่ตั้งโรงแรม", font=("Arial", 22))
    location_label.pack(pady=5)
    location_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400) 
    location_entry.pack(pady=5)

    price_label = ctk.CTkLabel(form_frame, text="ราคา", font=("Arial", 22))
    price_label.pack(pady=5)
    price_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400)  
    price_entry.pack(pady=5)

    image_label = ctk.CTkLabel(form_frame, text="เลือกรูปภาพโรงแรม", font=("Arial", 22))
    image_label.pack(pady=5)

    image_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400) 
    image_entry.pack(pady=5)

    def select_image():
        file_path = filedialog.askopenfilename(title="เลือกไฟล์ภาพ", filetypes=[("Image Files", "*.png;*.jpg;*.jpeg;*.bmp")])
        if file_path:
            image_entry.delete(0, 'end')
            image_entry.insert(0, file_path)

            img = Image.open(file_path)

            max_size = (500,500)
            img = img.resize(max_size, Image.Resampling.LANCZOS)

            img_ctk = CTkImage(img, size=max_size)

            if hasattr(select_image, "image_label"):
                select_image.image_label.configure(image=img_ctk)
            else:
                select_image.image_label = ctk.CTkLabel(image_frame, image=img_ctk, width=max_size[0], height=max_size[1])
                select_image.image_label.pack(pady=10)

            select_image.image = img_ctk

    select_image_button = ctk.CTkButton(form_frame, text="เลือกภาพ", command=select_image, font=("Arial", 22))
    select_image_button.pack(pady=5)

    email_label = ctk.CTkLabel(form_frame, text="อีเมล", font=("Arial", 22))
    email_label.pack(pady=5)
    email_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400) 
    email_entry.pack(pady=5)

    facebook_label = ctk.CTkLabel(form_frame, text="Facebook", font=("Arial", 22))
    facebook_label.pack(pady=5)
    facebook_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400) 
    facebook_entry.pack(pady=5)

    instagram_label = ctk.CTkLabel(form_frame, text="Instagram", font=("Arial", 22))
    instagram_label.pack(pady=5)
    instagram_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400)
    instagram_entry.pack(pady=5)

    phone_label = ctk.CTkLabel(form_frame, text="เบอร์โทร", font=("Arial", 22))
    phone_label.pack(pady=5)
    phone_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400)
    phone_entry.pack(pady=5)

    amenities_label = ctk.CTkLabel(form_frame, text="สิ่งอำนวยความสะดวก", font=("Arial", 22))
    amenities_label.pack(pady=5)
    amenities_entry = ctk.CTkEntry(form_frame, font=("Arial", 22), width=400)
    amenities_entry.pack(pady=5)

    room_frame = ctk.CTkScrollableFrame(form_frame)
    room_frame.pack(expand=True, fill="both", pady=20)

    room_entries = []

    def add_room_fields():
        room_number_label = ctk.CTkLabel(room_frame, text="หมายเลขห้องพัก", font=("Arial", 22))
        room_number_label.pack(pady=5)
        room_number_entry = ctk.CTkEntry(room_frame, font=("Arial", 22))
        room_number_entry.pack(pady=5)

        status_label = ctk.CTkLabel(room_frame, text="สถานะห้องพัก", font=("Arial", 22))
        status_label.pack(pady=5)
        status_option = ctk.CTkComboBox(room_frame, values=["จองแล้ว", "ยังไม่จอง"], font=("Arial", 22))
        status_option.set("ยังไม่จอง")
        status_option.pack(pady=5)

        room_entries.append((room_number_entry, status_option))

    add_room_button = ctk.CTkButton(form_frame, text="เพิ่มห้องพัก", command=add_room_fields, font=("Arial", 22))
    add_room_button.pack(pady=10)

    def save_hotel():
        hotel_name = hotel_name_entry.get()
        location = location_entry.get()
        price = float(price_entry.get())
        image_path = image_entry.get()
        email = email_entry.get()
        facebook = facebook_entry.get()
        instagram = instagram_entry.get()
        phone = phone_entry.get()
        amenities = amenities_entry.get()  

        if not hotel_name or not location or not price or not image_path:
            messagebox.showerror("ข้อมูลไม่ครบ", "กรุณากรอกข้อมูลให้ครบทุกช่อง")
            return

        c.execute(''' 
            INSERT INTO hotels (name, location, price, image, email, facebook, instagram, phone) 
            VALUES (?, ?, ?, ?, ?, ?, ?, ?)
        ''', (hotel_name, location, price, image_path, email, facebook, instagram, phone))
        conn.commit()

        # ดึง id ของโรงแรมที่บันทึกล่าสุด
        hotel_id = c.lastrowid  # ดึง ID ของโรงแรมที่บันทึกใหม่
        if hotel_id:
            if amenities:
                c.execute(''' 
                    INSERT INTO amenities (hotel_id, amenity) 
                    VALUES (?, ?)
                ''', (hotel_id, amenities))
                conn.commit()

            messagebox.showinfo("สำเร็จ", "โรงแรมถูกบันทึกเรียบร้อยแล้ว!")
        else:
            messagebox.showerror("ข้อผิดพลาด", "ไม่สามารถบันทึกโรงแรมได้")

        add_room_button.configure(state="normal")
        save_room_button.configure(state="normal")


    def save_room():
        hotel_name = hotel_name_entry.get()

        c.execute("SELECT hotel_id FROM hotels WHERE name=?", (hotel_name,))
        hotel = c.fetchone()

        if hotel is None:
            messagebox.showerror("ข้อผิดพลาด", "ไม่พบโรงแรมที่ระบุ กรุณาบันทึกข้อมูลโรงแรมก่อน")
            return

        hotel_id = hotel[0]

        for room_number_entry, status_option in room_entries:
            room_number = room_number_entry.get()
            status = status_option.get()

            if not room_number:
                messagebox.showerror("ข้อผิดพลาด", "กรุณากรอกหมายเลขห้องพัก")
                return

            c.execute(''' 
                INSERT INTO available_rooms (hotel_id, room_number, status) 
                VALUES (?, ?, ?)
            ''', (hotel_id, room_number, status))
        conn.commit()

        messagebox.showinfo("สำเร็จ", "ห้องพักถูกบันทึกเรียบร้อยแล้ว!")

    save_button = ctk.CTkButton(form_frame, text="บันทึกโรงแรม", command=save_hotel, font=("Arial", 22))
    save_button.pack(pady=10)

    save_room_button = ctk.CTkButton(form_frame, text="บันทึกห้องพัก", command=save_room, font=("Arial", 22))
    save_room_button.pack(pady=10)

    add_window.mainloop()
 
def hide_hotel_window():
    manage_window = ctk.CTkToplevel()
    manage_window.title("Manage Hotels")
    manage_window.geometry("800x600")

    c.execute("PRAGMA table_info(hotels)")
    columns = [column[1] for column in c.fetchall()]
    if 'status' not in columns:
        c.execute("ALTER TABLE hotels ADD COLUMN status TEXT DEFAULT 'active'")
        conn.commit()

    def load_hotels():
        c.execute("SELECT hotel_id, name, status FROM hotels")
        return c.fetchall()

    def refresh_hotel_list():
        hotel_list.delete(0, ctk.END)
        hotels = load_hotels()
        for hotel in hotels:
            status = f" ({hotel[2]})"
            if hotel[2] == 'not active':
                hotel_list.insert(ctk.END, f"{hotel[0]} - {hotel[1]}{status}")
                hotel_list.itemconfig(hotel_list.size() - 1, {'fg': 'red'})  # เปลี่ยนสีตัวอักษรเป็นแดง
            else:
                hotel_list.insert(ctk.END, f"{hotel[0]} - {hotel[1]}{status}")

    def hide_selected_hotels():
        selected_indices = hotel_list.curselection()
        if not selected_indices:
            messagebox.showerror("Error", "กรุณาเลือกโรงแรมก่อนซ่อน")
            return
        try:
            hotels = load_hotels()
            for index in selected_indices:
                hotel_id = hotels[index][0]
                c.execute("UPDATE hotels SET status = 'not active' WHERE hotel_id = ?", (hotel_id,))
            conn.commit()
            messagebox.showinfo("Success", "ซ่อนโรงแรมเรียบร้อยแล้ว")
            refresh_hotel_list()
        except Exception as e:
            messagebox.showerror("Error", f"เกิดข้อผิดพลาด: {e}")

    def unhide_selected_hotels():
        selected_indices = hotel_list.curselection()
        if not selected_indices:
            messagebox.showerror("Error", "กรุณาเลือกโรงแรมก่อนแสดง")
            return
        try:
            hotels = load_hotels()
            for index in selected_indices:
                hotel_id = hotels[index][0]
                c.execute("UPDATE hotels SET status = 'active' WHERE hotel_id = ?", (hotel_id,))
            conn.commit()
            messagebox.showinfo("Success", "แสดงโรงแรมเรียบร้อยแล้ว")
            refresh_hotel_list()
        except Exception as e:
            messagebox.showerror("Error", f"เกิดข้อผิดพลาด: {e}")

    def select_all():
        hotel_list.select_set(0, ctk.END)

    frame_main = ctk.CTkFrame(manage_window)
    frame_main.pack(expand=True, fill="both", padx=20, pady=20)

    lbl_title = ctk.CTkLabel(frame_main, text="Select Hotels to Hide/Unhide", font=("Arial", 20))
    lbl_title.pack(pady=10)

    hotel_list = Listbox(frame_main, font=("Arial", 16), height=20, selectmode="multiple")
    hotel_list.pack(pady=10, fill="both", expand=True)

    btn_select_all = ctk.CTkButton(frame_main, text="Select All", command=select_all)
    btn_select_all.pack(pady=5)

    btn_hide = ctk.CTkButton(frame_main, text="Hide Selected Hotels", command=hide_selected_hotels)
    btn_hide.pack(pady=5)

    btn_unhide = ctk.CTkButton(frame_main, text="Unhide Selected Hotels", command=unhide_selected_hotels)
    btn_unhide.pack(pady=5)

    btn_return = ctk.CTkButton(frame_main, text="Back to Manage Hotels", command=manage_window.destroy)
    btn_return.pack(pady=10)

    refresh_hotel_list()

def open_edit_hotel_window():
    Edit_window = ctk.CTkToplevel()
    Edit_window.title("Edit Hotel")
    Edit_window.attributes('-fullscreen', True)

    def load_hotels():
        c.execute("SELECT hotel_id, name, details, location, price, image FROM hotels")
        return c.fetchall()

    def load_amenities(hotel_id):
        c.execute("SELECT amenity FROM amenities WHERE hotel_id = ?", (hotel_id,))
        return [amenity[0] for amenity in c.fetchall()]
    
    def save_changes():
        hotel_id = entry_id.get()
        name = entry_name.get()
        details = entry_details.get()
        location = entry_location.get()
        price = entry_price.get()
        image = entry_image.get()

        if not hotel_id or not name or not location or not price:
            messagebox.showerror("Error", "กรุณากรอกข้อมูลให้ครบถ้วน")
            return

        try:
            price = float(price)

            c.execute('''UPDATE hotels
                        SET name = ?, details = ?, location = ?, price = ?, image = ?
                        WHERE hotel_id = ?''', (name, details, location, price, image, hotel_id))
            conn.commit()

            amenities_list = []
            for amenity in amenities_text.get("1.0", ctk.END).splitlines():
                if amenity.strip():
                    amenities_list.append(amenity.strip())

            c.execute('DELETE FROM amenities WHERE hotel_id = ?', (hotel_id,))
            for amenity in amenities_list:
                c.execute('INSERT INTO amenities (hotel_id, amenity) VALUES (?, ?)', (hotel_id, amenity))
            conn.commit()

            rooms_list = []
            for room in rooms_text.get("1.0", ctk.END).splitlines():
                if room.strip():
                    room_number, status = room.split(",")
                    rooms_list.append((room_number.strip(), status.strip()))

            for room_number, status in rooms_list:
                c.execute('''SELECT * FROM available_rooms 
                            WHERE hotel_id = ? AND room_number = ?''', (hotel_id, room_number))
                existing_room = c.fetchone()

                if existing_room:
                    c.execute('''UPDATE available_rooms
                                SET status = ?
                                WHERE hotel_id = ? AND room_number = ?''', (status, hotel_id, room_number))
                else:
                    c.execute('''INSERT INTO available_rooms (hotel_id, room_number, status)
                                VALUES (?, ?, ?)''', (hotel_id, room_number, status))
            conn.commit()

            messagebox.showinfo("Success", "แก้ไขข้อมูลโรงแรมเรียบร้อยแล้ว")
            refresh_hotel_list()

        except Exception as e:
            messagebox.showerror("Error", f"เกิดข้อผิดพลาด: {e}")


    def refresh_hotel_list():
        hotel_list.delete(1.0, ctk.END)  
        hotels = load_hotels()
        for index, hotel in enumerate(hotels, start=1):
            hotel_list.insert(ctk.END, f"{hotel[0]}. {hotel[1]}\n")  

    def select_hotel(event):
        try:
            selected_text = hotel_list.get("1.0", ctk.END).strip().split('\n')
            selected_line = hotel_list.index(f"@{event.x},{event.y}")
            line_number = int(selected_line.split('.')[0]) - 1

            if line_number < 0 or line_number >= len(selected_text):
                raise ValueError("ไม่พบข้อมูลโรงแรมที่เลือก")

            selected_data = selected_text[line_number]
            selected_id = selected_data.split('.')[0] 

            # ค้นหาในฐานข้อมูล
            c.execute("SELECT * FROM hotels WHERE hotel_id = ?", (selected_id,))
            hotel = c.fetchone()
            if hotel:
                entry_id.delete(0, ctk.END)
                entry_id.insert(0, hotel[0] if hotel[0] else "")  
                entry_name.delete(0, ctk.END)
                entry_name.insert(0, hotel[1] if hotel[1] else "")  
                entry_details.delete(0, ctk.END)
                entry_details.insert(0, hotel[2] if hotel[2] else "") 
                entry_location.delete(0, ctk.END)
                entry_location.insert(0, hotel[3] if hotel[3] else "") 
                entry_price.delete(0, ctk.END)
                entry_price.insert(0, hotel[4] if hotel[4] else "")  
                entry_image.delete(0, ctk.END)
                entry_image.insert(0, hotel[5] if hotel[5] else "") 

                amenities_list = load_amenities(hotel[0])  
                amenities_text.delete(1.0, ctk.END) 
                for amenity in amenities_list:
                    amenities_text.insert(ctk.END, f"{amenity}\n")

                c.execute("SELECT room_number, status FROM available_rooms WHERE hotel_id = ?", (hotel[0],))
                rooms = c.fetchall()
                rooms_text.delete(1.0, ctk.END)  
                for room in rooms:
                    rooms_text.insert(ctk.END, f"{room[0]}, {room[1]}\n")

            else:
                messagebox.showerror("Error", "ไม่พบข้อมูลโรงแรมในฐานข้อมูล")
        except Exception as e:
            messagebox.showerror("Error", f"เกิดข้อผิดพลาด: {e}")

    def return_to_manage():
        Edit_window.destroy()

    frame_left = ctk.CTkFrame(Edit_window)
    frame_left.grid(row=0, column=0, sticky="ns", padx=20, pady=20)

    frame_right = ctk.CTkFrame(Edit_window)
    frame_right.grid(row=0, column=1, sticky="nsew", padx=20, pady=20)

    Edit_window.grid_rowconfigure(0, weight=1)
    Edit_window.grid_columnconfigure(1, weight=1)

    hotel_list = ctk.CTkTextbox(frame_left, font=("Arial", 20), width=500, height=400) 
    hotel_list.grid(row=0, column=0, sticky="nsew")

    hotel_list.bind("<ButtonRelease-1>", select_hotel)

    lbl_id = ctk.CTkLabel(frame_right, text="Hotel ID")
    lbl_id.grid(row=0, column=0, sticky="w")
    entry_id = ctk.CTkEntry(frame_right, font=("Arial", 14), width=500)  
    entry_id.grid(row=1, column=0, sticky="ew", padx=10, pady=5)

    lbl_name = ctk.CTkLabel(frame_right, text="Hotel Name")
    lbl_name.grid(row=2, column=0, sticky="w")
    entry_name = ctk.CTkEntry(frame_right, font=("Arial", 14), width=500)
    entry_name.grid(row=3, column=0, sticky="ew", padx=10, pady=5)

    lbl_details = ctk.CTkLabel(frame_right, text="Details")
    lbl_details.grid(row=4, column=0, sticky="w")
    entry_details = ctk.CTkEntry(frame_right, font=("Arial", 14), width=500)
    entry_details.grid(row=5, column=0, sticky="ew", padx=10, pady=5)

    lbl_location = ctk.CTkLabel(frame_right, text="Location")
    lbl_location.grid(row=6, column=0, sticky="w")
    entry_location = ctk.CTkEntry(frame_right, font=("Arial", 14), width=500)
    entry_location.grid(row=7, column=0, sticky="ew", padx=10, pady=5)

    lbl_price = ctk.CTkLabel(frame_right, text="Price")
    lbl_price.grid(row=8, column=0, sticky="w")
    entry_price = ctk.CTkEntry(frame_right, font=("Arial", 14), width=500)
    entry_price.grid(row=9, column=0, sticky="ew", padx=10, pady=5)

    lbl_image = ctk.CTkLabel(frame_right, text="Image")
    lbl_image.grid(row=10, column=0, sticky="w")
    entry_image = ctk.CTkEntry(frame_right, font=("Arial", 14), width=500)
    entry_image.grid(row=11, column=0, sticky="ew", padx=10, pady=5)

    lbl_amenities = ctk.CTkLabel(frame_right, text="Amenities")
    lbl_amenities.grid(row=12, column=0, sticky="w")
    amenities_text = ctk.CTkTextbox(frame_right, font=("Arial", 14), width=500, height=100)
    amenities_text.grid(row=13, column=0, sticky="nsew", padx=10, pady=5)

    lbl_rooms = ctk.CTkLabel(frame_right, text="Rooms (Room Number, Status)")
    lbl_rooms.grid(row=14, column=0, sticky="w")
    rooms_text = ctk.CTkTextbox(frame_right, font=("Arial", 14), width=500, height=100)
    rooms_text.grid(row=15, column=0, sticky="nsew", padx=10, pady=5)

    btn_save = ctk.CTkButton(frame_right, text="Save Changes", command=save_changes)
    btn_save.grid(row=16, column=0, pady=10)

    btn_return = ctk.CTkButton(frame_right, text="Back to Manage Hotels", command=return_to_manage)
    btn_return.grid(row=17, column=0, pady=10)

    refresh_hotel_list()

def show_user_details():
    def delete_user(username):

        if messagebox.askyesno("ยืนยันการลบ", f"คุณต้องการลบผู้ใช้ '{username}' หรือไม่?"):
            conn = sqlite3.connect('hotels_All_Data.db')
            c = conn.cursor()
            c.execute("DELETE FROM users WHERE username = ?", (username,))
            conn.commit()
            conn.close()
            messagebox.showinfo("สำเร็จ", f"ลบผู้ใช้ '{username}' เรียบร้อยแล้ว")
            user_detail_window.destroy()  # ปิดหน้าต่างแล้วเปิดใหม่
            show_user_details()

    def show_user_info(username):
        conn = sqlite3.connect('hotels_All_Data.db')
        c = conn.cursor()
        c.execute("SELECT username, first_name, last_name, phone, email FROM users WHERE username = ?", (username,))
        user = c.fetchone()
        conn.close()

        if user:
            info_window = ctk.CTkToplevel()
            info_window.title("ข้อมูลผู้ใช้")
            info_window.geometry("400x300")

            username, first_name, last_name, phone, email = user
            ctk.CTkLabel(info_window, text=f"ชื่อผู้ใช้: {username}", font=("Arial", 14)).pack(pady=10)
            ctk.CTkLabel(info_window, text=f"ชื่อจริง: {first_name}", font=("Arial", 14)).pack(pady=10)
            ctk.CTkLabel(info_window, text=f"นามสกุล: {last_name}", font=("Arial", 14)).pack(pady=10)
            ctk.CTkLabel(info_window, text=f"เบอร์โทร: {phone}", font=("Arial", 14)).pack(pady=10)
            ctk.CTkLabel(info_window, text=f"อีเมล: {email}", font=("Arial", 14)).pack(pady=10)

            ctk.CTkButton(info_window, text="ลบบัญชีผู้ใช้", command=lambda: delete_user(username), fg_color="red").pack(pady=20)

    user_detail_window = ctk.CTkToplevel()
    user_detail_window.title("รายชื่อผู้ใช้ทั้งหมด")
    user_detail_window.geometry("600x400")

    conn = sqlite3.connect('hotels_All_Data.db')
    c = conn.cursor()

    c.execute("SELECT username FROM users WHERE username != 'admin'")
    users = c.fetchall()
    conn.close()

    frame = ctk.CTkScrollableFrame(user_detail_window, width=550, height=350)
    frame.pack(pady=20, padx=20, fill="both", expand=True)

    for user in users:
        username = user[0]
        ctk.CTkButton(frame, text=username, font=("Arial", 14), command=lambda u=username: show_user_info(u)).pack(pady=5, fill="x")

exit_confirmed = False
add_admin_account()
open_login_window()
