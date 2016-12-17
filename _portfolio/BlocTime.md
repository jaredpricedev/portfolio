---
layout: post
title: BlocTime
thumbnail-path: "img/bloctime.png"
short-description: BlocTime helps you stay focused on what you need to do!

---

{:.center}
![]({{ site.baseurl }}/img/bloctime.png)

# Explanation
This application is the first solo project of my Bloc Frontend. The goal of a Pomodoro Technique is to increase the productivity of the user by having them focus for 25 minutes. During the time they are not allowed to do anything other than the task at hand.

The app stack consists of AngularJS, Firebase and Angular Material. Assistance by my Bloc mentor.  

# The Goals
This project was split up into eight different checkpoints, and for brevity, I've combined into four different sections.

### 1. The Timer
Showing a timer on the page and updating it live.

### 2. Taking a break
The primary purpose of the application is to keep track of the time as well as showing it to the end user.
When the timer has been completed the user will be allowed a 5-minute break, and after four completed Pomodoros it turns into a 30-minute break.

### 3. Playing a ding
Since the user will be busy doing other things on the computer having a ding play will help them know when to stop or start working.

### 4. Todo items with Firebase
Keeping track of the items completed.

# Setting and Showing the Timer
Staring at a blank screen I know I need a number and a button to be the core framework to my application.
![](/img/bloctime-timer.png)

{% gist 69ccb5defb2f48a2b2e00edb65c8fb7e %}

Beautiful, right? Now that I had the basic info on the screen I could start getting into the guts of the app. Most of the logic for the program is housed in the Pomodoro.timer function.

{% gist 68ea1922864d2d7d978215cbbcdd0d6f %}

Separating the timerStart and timerEnd make this very simple to read. If the timer is running when the button is clicked stop the timer and vice versa. In these function, we will need to call a function to countdown, set the button text and style accordingly.

{% gist 0d817ebf64284d8b7dbef408e27428fa %}

Last but not least we need to have a timer. You can see that $interval is using a function named countdown and that it's called every 1000 milliseconds (1 second).

{% gist eb5ad5f911c70251fb58be512cbed059 %}

All of this was my first iteration of the application and was able to refactor with the help of my mentor. One of the main refactors was how the button is set in the timerStart/timerEnd functions. Adding a button object that has three options running, stopped, and break.

{% gist 1d4cc2a50b5497f6252fbfea4e3d8e33 %}

This allows for a very simple setButton function that can set the style and text by just passing the button you need.

{% gist 8aeb588cabef157e8e506e1e2b1f18b1 %}

# Taking a Break
After a productive 25 minutes, the timer will automatically switch and start a 5-minute break. The best place for this logic is the countdown function which is called every second.

{% gist d5f5cec08cbf53e81f9e512cdd6f0b38 %}

The logic flows as such:

if the currentTime is greater than 0 keep subtracting 1,

if currentTime is less than or equal to 0 and you're not on break, set the button, currentTime to 5 minutes and onBreak to true,

If currentTime is less than or equal to 0 and you're on a break, set the button, currentTime to 25 minutes and onBreak to false. The $interval.cancel is used because I didn't want the app to go straight into another Pomodoro session.

Now there are some similarities with the timerStart and timerEnd functions setting a variable to either break or session. With the setBreak logic becoming more complicated with the need for an if statement checking if pomodorosCompleted is divisible by 4.

{% gist 69c8ab79794cca62bda0fc9e7e8fcd4a %}

Compared to the old countdown function this is much leaner but still very simple to undersand.
{% gist 3805618ef77f51676be88ec14d6dd776 %}

# Ding! Ding! Ding!
I've used the Buzz library in BlocJams, an AngularJS tutorial at Bloc. The goal of this checkpoint was to alert the user with a ding when their timer had ended. First, we need a Buzz object for the playDing function to use.

{% gist  b3f40a7fe4e4b697252cb125c656a898 %}

We have a Buzz object now we need to create a function that accepts a number and checks if it's equal to zero and if it plays the ding.
{% gist 6d2c6e5178ca3240e7d87fa2e2e24063 %}

The most challenging part of this problem was figuring out how to use the $watch method correctly.

{% gist 5a50d22c0a4cb2acf1f22dcd04ca9a0c %}

The method will put a listener callback on pomo.pomodoro.currentTime to call the function every time the watchExpression changes. Passing the new and old values but we only need to pass the new value into our playDing function.


# Firebase
The real meat and potatoes of the application. I've dealt with databases before during my time with the backend course at Bloc but this is my first time with AngularJS and Firebase. After getting the syntax down, it was fairly simple to use. Abstracting it into a module will allow it to reusable across the application.

{% gist 76e57d88ba93c107a04616f9a7c46c5c %}

Referencing the root of my database with the ref variable, I'm able to turn that information into a JavaScript array with $firebaseArray. AngularFire helps with the $add method in the addTask function.

{% gist 7c81e4dec8a42dd192fe294407bbd0db %}

Inside the main controller, the newTask is set to blank to remove any leftovers from previous sessions. Next, we assign all the tasks to this.tasks; then we use Angular.Copy in the addTask method to copy the object newTask and hen we set the newTask to empty again.

{% gist e2f9fe3e91c928d3f304a64064487a8e %}

In the view the ng-model is set to the newTask and when submitted it calls the addTask function.

# Results

Add tasks and complete Pomodoros to earn breaks.

# Conclusion

The project was very useful in understanding how code should be structured and the balance between readability and going to far abstracting out. If the code becomes too verbose it become the opposite of what is intended.
