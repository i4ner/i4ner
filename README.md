import sqlite3

conn = sqlite3.connect("MyContacts.db")
cursor = conn.cursor()


cursor.execute('''
    CREATE TABLE IF NOT EXISTS Contacts (
        id INTERGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        phone_number TEXT,
        email TEXT,
        money TEXT
    );
''')


contacts_data = [
    ('John Doe', '+123456789','john.doe@gmail.ru','60000')
    ('Jane Smith', '+198764543','jane.smith@gmail.ru','40000')
    ('Michael Johnson', '+176905234','michael.johnson@gmail.ru','90000')
    ('Emily Brown', '+156478391','emily.brown@gmail.ru','50000')
    ('David Lee', '+101928374','david.lee@gmail.ru','30000')
]


cursor.executemany('TNSERT INTO Cintacts (name,  phone_number, email, money) VALUES (?,?,?)',
                   contacts_data)

conn.commit()


cursor.execute('SELECT * FROM Contacts')
all_contacts = cursor.fetchall()
print("ALL contacts:")
for contacts in all_contacts:
    print(contacts)

cursor.execute('SELECT name, phone_number FROM Contacts WHERE phone_number LTKE "+1"')
contacts_with_country_code = cursor.fetchall()
print('\nContacts with country code +1:')
for contacts in contacts_with_country_code:
    print(contacts)

cursor.execute('SELECT * FROM Contacts WHERE email LTKE "+gmail"')
gmail_contacts = cursor.fetchall()
print('\nContacts with Gmail email:')
for contacts in gmail_contacts:
    print(contacts)

cursor.execute('SELECT * FROM Contacts WHERE money LTKE "+00000"')
money_contacts = cursor.fetchall()
print('\nContacts with money:')
for contacts in money_contacts:
    print(contacts)


update_id = 1
new_name = "Update Name"
new_phone_number = "+999999999"
cursor.execute('"Update Contacts SET name = ?, phone_number = ? WHERE id = ?',
               new_name, new_phone_number, update_id)
conn.commit()


delete_id = 3
cursor.execute('DELETE FROM Contacts WHERE id = ?', (delete_id,))
conn.commit()


conn.close()
