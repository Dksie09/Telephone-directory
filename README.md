# Telephone-directory
#A connecetion was set between  mysql and python using a library called mysql-connector which enabled a flexible interaction between SQL database from within python
import pandas as pd
import mysql.connector
#mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
#cur=mydb.cursor()
print('\t\t\t^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^')
print("\t\t\t][                                    ][")
print("\t\t\t][     ONLINE TELEPHONE DIRECTORY     ][")
print("\t\t\t][                                    ][")
print("\t\t\tvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv")
print("\t\twelcome to XYZ delhi online telephone directory\n\n")
def menu():
    print('    -------------------------------------------------------------------------')
    print("    ||    select your task from the following listed options:               ||")
    print("    ||        (1)New registeration                                          ||")
    print("    ||        (2)Search for specific contact details                        ||")
    print("    ||        (3)Update registered details                                  ||")
    print("    ||        (4)Delete an entry                                            ||")
    print("    ||        (5)Exit                                                       ||")
    print('    -------------------------------------------------------------------------')
menu()
x=int(input("enter the no. selected: "))
while (0<x<5):
    if x == 1:
        print("\nFOR REGISTRATION")
        print('-----------------')
        print("\nkindly enter the following details:")
        code=input("\t4-digit spcl id: ")
        name=input("\tfull name: ")
        telno=int(input("\ttelephone no.: "))
        mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
        cur=mydb.cursor()
        query0=('select * from tel_directory where telephone_no=(%s)')
        cur.execute(query0,(telno,))
        row=cur.fetchall()
        if row == []:
            addr=input("\tfull address: ")
            date=input('\tdate of registeration(yyyymmdd): ')
            query1=("insert into tel_directory values(%s,%s,%s,%s,%s)")
            val=(code,name,telno,addr,date)
            cur.execute(query1,val)
            mydb.commit()
            #mydb.close()
            print("\n\t\t **you have been registered successfully**\n")
        else:
            print('\n\t\t *telephone number already exists*')
            print('\t\t**registeration process terminated**\n')
        menu()
        x=int(input("Enter task no.: "))
    elif x == 2:
        print("\nTO SEARCH")
        print('---------')
        print("select your search criteria-")
        print('\t1.name(N)')
        print('\t2.tel number(T)')
        print('\t3.Date of registeration(D)')
        cri=input('enter search criteria: ')
        if cri == 'T':
            name=input("enter telephone number: ")
            mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
            cur=mydb.cursor()
            query1=("select * from tel_directory where telephone_no=(%s)")
            cur.execute(query1,(name,))
            row=cur.fetchall()
            
            print("\ndisplaying the details-")
            for i in row:
                s=['spl_code             |','Name                 |','Telephone_no         |','Address              |','date of registeration|']
                df=pd.DataFrame(i,index=s)
                print('------------------------------------------------------------------')
                print("|",df,"|\n")
                
                
                #print(i)
        elif cri == 'N':
            name=input("enter name: ")
            mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
            cur=mydb.cursor()
            query1=("select * from tel_directory where Name= (%s)")
            cur.execute(query1,(name,))
            row=cur.fetchall()
            
            print("\ndisplaying the details-")
            for i in row:
                s=['spl_code             |','Name                 |','Telephone_no         |','Address              |','date of registeration|']
                df=pd.DataFrame(i,index=s)
                print('------------------------------------------------------------------')
                print(df,"\n")
                

        else:
            name=input("enter date(yyyymmdd): ")
            mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
            cur=mydb.cursor()
            query1=("select * from tel_directory where reg_date=(%s)")
            cur.execute(query1,(name,))
            row=cur.fetchall()
            print("\ndisplaying the details-")
            for i in row:
                s=['spl_code             |','Name                 |','Telephone_no         |','Address              |','date of registeration|']
                df=pd.DataFrame(i,index=s)
                print('------------------------------------------------------------------')
                print(df,"\n")
                
        print('\n\n')    
        menu()    
        #mydb.close()
        x=int(input("\nEnter task no.: "))

    elif x == 3:
        print("TO UPDATE")
        print('---------')
        print('select the detail you want to edit-')
        print('\t1.Name(N)')
        print('\t2.Address(A)')
        print('\t3.Telephone number(T)')
        fk=input("\t\tEnter your choice: ")
        if fk == 'A':
            print("TO EDIT ADDRESS")
            print('---------------')
            code=input("\tenter 4-digit spcl id: ")
        
            mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
            cur=mydb.cursor()
            query0=('select * from tel_directory where spl_id=(%s)')
            cur.execute(query0,(code,))
            row=cur.fetchall()
            if row == []:
                print('\n\t\t*invalid id*')
            else:
                addr=input("\tEnter new address: ")
                query1=('UPDATE tel_directory SET address =(%s) WHERE spl_id =(%s)')
                cur.execute(query1,(addr,code,))
                mydb.commit()
                #mydb.close()
                print("\n\t\t*updated successfully*\n")
        elif fk == 'N':
            print("TO EDIT NAME")
            print('---------------')
            code=input("\tenter 4-digit spcl id: ")
        
            mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
            cur=mydb.cursor()
            query0=('select * from tel_directory where spl_id=(%s)')
            cur.execute(query0,(code,))
            row=cur.fetchall()
            if row == []:
                print('\n\t\t*invalid id*')
            else:
                addr=input("\tEnter updated name: ")
                query1=('UPDATE tel_directory SET Name =(%s) WHERE spl_id =(%s)')
                cur.execute(query1,(addr,code,))
                mydb.commit()
                #mydb.close()
                print("\n\t\t*updated successfully*\n")

        elif fk == 'T':
            print('TO UPDATE TELEPHONE NUMBER')
            print('--------------------------')
            code=input("\tenter 4-digit spcl id: ")
        
            mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
            cur=mydb.cursor()
            query0=('select * from tel_directory where spl_id=(%s)')
            cur.execute(query0,(code,))
            row=cur.fetchall()
            if row == []:
                print('\n\t\t*invalid id*')
            else:
                addr=input("\tEnter new Telephone number: ")
                query1=('UPDATE tel_directory SET telephone_no =(%s) WHERE spl_id =(%s)')
                cur.execute(query1,(addr,code,))
                mydb.commit()
                #mydb.close()
                print("\n\t\t*updated successfully*\n")
        else:
             print('\n\t**invalid entry**')
            
    
        print('\n\n')
        menu()
        x=int(input("Enter task no.: "))

        
    else:
        print("TO DELETE")
        print('---------')
        code=input('\tenter 4-digit spl id: ')
        mydb=mysql.connector.connect(host="localhost",user="root",passwd="creator1434.",database="teledirectory")
        cur=mydb.cursor()
        query00=('select * from tel_directory where spl_id=(%s)')
        cur.execute(query00,(code,))
        row=cur.fetchall()
        if row == []:
            print('\n\t\t**invalid id**')
        
        else:
            query0=('select telephone_no from tel_directory where spl_id=(%s)')
            cur.execute(query0,(code,))
            d=cur.fetchall()
            for i in d:
                print('\n\t\tunregistering this number:',i)
            yn=input("\tPress 'f' to confirm deletion: ")
            if yn == "f":
                query1=("DELETE from tel_directory where spl_id=(%s)" )
                cur.execute(query1,(code,))
                mydb.commit()
                print("\n\t\t**Deleted successfully**\n")
            else:
                print("\n\t\t*Deletion proccess terminated*\n")
        print('\n\n')
        menu()
        x=int(input("Enter task no.: "))
print('\n-----------------------------------------------------------------')
print("\t\t\tHOPE YOU ENJOYED OUR SERVICES")
rate=input("\t\n\t\twould you like to rate us?y/n: ")
if rate == "y":
    a=int(input("rate our work on the scale of zero to five:  "))
    if a <=3:
        print("\t\twe will try to improve our services :(")
    com=input("additional comments: ")
    print("\t\t\tTHANKS!!")
print('-----------------------------------------------------------------') 
