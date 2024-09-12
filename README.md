# AccuKnox Django Trainee Questions

## Topic 1: Django Signals

### Question 1: By default are django signals executed synchronously or asynchronously? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.


**Answer:** By default, Django signals are executed **synchronously**. This means that when a signal is sent, the main code will stop and wait for the signal handlers to complete their execution before moving on.

#### Code snippet:

```python
import time
from django.db.models.signals import post_save
from django.contrib.auth.models import User
from django.dispatch import receiver


@receiver(post_save, sender=User)
def slow_signal(sender, instance, **kwargs):
    print("Signal started")
    time.sleep(5)  
    print("Signal finished")


print("Before user creation")
User.objects.create(username="testuser")
print("After user creation")
```


### Question 2: Do django signals run in the same thread as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.


**Answer:** Yes, Django signals run in the same thread as the code that triggers them. To verify this, we can print the thread ID of both the main thread (where the signal is sent) and inside the signal handler.

#### Code snippet:

```python
import threading
from django.db.models.signals import post_save
from django.contrib.auth.models import User
from django.dispatch import receiver


@receiver(post_save, sender=User)
def check_thread(sender, instance, **kwargs):
    print(f"Signal thread: {threading.get_ident()}")


print(f"Caller thread: {threading.get_ident()}")
User.objects.create(username="testuser")

```
### Question 3:  By default do django signals run in the same database transaction as the caller? Please support your answer with a code snippet that conclusively proves your stance. The code does not need to be elegant and production ready, we just need to understand your logic.




**Answer:** Yes, Django signals run in the same database transaction as the code that triggers them. If the main transaction fails, any database changes made inside the signal handler are also rolled back.

#### Code snippet:

```python
from django.db import transaction
from django.db.models.signals import post_save
from django.contrib.auth.models import User
from django.dispatch import receiver

@receiver(post_save, sender=User)
def modify_profile(sender, instance, **kwargs):
    print("Signal executed")
    


try:
    with transaction.atomic():
        print("Transaction started")
        user = User.objects.create(username="testuser")
       
        raise Exception("Rolling back transaction")  
except:
    print("Transaction rolled back")


if User.objects.filter(username="testuser").exists():
    print("User exists after rollback")
else:
    print("User was rolled back")

```

## Topic 2 : Custom Classes in Python

#### Code snippet:
```python
class Rectangle:
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def __iter__(self):
        
        return iter([{'length': self.length}, {'width': self.width}])


rectangle = Rectangle(10, 5)

for dimension in rectangle:
    print(dimension)

```
