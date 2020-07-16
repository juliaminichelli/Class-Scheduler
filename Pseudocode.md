ClassScheduler-GitHub

I will use the class structures to make my code more readable and more easily iterated over.

The main object I will be using are Teacher objects, which will have attributes such as the amount of classes taught for which cohorts, days available, total amount of classes to be taught. It will have two methods: one that calculates the availability score and one that updates the class count.

The code will go as follows:

Step 0: Creates the structure of the schedule (which will be a list of lists). There will be five elements in the week_schedule, one referring to each day of the week. Within each list, there will be slot lists, which will contain the teachers teaching at that cohort. Each slot list will have eight elements, which refer to the total amount of cohorts that will have to be scheduled.

Step 1: Design the four main functions.

1. Heapify. The first function to be implemented will be a heap. Heaps were implemented using the code from Assignment 2. I chose this data structure because, for each cohort and day, I will need to sort the teachers available by their availability scores. The heap structure is an efficient way to sort the items, serving a priority queue for the teachers with availability scores that will make me add them first into the schedule. It has the complexity of O(log n) to sort, and I can easily include and delete items from it.

2. Determining start. The purpose of this function is to compute the sum of the availability score for all teachers of each cohort to determine what is the order in which their classes will be assigned at any given day (read Metric for reference).

3. Create heap. This function creates a max heap with the available teachers at a given day, using the availability score of a teacher at that day and time of schedule (if a teacher gets assigned to two classes on a same day, it had different availability scores - and most likely different positions in the heap - because the classes to be scheduled - the numerator - changed after each assignment).

4. Assigning root. This is the most important function for the algorithm. Hence, I will explain it step by step.

a. It takes many elements as input:
    i. A max heap with teachers objects and their availability scores for a given day and cohort;
    ii. The day of the week we are referring to;
    iii. The cohort we are referring to;
    iv. The slot number we are iterating over;
    v. The schedule structure (list of lists) that we will iterate over;

b. It starts by checking if the input heap is not empty, and, if it is not, we use the root of the heap as a potential item to be included in the slot we are looking for.

c. Because we cannot schedule a same teacher to the same time slot, we need to check if it has already been included. If it has, we must move down to our heap priority queue to get a teacher that has not yet been scheduled. If there are none available, we return an empty slot. If we find one, we input it in the slot we are looking at.

d. After including it in the slot, we must delete one from the class count (as it is the variable we are using to keep track of how many classes we still have to
schedule).

e. Then, our next step is to re-compute the availability score of our item. It will have changed because the class count changed. It has to change because we want to prioritize higher availability scores to make sure we get more teachers assigned.
    i. If the availability score is now zero, we must delete it from the heap, as it is because we do not have more classes to schedule (class count is zero).

f. Because the availability score of the item changed, we must heapify our priority queue to make sure it is properly ordered after the changes.

g. The output is the schedule with the slot fulfilled.

Step 2: With these four functions we are able to finalize the schedule. I created two functions to accomplish that: one that implements a given day, looping over slots, and another that loops over the days, using a combination of these four functions. Within the implement_day function, it is important to acknowledge that our last step is to change the available days for the characters, which deletes the day that has already been computed. This is an important step to make sure that the availability scores metrics will be optimized, as explained in the Metric section. 

Now that we are familiar with what it is the code will be doing I want to justify what are the features of a greedy algorithm in this implementation. The main feature that resembles a greedy implementation in this algorithm is in the Assigning root function. As can be noticed, we use the “most optimal” availability score to determine which is the next teacher that will be assigned to a given time slot. To make sure that this metric is the most optimized within our constraints, I made sure to include the updates in the numerator and denominator of the calculations, such that, at any time step, we make the most optimal decision. Nonetheless, this does not satisfy the greedy-choice property nor the optimal substructure features of greedy algorithms, which would guarantee that we get an optimal solution using a greedy strategy.