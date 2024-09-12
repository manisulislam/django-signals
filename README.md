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

