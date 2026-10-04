#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Thu Sep 24 19:09:39 2026

@author: pgcp-esd
"""
```
#####accountclass.py
from abc import abstractmethod,ABC
class Account(ABC):
    def __init__(self,aid=0,nm="",bal=0,pin="",sque="",sans=""):
        self.__aid=aid
        self.__name=nm
        self._balance=bal
        self.__pin=pin
        self.__sque=sque
        self.__sans=sans
    def set_aid(self,aid):
        self.__aid=aid
    def set_name(self,nm):
        self.__name=nm
    def set_balance(self,bal):
        self._balance=bal
    def set_pin(self,pin):
        self.__pin=pin
    def set_sque(self,sque):
        self.__sque=sque
    def set_sans(self,sans):
        self.__sans=sans
    #getter methods
    def get_aid(self):
         return self.__aid
    def get_name(self):
         return self.__name
    def get_balance(self):
         return self._balance
    def get_pin(self):
         return self.__pin
    def get_sque(self):
         return self.__sque    
    def get_sans(self):
         return self.__sans
    def withdraw(self,amount):
        self._balance-=amount
    def deposit(self,amount):
        self._balance+=amount
    @abstractmethod
    def calculatecharges(self):
        pass
    def __str__(self):
        return f"Aid :{self.__aid} name: {self.__name} balance: {self._balance} pin: {self.__pin} que: {self.__sque} Ans: {self.__sans}"

class DematAccount(Account):
    def __init__(self,aid=0,nm="",bal=0,pin="",sque="",sans="",comm=0):
        super().__init__(aid,nm,bal,pin,sque,sans)
        self.__comm=comm
    def set_comm(self,num):
        self.__comm=num
    def get_comm(self):
        return self.__comm
    def calculatecharges(self):
        return self._balance*self.__comm
    def __str__(self):
        return super().__str__()+f" commission {self.__comm}"
    
class SavingAccount(Account):
    def __init__(self,aid=0,nm="",bal=0,pin="",sque="",sans="",chnum=0):
        super().__init__(aid,nm,bal,pin,sque,sans)
        self.__chequebknum=chnum
    def set_chequebknum(self,num):
        self.__chequebknum=num
    def calculatecharges(self):
        return self._balance*0.02
    def get_chequebknum(self):
        return self.__chequebknum
    def __str__(self):
        return super().__str__()+f" cheque bk num {self.__chequebknum}"

if __name__=="__main__":    
    #ac1=Account(12,"Sameer",23456,1111,"favorite color","Red")
    #ac2=Account(13,"Rohit",33456,2222,"favorite color","Red")
    sac1=SavingAccount(12,"Sameer",23456,1111,"favorite color","Red",11111111)
    sac2=SavingAccount(13,"Rohit",33456,2222,"favorite color","Red",22333)
    print(sac1)
    print(sac2)
    
 ########Bankmgnt.py
 
from accountclass import *
accounts={100:SavingAccount(100,"Sameer",23456,1111,"favorite color","Red",11111111),
          101:DematAccount(101,"Sameer",23456,1111,"favorite color","Red",0.03)}
#add a new account
def addnewAccount(ch):
    aid=int(input("enetr account number"))
    nm=input("enetr name")
    bal=float(input("enetr balance")) 
    pin=int(input("enetr pin"))
    sques=input("enetr question")
    sans=input("enetr ans")
    if ch==1:
        chk=int(input("enter checque bk num"))
        s=SavingAccount(aid,nm,bal,pin,sques,sans,chk)
    else:
        comm=float(input("enetr commission"))
        s=DematAccount(aid,nm,bal,pin,sques,sans,comm)
    accounts[aid]=s

#display all accounts    
def displayAll():
    for num,ac in accounts.items():
        print(ac)
        
def getBalance(acid):
    v=accounts.get(acid,-1)
    if v!=-1:
        pin=input("enetr pin")
        if pin==v.get_pin():
          return v.get_balance()
    return -1

def changepinnumber(acid):
    #search account
    v=accounts.get(acid,-1)
    if v!=-1:
        #display secrete question
        ans=input(v.get_sque())
        #if answer matches then change the pin
        if ans==v.get_sans():
            newpin=int(input("enter new pin"))
            v.set_pin(newpin)
            return True
    return False

def getCharges(acid):
      v=accounts.get(acid,-1)
      if v!=-1:
          return v.calculatecharges()
      return -1
  
def getcommission(acid):
    v=accounts.get(acid,-1)
    if v!=-1:
        if isinstance(v, DematAccount):
            v.get_comm()
        
    return -1;
    
choice=-1
while choice!=0:
    choice=int(input("""1. add new acoount
                 2. display all account
                 3. display balance by id
                 4. Withdraw amount
                 5. deposite amount
                 6. change pin
                 7. calculateCharges by id
                 8. display commision
                 9. display checquebook number
                 10. delete account
                 0. exit"""))
    match choice:
        case 1:
            ch=int(input("1. Demat 2. Saving"))
            addnewAccount(ch)
            pass
        case 2:
            displayAll()
           
        case 3:
            acid=int(input("enetr account id"))
            balance=getBalance(acid)
            if balance!=-1:
                print(f"Balance : {balance}") 
            else:
                print("wrong account id")
        case 4:
            pass
        case 5:
            pass
        case 6:
            acid=int(input("enetr account id"))
            status=changepinnumber(acid)
            if status:
                print("pin changed successfully")
            else:
                print("you are not autherised")
           
        case 7:
            acid=int(input("enetr account id"))
            charges=getCharges(acid)
            if charges!=-1:
                print("Charges are ",charges)
            else:
                print("not found")
            
        case 8:
            acid=int(input("enetr account id"))
            comm=getcommission(acid)
            if comm!=-1:
                print("Commission : ",comm)
            else:
                print("Not found")
        case 9:
            pass
        case 0:
            print("Thank you for visiting......")
           
        case _:
            print("wrong choice")

```

```
 
==============================================================================
# COURSE MENU PROGRAM
lst=[("Java",200,150),("cpp",180,200)]
def addnewcourse():
    nm=input("enetr name")
    duration=int(input("enetr duration"))
    capacity=int(input("enter capacity"))
    lst.append((nm,duration,capacity))
    return True

def displayAll(clst=lst):
    for c,d,cap in clst:
        print(f"{c}---->{d}---->{cap}")
        
def displayByCapacity(c):
    clist=[]
    for course in lst:
        if course[2]>c:
           clist.append(course)
    if len(clist)>0:
        return clist
    else:
        return None
    
def searchByName(nm):
    for pos,course in enumerate(lst):
        if course[0]==nm:
            return pos,course
    return -1,None


    
def deleteByName(nm):
    pos,course=searchByName(nm)
    if course!=None:
        lst.remove(course)
        return True
    return False
            
def modifyByName(cname,c,d):
    pos,course=searchByName(cname)
    if pos!=-1:
      ans=input(f"do you want to modify {course}") 
      if ans=="y":
          #overwrite old tuple with new tuple
          lst[pos]=cname,d,c 
          return 1
      else: 
          return 2
    else:
        return 3
def sortByDuration(ch):
    lst1=lst.copy()
    if ch==1:
        lst1.sort(key=lambda x:x[1])
    else:
        lst1.sort(key=lambda x:x[1],reverse=True)
    return lst1
           
choice=0
while choice!=9:
    choice=int(input("""
                     1. add new course
                     2. delete course by name
                     3. display all
                     4. display by capacity
                     5. display by duration
                     6. sort on duration
                     7.sort on capacity
                     8. modify course duration and capacity
                     9.exit"""))
    match choice:
        case 1:
            status=addnewcourse()
            if status:
                print("course added successfully")
            else:
                print("Error occured")
        case 2:
            nm=input("enetr name to delete")
            status=deleteByName(nm)
            if status:
                print("Deleted successfully")
            else:
                print("Not found")
                
        case 3:
            displayAll()
            
        case 4:
            c=int(input("enter capacity"))
            lstcap=displayByCapacity(c)
            if lstcap!=None:
                displayAll(lstcap)
            
            pass
        case 5:
            pass
        case 6:
            ch=int(input("1. Ascending 2.Descending"))
            
            sort_duration=sortByDuration(ch)
            displayAll(sort_duration)
            pass
        case 7:
            pass
        case 8:
            cname=input("enetr course name to modify")
            c=int(input("enetr new capacity"))
            d=int(input("Enter duration"))
            status=modifyByName(cname,c,d)
            if status==1:
                print("found and modification done")
            elif status==2:
                print("found and modification not done")
            else:
                print(f"{cname} not found")
           
        case 9:
            print("Thank you for visiting......")
            
        case _:
            print("wrong choice")
                




```
