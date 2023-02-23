# Homework Assignment 6


**Overview**. In the urgent care simulation model we developed in class, let's assume that 
patients will also get screened for depression during their exam. If the result is positive, they will be 
seen by an on-site mental health specialist before leaving the urgent care. 
Here we want to make the necessary changes to the model we implemented in the class
to simulate the operation of this new system. Feel free to use any of the files we worked on durin the class 
(you can find them under the branch 'P1_Simulation_Solution' of [this repository](https://github.com/HPM573/Lab_DiscreteEventSimulation/tree/P1_Simulation_Solution))

**Problem 1: Flowcharts (Weight 4)**. Update the flowcharts of processing the events of this system. 
You can use any program your wish to draw the flowcharts. Include the flowcharts in your submission.
Note that this new model involves a new event to represent the "End of Mental Health Care". 

_Hint:_ At the time you are creating a patient, you can also decide decide 
whether a patient will get diagnosed with depression during their visit with a flip of a coin. 

**Problem 2: Simulation (Weight 10)**. 
Modify the code of the urgent care model to incorporate the addition of the mental health specialist. 
Run your simulation with these assumptions:
- 20 hours of operation
- 10 primary-care physicians,
- 1 mental health specialist on site,
- Patient inter-arrival time exponentially distributed with mean 1 minutes, 
- For primary-care physicians, the exam duration is exponentially distributed with mean 10 minutes,
- Proportion of patients with depression 10%, 
- For mental health specialist, the duration of mental health consultation is exponentially distributed with mean 20 minutes.

Report the number of patients arrvied and who were seen by a physician 
and the mental health specialist before the urgent care closed.

_Hint:_ Modify the `__init__` method of the `Patient` class so that it takes `if_with_depression` as an argument,
and set `self.ifWithDepression = if_with_depression` so that you can know the depression status of the
patient during the simulation. 
At the time you are creating a patient, decide about the depression status 
of the patient with a coin flip. At the end of the exam, check the patient's `ifWithDepression` 
and if it is `True`, send the patient to the mental health specialty unit. 

_Hint:_ If a patient has depression, they will go to see the mental health specialist after completing their visit with the primary care physician. Therefore, we need to modidify the `remove_patient` method of the `Physician` class to get the patient who finished the exam. One may think that the following code would work:

```
def remove_patient(self):
    """ :returns the patient that was being served in this exam room"""
    
    # return the patient being served 
    return self.patientBeingServed
    
    # set the patient being serive to none
    self.patientBeingServed = None
    
    # the physician is idle now
    self.isBusy = False
```
The issue with the above code is that the second line 
(`self.patientBeingServed = None`) never gets executed since the code exists this function right after executing
`return self.patientBeingServed`.

The code below resolves this issue by first storing the patient 
that is to be returned in the variable `returned_patient`.

```
def remove_patient(self):
    """ :returns the patient that was being served in this exam room"""

    # store the patient to be returned
    returned_patient = self.patientBeingServed
    
    # set the patient being serive to none
    self.patientBeingServed = None

    # the physician is idle now
    self.isBusy = False
    
    # return the patient that was being served
    return returned_patient
```
