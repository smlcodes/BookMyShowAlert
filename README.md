# BookMyShowAlert

We need to use FIREFOX BROWSER

In Firefox we need to **Enable Autoplay**
![image](https://user-images.githubusercontent.com/20472904/159620153-8d586505-2daa-4f99-93d1-07f485571651.png)


Intall **Greasemonkey** – Get this Extension for Firefox (en-US)
![image](https://user-images.githubusercontent.com/20472904/159620056-8e0a4d20-febe-4c21-9f3f-46d3ff71ceca.png)

Now open Booking page of a MOVIE contains Dates & Theretres
![image](https://user-images.githubusercontent.com/20472904/159620379-1a2a2f2a-15d0-4583-a5cb-8dd5162656f7.png)


Click on **Greasemonkey Extension ICON** > **New User Script** : Paste js file data
![image](https://user-images.githubusercontent.com/20472904/159620517-0c5a5028-33ff-416d-8b28-dcc3b12168a9.png)


Paste Below Script & Save
<code>
  // ==UserScript==
// @name         book my show
// @namespace    http://tampermonkey.net/
// @version      0.3
// @description  play music when tickets are available
// @author       preetam
// @match        https://in.bookmyshow.com/buytickets/*
// @grant        none
// ==/UserScript==

//date = simply the day,
//interval = number of min b/w each check (in sec, default = 5min),
//theatres = names of the theatre (should be exact and the numbers can be anything.)
//date and interval are required fields, theatres is optional and can be left empty so that sound will be played even if one theatre is present for that movie.
var date = "25", interval = 5*60;

var theatres = {};

theatres['PVR Forum Sujana Mall: Kukatpally, Hyderabad']=1;
//theatres['Miraj Cinemas: Balanagar'] = 2;
//theatres['Asian CineSquare Multiplex: Uppal']=3;


var audioLink = 'https://quz1yp-a.akamaihd.net/downloads/ringtones/files/mp3/ambulance-sound-27708.mp3';
// window.onload = readDocument;

(function() {
    'use strict';
   //alert('Iam Running ....');
    readDocument();
  	
})();


function readDocument() {
  	var today = new Date();
  	console.log("checked on:" + today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds());
    var x = document.getElementsByClassName("dates-wrapper");
    var curDate = today.getDate();
    if (curDate > date) {
        window.alert("Sorry to break this to you but we cant go back in time\n. Your ship has already sailed");
        return;
    }
  	console.log("1-----------> ");
	
	var datesAvailable = x[0].getElementsByClassName("date-numeric");
	console.log("datesAvailable-----------> "+datesAvailable);
	
	var found = false;
	for (let i = 0; i < datesAvailable.length; i++) {
		var currDate = datesAvailable[i].innerHTML.trim();
		console.log("currDate-----------> "+currDate);
		if (currDate === date) {
			found = true;
		}
	}
	console.log("2-----------> found :"+found);

	if (found){
   
if (Object.keys(theatres).length === 0) {
  		playSound();  		
      	return;
    }

	
	console.log("3-----------> ");
	
	
	var availableTheatresList = document.getElementsByClassName("__venue-name");
   // alert(availableTheatresList);
		searchForTheatre(availableTheatresList);
	} else {
		console.log("Either bookings for your movie is't open yet or this movie is not gonna be there till then.");
        stateChange(-1);
	}
}

function playSound() {
  	console.log("playing the song");
    var audio = new Audio(audioLink);
    
    audio.addEventListener('ended', function() {
      this.currentTime = 0;
      this.play();
  }, false);
	
  audio.play();
  alert("Booking Opend For  "+Object.keys(theatres));
}

function searchForTheatre(availableTheatresList) {
    console.log("what we have: " + availableTheatresList.length);
    console.log("what we want: " + theatres);
  	for (let i = 0; i < availableTheatresList.length; i++) {
    		if (theatres.hasOwnProperty(availableTheatresList[i].text.trim())) {
            console.log("we found it! " + availableTheatresList[i]);
            playSound();
        }
    }
  	stateChange(-1);
}

function stateChange(newState) {
    setTimeout(function () {
        if (newState == -1) {
            location.reload(true);
        }
    }, 1000 * interval);
}

</code>  



