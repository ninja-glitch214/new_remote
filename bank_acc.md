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
        
 
    
    
