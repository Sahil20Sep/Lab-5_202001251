# IT314: SOFTWARE ENGINEERING (Sec-B)
# LAB-5 (Group 5)
## _Name: Sahil Mangukiya_
## _Student ID: 202001251_
### STATIC ANALYSIS USING MYPY FOR PYTHON FILES:
### (Here, I used My dbms Project Python Code Hackathon Manangement System).

### Installing mypy library in VS Code: <br>
![1](https://user-images.githubusercontent.com/118440195/227467815-2d8cd18e-071a-4313-8bd0-d154d306e6db.png)

### views.py Code: <br>
```
from django.shortcuts import render
from Hackathon_Management_System.models import ParticipantModel
from Hackathon_Management_System.models import HackathonModel
from django.contrib import messages
from Hackathon_Management_System.forms import Participantforms, Hackathonforms
from django.db import connection

def HomePage(request):
    return render(request,'main.html')

def showpart(request):
    showall=ParticipantModel.objects.all()
    return render(request,'Index.html',{"data":showall})

def showhack(request):
    showall=HackathonModel.objects.all()
    return render(request,'Index2.html',{"data":showall})

def Insertpart(request):
    if request.method=="POST":
        if request.POST.get('part_id') and request.POST.get('first_name') and request.POST.get('last_name') and request.POST.get('email_add') and request.POST.get('dob') and request.POST.get('student') and request.POST.get('college') and request.POST.get('cpi'):
            saverecord=ParticipantModel()
            saverecord.part_id=request.POST.get('part_id')
            saverecord.first_name=request.POST.get('first_name')
            saverecord.last_name=request.POST.get('last_name')
            saverecord.email_add=request.POST.get('email_add')
            saverecord.dob=request.POST.get('dob')
            saverecord.student=request.POST.get('student')
            saverecord.college=request.POST.get('college')
            saverecord.cpi=request.POST.get('cpi')
            saverecord.save()
            messages.success(request,'Participant '+saverecord.part_id+ ' is Saved Successfully..!')
            return render(request,'Insert.html')
    else:
        return render(request,'Insert.html')

def Inserthack(request):
    if request.method=="POST":
        if request.POST.get('hack_id') and request.POST.get('hack_name') and request.POST.get('name_type') and request.POST.get('date') and request.POST.get('time') and request.POST.get('duration'):
            saverecord=HackathonModel()
            saverecord.hack_id=request.POST.get('hack_id')
            saverecord.hack_name=request.POST.get('hack_name')
            saverecord.name_type=request.POST.get('name_type')
            saverecord.date=request.POST.get('date')
            saverecord.time=request.POST.get('time')
            saverecord.duration=request.POST.get('duration')
            saverecord.save()
            messages.success(request,'Hackathon '+saverecord.hack_id+ ' is Saved Successfully..!')
            return render(request,'Insert2.html')
    else:
        return render(request,'Insert2.html')

def Editpart(request,id):
    editpartobj=ParticipantModel.objects.get(part_id=id)
    return render(request,'Edit.html',{"ParticipantModel":editpartobj})

def updatepart(request,id):
    Updatepart=ParticipantModel.objects.get(part_id=id)
    form=Participantforms(request.POST,instance=Updatepart)
    if form.is_valid():
        form.save()
        messages.success(request,'Record Updated Successfully..!')
        return render(request,'Edit.html',{"ParticipantModel":Updatepart})

def Edithack(request,id):
    edithackobj=HackathonModel.objects.get(hack_id=id)
    return render(request,'Edit2.html',{"HackathonModel":edithackobj})

def updatehack(request,id):
    Updatehack=HackathonModel.objects.get(hack_id=id)
    form=Hackathonforms(request.POST,instance=Updatehack)
    if form.is_valid():
        form.save()
        messages.success(request,'Record Updated Successfully..!')
        return render(request,'Edit2.html',{"HackathonModel":Updatehack})

def Delpart(request,id):
    delpart=ParticipantModel.objects.get(part_id=id)
    delpart.delete()
    showdata=ParticipantModel.objects.all()
    return render(request,"Index.html",{"data":showdata})

def Delhack(request,id):
    delhack=HackathonModel.objects.get(hack_id=id)
    delhack.delete()
    showdata=HackathonModel.objects.all()
    return render(request,"Index2.html",{"data":showdata})

def sortParticipant(request):
    if request.method=="POST":
        if request.POST.get('Sort'):
            type=request.POST.get('Sort')
            sorted=ParticipantModel.objects.all().order_by(type)
            context = {
                'data': sorted
            }
            return render(request,'Sort.html',context)
    else:
        return render(request,'Sort.html')

def sortHackathon(request):
    if request.method=="POST":
        if request.POST.get('Sort'):
            type=request.POST.get('Sort')
            sorted=HackathonModel.objects.all().order_by(type)
            context = {
                'data': sorted
            }
            return render(request,'Sort2.html',context)
    else:
        return render(request,'Sort2.html')

def runQuerypart(request):
    raw_query = "select * from participant where first_name like 'S%' or last_name like '%s' order by part_id;"

    cursor = connection.cursor()
    cursor.execute(raw_query)
    alldata=cursor.fetchall()

    return render(request,'runQuerypart.html',{'data':alldata})


```

### Error 1: 
Running above code and reviewing import errors: <br>

![2](https://user-images.githubusercontent.com/118440195/227468516-f568f8da-e015-42ed-a2dc-928a3f2f1427.png)

### Error 2: 
Finding indentation error: <br>

![3](https://user-images.githubusercontent.com/118440195/227470177-1fbb2234-46f0-45fa-b1f4-b4127b374d69.png)

### Error 3: 
Finding syntax error: <br>

![4](https://user-images.githubusercontent.com/118440195/227470357-0aae12f7-bdcb-4cce-904e-3b31e940d484.png)

### Error 4: 
Finding Attribute error: <br>

![5](https://user-images.githubusercontent.com/118440195/227470897-29888936-781b-483e-8e0d-146c6ab60249.png)

### models.py Code: <br>

```
from django.db import models

class ParticipantModel(models.Model):
    part_id=models.BigIntegerField(primary_key=True)
    first_name=models.CharField(max_length=255, null=False)
    last_name=models.CharField(max_length=255,null=False)
    email_add=models.CharField(max_length=255, null=False)
    dob=models.DateField(null=False)
    student=models.CharField(max_length=255, null=False)
    college=models.CharField(max_length=255, null=False)
    cpi=models.FloatField(null=False)
    class Meta: 
        db_table="participant"

class HackathonModel(models.Model):
    hack_id=models.BigIntegerField(primary_key=True)
    hack_name=models.CharField(max_length=255,null=False)
    name_type=models.CharField(max_length=255,null=False)
    date=models.DateField(null=False)
    time=models.TimeField(null=False)
    duration=models.BigIntegerField(null=False)
    class Meta:
        db_table="Hackathon"
```

### Error 5: 
Finding name not defined error: <br>

![6](https://user-images.githubusercontent.com/118440195/227471047-c1564684-f4da-4188-a106-969b80d14ebf.png)

### Error 6:
Finding syntax error: <br>

![7](https://user-images.githubusercontent.com/118440195/227471352-d188a0ed-fa66-44e9-b6ca-544a1941efe2.png)

### asgi.py Code: <br>

```
"""
ASGI config for Hackathon_Management_System project.

It exposes the ASGI callable as a module-level variable named ``application``.

For more information on this file, see
https://docs.djangoproject.com/en/4.1/howto/deployment/asgi/
"""

import os

from django.core.asgi import get_asgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "Hackathon_Management_System.settings")

application = get_asgi_application()  
```

### Error 7:
Finding invalid syntax error: <br>

![8](https://user-images.githubusercontent.com/118440195/227471538-1ce24388-0a1c-4fbf-a3fd-38d59eb14752.png)






