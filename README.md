PYTHON PYQT5 + DATABASE CONNECTION ASSIGNMENT

Tools Used
    • Azure Data studio(SSMS Alternative for Linux)
    • Pyqt5 tools
    • Jupyter Lab
    • Designer ToolKit
    • Pyuic5

STUDENT INFORMATION MANAGEMENT SYSTEM
The Student Information Management System is a desktop application built using PyQt5 for the graphical interface and SQL Server for the database backend. It allows the user to perform CRUD operations (Create, Read, Update, Delete) on student records, search for specific students, and navigate through records. The system also supports resetting the database and ensures proper validation of input fields.

PROCEDURE USED
1. Creation of the student database via Azure Data Studio
2. Via the Designer QT interface, I managed to do proper UI building with elements like...textboxes, radio buttons, line text and even labels
3. Conversion to .py file using Pyuic5 python package 
4. Database Connection using odbc and password authentication
5. Main Python file scripting and testing, with adding functionalities to elements in the table 
6. Final usability test and error debugging

1. Database Creation
This procedure included the creation of the database system that is to be connected to our frontend, the database schema use MYSQL and was quite simple to create through the following lines of code via the azure data studio system:
-- Create Database
CREATE DATABASE University;

-- Use the Database
USE University;

-- Create Students Table
CREATE TABLE Students (
RegistrationNumber VARCHAR(20) PRIMARY KEY,
FirstName VARCHAR(50) NOT NULL,
SecondName VARCHAR(50),
Surname VARCHAR(50) NOT NULL,
Course VARCHAR(100) NOT NULL,
County VARCHAR(50),
Gender VARCHAR(10),
Department VARCHAR(100),
School VARCHAR(100)
);

INSERT INTO Students VALUES
('IN17/00001/23', 'Brian', 'Otieno', 'Omondi', 'Computer Science', 'Kisumu', 'Male', 'ICT', 'School of Computing'),
('IN17/00002/23', 'Faith', 'Wanjiku', 'Mwangi', 'Information Technology', 'Kiambu', 'Female', 'ICT', 'School of Computing'),
('IN17/00003/23', 'Kevin', 'Kiptoo', 'Cheruiyot', 'Software Engineering', 'Uasin Gishu', 'Male', 'ICT', 'School of Computing'),
('IN17/00004/23', 'Mercy', 'Achieng', 'Odhiambo', 'Business Administration', 'Homa Bay', 'Female', 'Business', 'School of Business'),
('IN17/00005/23', 'Dennis', 'Mutua', 'Musyoka', 'Accounting', 'Machakos', 'Male', 'Finance', 'School of Business'),
('IN17/00006/23', 'Esther', 'Naliaka', 'Wekesa', 'Nursing', 'Bungoma', 'Female', 'Health Sciences', 'School of Health'),
('IN17/00007/23', 'James', 'Kamau', 'Njoroge', 'Civil Engineering', 'Nyeri', 'Male', 'Engineering', 'School of Engineering'),
('IN17/00008/23', 'Lilian', 'Chebet', 'Koech', 'Education Arts', 'Kericho', 'Female', 'Education', 'School of Education'),
('IN17/00009/23', 'Peter', 'Maina', 'Kariuki', 'Economics', 'Murang’a', 'Male', 'Economics', 'School of Arts'),
('IN17/00010/23', 'Ann', 'Njeri', 'Wambui', 'Law', 'Nairobi', 'Female', 'Law', 'School of Law');

SELECT * FROM Students;

The following shows an image of the work done as well:
This shows our schema and our created datbase with name univeirsty which we will connect to our frontend using pypyodbc in the future procedures 

The picture below shows our table:
We currently have only ten records and we will make more changes via our interface created through designer qt


2. UI Design Phase
This was an interactive phase where we build the user interface step by step, one element at a time to get our complete user interface in place, the procedure is showm via the images below:

fig 1. This stage involved adding the text labels for our sections such as the Registration Number, First Name and similar details to act as a display of where what is in the system


fig 2. Involved giving the labels their appropriate naming conventions for easier identification

fig 3. involved adding the line texts to our UI to act as holding blocks for our data imported straight from our University database






fig 4. this phase involved addig buttons that we will add various python functions to for usability


The final out look of the UI system is as so:
With an added RESET DATABASE Button and a SEARCH tab as well

3. File Conversion from .UI to .PY
This stage involved converting our .ui file to a .py file, the .py script can easily display our UI when ran as a jupyter lab .ipynb file(a notebook)
I saved the file as student_information.ui and I used the pyuic5 package for the conversion with the following script on my terminal
pyuic5 student_information.ui -o student_information.py

This Creates a .py script that shows our UI file in code form...we will use this script to create functions for the buttons and lables in the following procedures to improve our UI

4. Database Connection and Authentication
This is done through pypyodbc and the package can be installed via the following command
pip install pypyodbc

The script is as follows:

# ------------------------ DATABASE CONFIG ------------------------
DRIVER_NAME = "ODBC Driver 18 for SQL Server"
SERVER_NAME = "localhost"
DATABASE_NAME = "University"
USERNAME = "SA"
PASSWORD = "Lenashalewis!"

connection_string = f"""
    DRIVER={{{DRIVER_NAME}}};
    SERVER={SERVER_NAME};
    DATABASE={DATABASE_NAME};
    UID={USERNAME};
    PWD={PASSWORD};
    TrustServerCertificate=yes;
"""

def connect_db():
    return odbc.connect(connection_string)


Password authentication was mandatory as I run a protected database system on my linux OS,
 we are running under an ODBC Driver 18 for SQL Server
Also includes among other details such as our database name, our username and autenticatiion details stored within a connection string variable

5. Main Python File
This is where the magic happens we have procedures such as...button scripting, adding functions, importing the .py file from earlier in the proccess as well….we will also add reset database functionalities and carry out testing for caplaities and debug various issues

  
fig 5. 


This section includes a piece of code that creates a new class that inherits from the QTWidgets main window...we have also defined a constructor and various button connections maped to functions that we will display in the code later on
fig 6.

Function: display_record
Populates the GUI with data from a single record (row) retrieved from our database 
Parameters
    • row (list): Contains values in the order [RegistrationNumber, FirstName, SecondName, Surname, Course, County, Gender, Department, School].
Behavior
    1. Text Fields
        ◦ Maps each element of row to a corresponding QLineEdit widget:
            ▪ row[0] → Registration Number
            ▪ row[1] → First Name
            ▪ row[2] → Second Name
            ▪ row[3] → Surname
            ▪ row[4] → Course
            ▪ row[5] → County
            ▪ row[7] → Department
            ▪ row[8] → School
    2. Gender Selection
        ◦ Reads row[6] (Gender) and sets the appropriate radio button:
            ▪ "Male" → radioButton checked
            ▪ "Female" → radioButton_2 checked
            ▪ Any other value → radioButton_3 checked
        ◦ Ensures only one radio button is active at a time.

Fig 7.
add_record()
Purpose: Adds a new student record to the database.
Behavior:
    • Validates input before proceeding.
    • Executes an SQL INSERT INTO Students statement using values from the GUI fields.
    • Commits changes and reloads records.
    • Displays success or error messages via QmessageBox.

Fig 8.
update_record()
Purpose: Updates an existing student record in the database.
Behavior:
    • Validates input before proceeding.
    • Executes an SQL UPDATE Students SET ... WHERE RegistrationNumber=?.
    • Commits changes and reloads records.
    • Displays success or error messages.

Fig 9.
 delete_record()
Purpose: Deletes a student record from the database based on the registration number.
Behavior:
    • Prompts the user for confirmation.
    • Executes an SQL DELETE FROM Students WHERE RegistrationNumber=?.
    • Commits changes and reloads records.
    • Displays

fig 10.
next_record() / prev_record()
Purpose: Navigates through the list of loaded records.
Behavior:
    • next_record() moves forward if another record exists.
    • prev_record() moves backward if a previous record exists.
    • Calls display_record() to update the GUI with the current record.
clear_fields()
Purpose: Resets all input fields and radio buttons in the GUI.
Behavior:
    • Clears text from all QLineEdit widgets.
    • Unchecks gender radio buttons.
Fig 11.
get_gender()
Purpose: Determines the selected gender from the radio buttons.
Behavior:
    • Returns "Male" if radioButton is checked.
    • Returns "Female" if radioButton_2 is checked.
    • Returns "Other" if neither is selected.
validate()
Purpose: Ensures required fields are filled before database operations.
Behavior:
    • Checks that the following fields are not empty:
        ◦ Registration Number (lineEdit)
        ◦ First Name (lineEdit_2)
        ◦ Second Name (lineEdit_3)
        ◦ Surname (lineEdit_4)
    • Displays a warning message if any required field is missing.
    • Returns True if all validations pass, otherwise False.




reset_db()
Purpose: Clears all records from the Students database table.
Behavior:
    • Prompts the user for confirmation before proceeding.
    • Executes an SQL DELETE FROM Students statement without conditions (removes all rows).
    • Commits changes and reloads records.
    • Displays confirmation or error messages.
Fig 12.




fig 13.

search_record()
Purpose: Finds and displays a student record based on user input.
Behavior:
    • Retrieves the search value from lineEdit_9.
    • Executes an SQL query to match either:
        ◦ RegistrationNumber = ?
        ◦ (FirstName + ' ' + SecondName) = ?
    • If a record is found:
        ◦ Calls display_record(row) to show the data in the GUI.
        ◦ Displays a success message.
    • If no record is found:
        ◦ Displays a warning message.
FINAL FULL FILE
import sys
import pypyodbc as odbc
from PyQt5 import QtWidgets
from student_information import Ui_MainWindow  


# ------------------------ DATABASE CONFIG ------------------------
DRIVER_NAME = "ODBC Driver 18 for SQL Server"
SERVER_NAME = "localhost"
DATABASE_NAME = "University"
USERNAME = "SA"
PASSWORD = "Lenashalewis!"

connection_string = f"""
    DRIVER={{{DRIVER_NAME}}};
    SERVER={SERVER_NAME};
    DATABASE={DATABASE_NAME};
    UID={USERNAME};
    PWD={PASSWORD};
    TrustServerCertificate=yes;
"""

def connect_db():
    return odbc.connect(connection_string)


# ------------------------ MAIN APPLICATION ------------------------
class StudentApp(QtWidgets.QMainWindow):
    def __init__(self):
        super().__init__()
        self.ui = Ui_MainWindow()
        self.ui.setupUi(self)

        self.records = []
        self.current_index = -1

        # Button Connections
        self.ui.pushButton.clicked.connect(self.add_record)      # ADD
        self.ui.pushButton_6.clicked.connect(self.update_record) # UPDATE
        self.ui.pushButton_4.clicked.connect(self.delete_record) # DELETE
        self.ui.pushButton_5.clicked.connect(self.clear_fields)  # CANCEL
        self.ui.pushButton_7.clicked.connect(self.reset_db)      # RESET
        self.ui.pushButton_2.clicked.connect(self.next_record)   # NEXT
        self.ui.pushButton_3.clicked.connect(self.prev_record)   # PREV

        # SEARCH using text field (press Enter)
        self.ui.lineEdit_9.returnPressed.connect(self.search_record)

        self.load_records()
        if self.records:
            self.display_record(self.records[0])

    # ------------------------ LOAD RECORDS ------------------------
    def load_records(self):
        conn = connect_db()
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM Students")
        self.records = cursor.fetchall()
        self.current_index = 0 if self.records else -1

    # ------------------------ DISPLAY RECORD ------------------------
    def display_record(self, row):
        # RegistrationNumber, FirstName, SecondName, Surname, Course, County, Gender, Department, School
        self.ui.lineEdit.setText(row[0])   # Reg No
        self.ui.lineEdit_2.setText(row[1]) # First Name
        self.ui.lineEdit_3.setText(row[2]) # Second Name
        self.ui.lineEdit_4.setText(row[3]) # Surname
        self.ui.lineEdit_7.setText(row[4]) # Course
        self.ui.lineEdit_6.setText(row[5]) # County
        self.ui.lineEdit_5.setText(row[7]) # Department
        self.ui.lineEdit_8.setText(row[8]) # School

        # Gender
        gender = row[6].strip() 
        if gender == "Male":
            self.ui.radioButton.setChecked(True)
            self.ui.radioButton_2.setChecked(False)
            self.ui.radioButton_3.setChecked(False)
        elif gender == "Female":
            self.ui.radioButton_2.setChecked(True)
            self.ui.radioButton.setChecked(False)
            self.ui.radioButton_3.setChecked(False)
        else:
            self.ui.radioButton_3.setChecked(True)
            self.ui.radioButton.setChecked(False)
            self.ui.radioButton_2.setChecked(False)

    # ------------------------ GET GENDER ------------------------
    def get_gender(self):
        if self.ui.radioButton.isChecked():
            return "Male"
        elif self.ui.radioButton_2.isChecked():
            return "Female"
        else:
            return "Other"

    # ------------------------ VALIDATION ------------------------
    def validate(self):
        if not self.ui.lineEdit.text():
            QtWidgets.QMessageBox.warning(self, "Error", "Registration Number is required")
            return False
        if not self.ui.lineEdit_2.text():
            QtWidgets.QMessageBox.warning(self, "Error", "First Name is required")
            return False
        if not self.ui.lineEdit_3.text():
            QtWidgets.QMessageBox.warning(self, "Error", "Second Name is required")
            return False
        if not self.ui.lineEdit_4.text():
            QtWidgets.QMessageBox.warning(self, "Error", "Surname is required")
            return False
        return True

    # ------------------------ ADD RECORD ------------------------
    def add_record(self):
        if not self.validate():
            return
        try:
            conn = connect_db()
            cursor = conn.cursor()
            cursor.execute("""
                INSERT INTO Students (RegistrationNumber, FirstName, SecondName, Surname, Course, County, Gender, Department, School)
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)
            """, (
                self.ui.lineEdit.text(),
                self.ui.lineEdit_2.text(),
                self.ui.lineEdit_3.text(),
                self.ui.lineEdit_4.text(),
                self.ui.lineEdit_7.text(),
                self.ui.lineEdit_6.text(),
                self.get_gender(),
                self.ui.lineEdit_5.text(),
                self.ui.lineEdit_8.text()
            ))
            conn.commit()
            QtWidgets.QMessageBox.information(self, "Success", "Record Added")
            self.load_records()
        except Exception as e:
            QtWidgets.QMessageBox.warning(self, "Error", str(e))

    # ------------------------ UPDATE RECORD ------------------------
    def update_record(self):
        if not self.validate():
            return
        try:
            conn = connect_db()
            cursor = conn.cursor()
            cursor.execute("""
                UPDATE Students SET
                FirstName=?, SecondName=?, Surname=?, Course=?, County=?, Gender=?, Department=?, School=?
                WHERE RegistrationNumber=?
            """, (
                self.ui.lineEdit_2.text(),
                self.ui.lineEdit_3.text(),
                self.ui.lineEdit_4.text(),
                self.ui.lineEdit_7.text(),
                self.ui.lineEdit_6.text(),
                self.get_gender(),
                self.ui.lineEdit_5.text(),
                self.ui.lineEdit_8.text(),
                self.ui.lineEdit.text()
            ))
            conn.commit()
            QtWidgets.QMessageBox.information(self, "Updated", "Record Updated")
            self.load_records()
        except Exception as e:
            QtWidgets.QMessageBox.warning(self, "Error", str(e))

    # ------------------------ DELETE ------------------------
    def delete_record(self):
        reply = QtWidgets.QMessageBox.question(
            self, "Confirm", "Delete this record?",
            QtWidgets.QMessageBox.Yes | QtWidgets.QMessageBox.No
        )
        if reply == QtWidgets.QMessageBox.Yes:
            try:
                conn = connect_db()
                cursor = conn.cursor()
                cursor.execute(
                    "DELETE FROM Students WHERE RegistrationNumber=?",
                    self.ui.lineEdit.text()
                )
                conn.commit()
                QtWidgets.QMessageBox.warning(self, "Deleted", "Record Deleted")
                self.load_records()
            except Exception as e:
                QtWidgets.QMessageBox.warning(self, "Error", str(e))

    # ------------------------ SEARCH ------------------------
    def search_record(self):
        try:
            conn = connect_db()
            cursor = conn.cursor()
            search_value = self.ui.lineEdit_9.text()
            cursor.execute("""
                SELECT * FROM Students 
                WHERE RegistrationNumber=? 
                OR (FirstName + ' ' + SecondName)=?
            """, (search_value, search_value))
            row = cursor.fetchone()
            if row:
                self.display_record(row)
                QtWidgets.QMessageBox.information(self, "Found", "Record Found")
            else:
                QtWidgets.QMessageBox.warning(self, "Not Found", "No record found")
        except Exception as e:
            QtWidgets.QMessageBox.warning(self, "Error", str(e))

    # ------------------------ NEXT ------------------------
    def next_record(self):
        if self.records and self.current_index < len(self.records) - 1:
            self.current_index += 1
            self.display_record(self.records[self.current_index])

    # ------------------------ PREVIOUS ------------------------
    def prev_record(self):
        if self.records and self.current_index > 0:
            self.current_index -= 1
            self.display_record(self.records[self.current_index])

    # ------------------------ CLEAR FIELDS ------------------------
    def clear_fields(self):
        self.ui.lineEdit.clear()
        self.ui.lineEdit_2.clear()
        self.ui.lineEdit_3.clear()
        self.ui.lineEdit_4.clear()
        self.ui.lineEdit_5.clear()
        self.ui.lineEdit_6.clear()
        self.ui.lineEdit_7.clear()
        self.ui.lineEdit_8.clear()
        self.ui.lineEdit_9.clear()
        self.ui.radioButton.setChecked(False)
        self.ui.radioButton_2.setChecked(False)
        self.ui.radioButton_3.setChecked(False)

    # ------------------------ RESET DATABASE ------------------------
    def reset_db(self):
        reply = QtWidgets.QMessageBox.question(
            self, "Confirm", "Delete ALL records?",
            QtWidgets.QMessageBox.Yes | QtWidgets.QMessageBox.No
        )
        if reply == QtWidgets.QMessageBox.Yes:
            try:
                conn = connect_db()
                cursor = conn.cursor()
                cursor.execute("DELETE FROM Students")
                conn.commit()
                QtWidgets.QMessageBox.warning(self, "Reset", "Database Cleared")
                self.load_records()
            except Exception as e:
                QtWidgets.QMessageBox.warning(self, "Error", str(e))


# ------------------------ RUN APPLICATION ------------------------
if __name__ == "__main__":
    app = QtWidgets.QApplication(sys.argv)
    window = StudentApp()
    window.show()
    sys.exit(app.exec_())
6.Final testing and Error debugging
This is how our simple, precise and usable final interface looks like
fig 14.

This is when our full python script is run...automatically displays our first record in our database table...let us test some of the functions:

1. Next
Displays the next student’s detials

fig 15.


2. Update

Adds new student record to the table

fig 16.



3. Delete
Deletes a student record from the table


fig 17.


