from tkinter import *
from PIL import ImageTk, Image
import tkinter.messagebox as box
from tkinter import filedialog
import os
import pandas as pd
import mysql.connector as sqltor
import pymysql
from sqlalchemy import create_engine 
import PIL
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
import numpy as np

my_conn = sqltor.connect (host='localhost', user = 'root', passwd = 'trisha123A', database = 'db')
my_conn1= sqltor.connect (host='localhost', user = 'root', passwd = 'trisha123A', database = 'db')
my_conn2 = sqltor.connect(host='localhost', user = 'root', passwd = 'trisha123A', database = 'db')

a= pd.DataFrame(['Name', "ID"])


def dialog1():
    username=entry1.get()
    password = entry2.get()
    if (username == 'admin' and  password == 'adminpass'):
        box.showinfo('info','Correct Login')
        MAIN_WINDOW=Tk()      
        MAIN_WINDOW.configure(bg='deep sky blue')
        MAIN_WINDOW.geometry("600x500+300+100")
        MAIN_WINDOW.title('Hospital Management System')
        l1 = Label(MAIN_WINDOW, text = 'Admin Dashboard', font = ('noto sans cjk tc', 18, 'bold'))
        l1.pack(padx = 100, pady=20)
        
        

        
        def openNewWindow(): 
           newWindow = Toplevel(MAIN_WINDOW) 
  
         
           newWindow.title("New Window") 
            
           newWindow.geometry("200x200") 
           
        # Appointment Program
        def appointments() :
            newWindow=Tk()  
            frame = Frame(newWindow)
            newWindow.title("Appointments")
            newWindow.geometry("600x500+300+100")
            newWindow.configure(bg='Cyan')
            title = Label(newWindow,text = 'Appointment Dashboard', font = ('noto sans cjk tc', 18, 'bold'))
            title.pack(padx = 5, pady = 20)
            title.configure(bg = 'midnight blue', fg = 'white')
            def app():
               def table():
                    a_id = entry1a.get()
                    p_id = entry2a.get()
                    d_id = entry3a.get()
                    date = entry4a.get()
                    j = [{'A_ID':a_id,'P_Id':p_id,'D_Id': d_id,'Date':date}]
                    m = pd.DataFrame(j)
                    print (m)
                    engine = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
                    mycon=engine.connect()
                    m.to_sql('appointments', mycon, index=False, if_exists = 'append')
                    l= Label(newWindow12, text = 'succesfully added the record')
                    l.pack(padx=15)
            
               newWindow12=Tk()  
               frame = Frame(newWindow12)
               newWindow12.title("Add an Appointment")
               newWindow12.geometry("600x500+300+100")
               newWindow12.configure(bg='cyan')
            
            
               frame.pack(pady = 20)
               Label1a = Label(newWindow12,text = 'Appointment ID:', font=('noto sans cjk tc', 10))
               Label1a.pack(padx=15,pady= 5)

               entry1a = Entry(newWindow12, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
               entry1a.pack(padx=15)
            
               Label2a = Label(newWindow12,text = 'Patient ID: ', font=('noto sans cjk tc', 10))
               Label2a.pack(padx = 15,pady=6)

               entry2a = Entry(newWindow12, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
               entry2a.pack(padx = 15,pady=7)

               Label3a = Label(newWindow12, text = 'Doctor ID', font=('noto sans cjk tc', 10))
               Label3a.pack(padx=15, pady = 6)

               entry3a = Entry(newWindow12, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
               entry3a.pack(padx=15, pady=7)
        
               Label4a = Label(newWindow12, text = 'Date', font=('noto sans cjk tc', 10))
               
    
               entry4a= Entry(newWindow12, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
               Label4a.pack(padx=15, pady=6)
               entry4a.pack(padx=15, pady=7)   
            
               btn5 = Button(newWindow12, text = 'Submit',command = table, bg='white', font=('FreeSans', 14))
               btn5.pack(padx =5)
            def appointmentlist() :
               my_w = Tk() 
               my_w.title('Appointment List')  
               my_w.geometry("600x500+300+100") 
               my_w.configure(bg='cyan')
               
               from sqlalchemy import create_engine
               engine2 = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
               r_set=engine2.execute("SELECT * FROM appointments")
               i = 0             
               for Appointments in r_set: 
                   for j in range(len(Appointments)):
                       e = Entry(my_w, width=25, fg='black', font = 'helvetica 10') 
                       e.grid(row=i, column=j) 
                       e.insert(END, Appointments[j])
                   i = i + 1        
               my_w.mainloop()
               
               
                  
            def delapp ():
                myw2=Tk()
                myw2.title('Remove an Appointment')
                myw2.geometry("600x500+300+100")
                myw2.configure(bg='cyan')
                Labelo = Label(myw2,text = 'Appointment ID:', font=('FreeSans', 14))
                Labelo.pack(padx=15,pady= 20)

                entry1o = Entry(myw2, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
                entry1o.pack(padx=15, pady=5)
                def delappt ():                  
                        u = entry1o.get()
                        engine3 = create_engine('mysql+pymysql://root:12345678@localhost/db')
                        r_set=engine3.execute("delete FROM appointments where A_Id like '%s';" %(u,))
                        box.showinfo('info',' Succesfullu Removed a Patient')
                        
                    
                btn99 = Button(myw2, text = 'Remove', command = delappt, bg='white', font=('FreeSans', 14))
                btn99.pack(padx=5, pady = 20)
                
            def agraph():    
              
                mycon = sqltor.connect(host='localhost', user='root', passwd = 'trisha123A', database = 'db')
                df3 = pd.read_sql("Select Date,Count(Date) from appointments group by Date;", mycon)
             
                root = Tk()
                root.title('Analysis and Report')
                root.geometry("600x600+500+200")
                
                figure1 = plt.Figure(figsize=(10,10), dpi=100)
                ax1 = figure1.add_subplot(111)
                bar1 = FigureCanvasTkAgg(figure1, root)
                bar1.get_tk_widget().pack(side='left')
                df1 = df3[['Date','Count(Date)']].groupby('Date').sum()
                df1.plot(kind='bar', legend=True, ax=ax1, color = 'cyan')
                ax1.set_title('Occupancy on a Date')
              
               

                root.mainloop() 
         
                
                
            btn6 = Button(newWindow, text = 'See Appointments',command = appointmentlist, bg='white', font=('FreeSans', 14))
            btn6.pack(padx =5, pady =20)
            btn69 = Button(newWindow, text = 'Add an Appointment', command = app, bg='white', font=('FreeSans', 14))
            btn69.pack(padx=5, pady=20)
            btn79 = Button(newWindow, text = 'Remove an Appointment', command = delapp, bg='white', font=('FreeSans', 14))
            btn79.pack(padx=5, pady = 20)
            bt01 = Button(newWindow, text = 'Reports', command = agraph, bg='white', font=('FreeSans', 14))
            bt01.pack(padx=35, pady=20)
        # Appointment Program
        def Patients():
            newWindow1=Tk()     
            newWindow1.title("Patients")
            newWindow1.geometry("600x500+300+100")
            newWindow1.configure(bg='Orange')
            title = Label(newWindow1,text = 'Patient Dashboard', font = ('noto sans cjk tc', 18, 'bold'))
            title.pack(padx = 5, pady = 20)
            title.configure(bg = 'Black', fg = 'white')
            def pati():
                def ptable():
                    p_name = entry1b.get()
                    p_id = entry2b.get()
                    need = entry3b.get()
                    symptoms = entry4b.get()
                    n = [{'P_Name':p_name,'P_Id':p_id,'Need': need,'Symptoms':symptoms}]
                    m1 = pd.DataFrame(n)
                    print (m1)
                    engine = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
                    mycon=engine.connect()
                    m1.to_sql('patients', mycon, index=False, if_exists = 'append')
            
        
                newWindow122=Tk()  
                frame = Frame(newWindow122)
                newWindow122.title("Add an Patient")
                newWindow122.geometry("600x500+300+100")
                frame.pack(padx=10,pady = 20)
                Label1b = Label(newWindow122,text = 'Patient Name:')
                Label1b.pack(padx=15,pady= 5)

                entry1b = Entry(newWindow122, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
                entry1b.pack(padx=15, pady=6)
                
                Label2b = Label(newWindow122,text = 'Patient ID: ')
                Label2b.pack(padx = 15,pady=5)
                
                entry2b = Entry(newWindow122, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
                entry2b.pack(padx = 15,pady=6)
                Label3b = Label(newWindow122, text = 'Need')
                Label3b.pack(padx=15, pady = 5)

                entry3b = Entry(newWindow122, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
                entry3b.pack(padx=15, pady=6)
                
                Label4b = Label(newWindow122, text = 'Symptoms')
                Label4b.pack(padx=15, pady=5)
    
                entry4b= Entry(newWindow122, width=50, fg = "red", font=('helvetica', 13), bg='mint cream')
                entry4b.pack(padx=15, pady=6)   
            
                btn7 = Button(newWindow122, text = 'Report',command = ptable, bg='white', font=('FreeSans', 14))
                btn7.pack(padx =5)
           
            def patientslist() :
                    my_w = Tk() 
                 
                    my_w.geometry("600x500+300+100")                                        
                    from sqlalchemy import create_engine
                    engine2 = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
                    r_set=engine2.execute("SELECT * FROM patients")
                    i=0 
                    for patients in r_set: 
                        for j in range(len(patients)):
                            e = Entry(my_w, width=25, fg='black', font = 'Helvetica 10') 
                            e.grid(row=i, column=j) 
                            e.insert(END, patients[j])
                        i=i+1
                
                    

                    my_w.mainloop()
            def delpat ():
                    myw1= Tk()
                    myw1.title('Delete a patient')
                    Labelo = Label(myw1,text = 'Patient ID:')
                    Labelo.pack(padx=15,pady= 5)
                    myw1.geometry("600x500+300+100")
                    entry1o = Entry(myw1,bd =5)
                    entry1o.pack(padx=15, pady=5)
                    def delpatt ():                  
                            u = entry1o.get()
                            engine3 = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
                            r_set=engine3.execute("delete FROM patients where P_Id like %s;" %(u,))
                        
                            box.showinfo('info',' Succesfullu Removed a Patient')
                    
                    btn99 = Button(myw1, text = 'Remove', command = delpatt)
                   
                    btn99.pack(padx=5, pady = 20)
                    
            
           
            def pgraph():
                mycon = sqltor.connect(host='localhost', user='root', passwd = 'trisha123A', database = 'db')
                df3 = pd.read_sql("Select Need,Count(Need) from patients group by Need;", mycon)
             
                root = Tk()
                root.title('Analysis and Report')
                root.geometry("1000x1000+500+200")
                
                figure1 = plt.Figure(figsize=(10,50), dpi=100)
                ax1 = figure1.add_subplot(111)
                bar1 = FigureCanvasTkAgg(figure1, root)
                bar1.get_tk_widget().pack(side='left')
                df1 = df3[['Need','Count(Need)']].groupby('Need').sum()
                df1.plot(kind='bar', legend=True, ax=ax1, color = 'orange')
                ax1.set_title('Demand of Speciality')

                
               

                root.mainloop() 
                
            btn8= Button(newWindow1, text = 'See Patients ', command = patientslist, bg='white', font=('FreeSans', 14))
            btn8.pack(padx=5, pady=20)        
            btn69 = Button(newWindow1, text = 'Add a Patient', command = pati, bg='white', font=('FreeSans', 14))
            btn69.pack(padx=5, pady=20)
            btn799 = Button(newWindow1, text = 'Remove a Patient', command = delpat, bg='white', font=('FreeSans', 14))
            btn799.pack(padx=5, pady = 20)
            bt01 = Button(newWindow1, text = 'Report', command = pgraph, bg='white', font=('FreeSans', 14))
            bt01.pack(padx=35, pady=20)
                
        def Doctors(): 
            newWindow2=Tk()     
            newWindow2.title("Doctors")
            newWindow2.geometry("600x500+300+100")    
            newWindow2.configure(bg='Yellow')
            title = Label(newWindow2,text = 'Doctor Dashboard', font = ('noto sans cjk tc', 18, 'bold'))
            title.pack(padx = 5, pady = 20)
            title.configure(bg = 'Coral')
            def docc():
                def dtable():
                    d_name = entry1c.get()
                    d_id = entry2c.get()
                    speciality = entry3c.get()
                    timing = entry4c.get()
                    Fees = entry5c.get()
                    n1 = [{'D_ID':d_id,'Doctor_Name':d_name,'Speciality': speciality,'Timings':timing, 'Fees' : Fees}]
                    m1 = pd.DataFrame(n1)
                    print (m1)
                    engine = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
                    mycon=engine.connect()
                    m1.to_sql('doctors', mycon, index=False, if_exists = 'append')
            
        
                
                newWindow121=Tk()  
                frame = Frame(newWindow121)
                newWindow121.geometry("600x500+300+100")
                newWindow121.title("Add an Appointment")
                newWindow121.configure(bg='yellow')
                frame.pack(padx=10,pady = 20)
                Label1c = Label(newWindow121,text = 'Doctor Name:')
                Label1c.pack(padx=15,pady= 5)

                entry1c = Entry(newWindow121,bd =5)
                entry1c.pack(padx=15, pady=5)
            
                Label2c = Label(newWindow121,text = 'Doctor ID: ')
                Label2c.pack(padx = 15,pady=6)
                
                entry2c = Entry(newWindow121, bd=5)
                entry2c.pack(padx = 15,pady=7)

                Label3c = Label(newWindow121, text = 'Speciality')
                Label3c.pack(padx=15, pady = 6)

                entry3c = Entry(newWindow121, bd=5)
                entry3c.pack(padx=15, pady=7)
                
                Label4c = Label(newWindow121, text = 'Timings')
                Label4c.pack(padx=15, pady=6)
    
                entry4c= Entry(newWindow121, bd=5)
                entry4c.pack(padx=15, pady=7)   
            
                Label5c = Label(newWindow121, text = 'Fees')
                Label5c.pack(padx = 15, pady = 5)
                
                entry5c = Entry(newWindow121, bd = 5)
                entry5c.pack()
                
                btn11 = Button(newWindow121, text = 'Submit',command = dtable, bg='white', font=('FreeSans', 14))
                btn11.pack(padx =5)
                newWindow121.mainloop()
           
            def doclist() :
                    my_w = Tk() 
                    my_w.configure(bg='yellow')
                    my_w.geometry("600x500+300+100")                                     
                    from sqlalchemy import create_engine
                    engine2 = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
                    r_set=engine2.execute("SELECT * FROM doctors")
                    i=0 
                    for doctors in r_set: 
                        for j in range(len(doctors)):
                            e = Entry(my_w, width=25, fg='black', font = 'Helvetica 10') 
                            e.grid(row=i, column=j) 
                            e.insert(END, doctors[j])
                        i=i+1
                   
                    

                    my_w.mainloop()
            def deldoc ():
                    myw = Tk()
                    myw.title('Remove a Doctor')
                    myw.geometry("600x500+300+100")
                    myw.configure(bg='yellow')
                    Labelo = Label(myw,text = 'Doctor ID: Enclose in ""')
                    Labelo.pack(padx=15,pady= 20)

                    entry1o = Entry(myw,bd =5)
                    entry1o.pack(padx=15, pady=5)
                    def deldoct ():                  
                            u = entry1o.get()
                            engine3 = create_engine('mysql+pymysql://root:trisha123A@localhost/db')
                            r_set=engine3.execute("delete FROM doctors where D_Id like %s;" %(u,))
                        
                        
                    
                    btn991 = Button(myw, text = 'Remove', command = deldoct, bg='white', font=('FreeSans', 14))
                    btn991.pack(padx=5, pady = 20)
                    
            def dgraph():
                mycon = sqltor.connect(host='localhost', user='root', passwd = 'trisha123A', database = 'db')
                df3 = pd.read_sql("Select Doctor_Name, Fees from doctors group by Fees;", mycon)
                root = Tk()
                root.title('Analysis and Report')
                root.geometry("800x800+500+200")
                figure3 = plt.Figure(figsize=(20,4), dpi=100)
                ax3 = figure3.add_subplot(111)
                ax3.scatter(df3['Doctor_Name'],df3['Fees'], color = 'yellow')
                scatter3 = FigureCanvasTkAgg(figure3, root) 
                scatter3.get_tk_widget().pack(side='right')
                ax3.legend(['Fees']) 

                ax3.set_xlabel('Doctor Name')
                ax3.set_title('Fees')
                root.mainloop()

                
                
                
                
              
                
            btn8= Button(newWindow2, text = 'See Doctors', command = doclist, bg='white', font=('FreeSans', 14))
            btn8.pack(padx=5, pady=20)           
                    
            btn019= Button(newWindow2, text = 'Add a Doctor', command = docc, bg='white', font=('FreeSans', 14))  
            btn019.pack(padx=35, pady=20)
                
            btn7999 = Button(newWindow2, text = 'Remove a Doctor', command = deldoc, bg='white', font=('FreeSans', 14))
            btn7999.pack(padx=5, pady = 20)
            
            bt01 = Button(newWindow2, text = 'Report', command = dgraph, bg='white', font=('FreeSans', 14))
            bt01.pack(padx=35, pady=20)  
            
        
        btn1 = Button(MAIN_WINDOW, text = 'Appointments Portal',command = appointments, bg='sky blue', font = ('helvetica', 16))
        btn1.pack(padx=20, pady=20) 
        btn2 = Button(MAIN_WINDOW, text = 'Patients Portal',command = Patients, bg='sky blue', font = ('helvetica', 16))
        btn2.pack(padx=20, pady=20)
        btn3 = Button(MAIN_WINDOW, text = 'Doctors Portal',command = Doctors, bg='sky blue', font = ('helvetica', 16))
        btn3.pack(padx=20, pady=20)
        
        
        
   
        newWindow.mainloop()
        
       
        MAIN_WINDOW. mainloop()
      
 
window = Tk()
window.title('Admin Login')

'''
img = ImageTk.PhotoImage(Image.open("688.png"))

panel = Label(window, image = img)
panel.pack(fill = "both", expand = "no")

panel.configure(bg = 'papaya whip')
'''

window.geometry("800x800+600+300")
window.configure(bg = 'papaya whip')

frame = Frame(window, borderwidth=2)
Label22=Label(window, text='', font = ('noto sans cjk tc', 18, 'bold'))
Label22.pack(padx=20, pady=6)
Label22.configure(bg = 'papaya whip')

Label221=Label(window, text='HOSPITAL MANAGEMENT SYSTEM', font = ('noto sans cjk tc', 18, 'bold'), )
Label221.pack(padx=20, pady=6)
Label221.configure(bg = 'papaya whip')
Label1 = Label(window,text = 'Username:', font = ('MS sans serif', 18, 'bold'))
Label1.pack(padx=15,pady= 5)
Label1.configure(bg = 'light cyan')

entry1 = Entry(window, font=('helvetica', 20))
entry1.pack(padx=15, pady=5)
Label2 = Label(window,text = 'Password: ', font = ('MS sans serif', 18, 'bold'))
Label2.pack(padx = 15,pady=6)
Label2.configure(bg = 'light cyan')
entry2 = Entry(window, show = '•', font=('helvetica', 20))
entry2.pack(padx = 15,pady=7)


btn = Button(frame, text = 'Check Login',command = dialog1, bg = 'white', font = ('helvetica', 18, 'bold'))



btn.pack(side = RIGHT , padx =5)
frame.pack(padx=100,pady = 19)
window.mainloop()
