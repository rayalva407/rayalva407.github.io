---
layout: post
title:      "Creating An Oscillator With The Web Audio API"
date:       2020-12-17 00:40:36 +0000
permalink:  creating_an_oscillator_with_the_web_audio_api
---


The awesome thing about coding is that there are infinite possibilities. There is something in coding for everyone. One of my passions before I discovered coding was music. So naturally I have always been curious about how to merge these two things together. Recently I have been reading about the Web Audio API. This is a Javascript API that is used for processing and synthesizing audio in web applications. The purpose of this API is to reproduce the features found in most audio production applications but it can be used for other things like video game audio and sound effects. Today I am going to build a simple oscillator using HTML, CSS and Javascript to show what the Web Audio API can do.
First I am just going to create a simple button element inside my HTML’s body so that we can have something to click on and create a sound. We will also have a slider that will increase and decrease the sound’s pitch.
```
<button id="osc"></button>
<input type="range" id="oscPitch" min="50" max="500" step="1" value="90">
```
Very simple! Although not necessary I decided to style it a little bit with CSS.
```
#osc {
  background-color:aqua;
  width: 200px;
  height: 200px;
}
```
After that code this is what the button and slide will look like.
A very simple button
Very simple oscillator button
Great now to the fun part! In order for us to do anything with the Web Audio API we need to create an instance of the AudioContext in our Javascript. This is what gives us all the features and functionality of the API. We are going to create two variables one to create the audio context and one to grab the oscillator button in our HTML.
```
const context = new AudioContext(); //allows access to webaudioapi
const osc = document.querySelector('#osc'); //grabs the button
Next we are going to create a function that will make sound for as long as we hold a button. In order to do that we need to create an oscillator inside the function and assign it a waveform type and frequency. There are four waveform types that most synthesizers use to make sound. They are called the “sine wave”, “triangle wave”, “square/pulse wave” and “saw wave”. These waves have different sonic properties. The saw wave has a harsh tone and the sine wave has more of a mellow tone. Read more about oscillator waveforms here. Let’s go ahead and build this function.
osc.onmousedown = function() {
  let oscPitch = document.querySelector('#oscPitch').value; //assigning the value of the slider to a variable
oscillator = context.createOscillator(); //creates oscillator
oscillator.type = "sine"; //chooses the type of wave
oscillator.frequency.value = oscPitch; //assigning the value of oscPitch to the oscillators frequency value
oscillator.connect(context.destination); //sends to output
oscillator.start(context.currentTime) //starts the sound at the current time
}
```
In this code I used the sine wave but 
You might notice that once it is clicked the sound doesn’t stop. Well we have to create a function that will disconnect the oscillator once the mouse button is lifted. This is a very simple function that looks like this.
```
osc.onmouseup = function() {
  oscillator.disconnect() //disconnects the oscillator
}
```
All done! This is just scratching the surface on what the Web Audio API can do. I will be posting more about the Web Audio API and other audio/programming related things the more I learn about them. Experiment with adding more oscillators! Happy coding!😀
