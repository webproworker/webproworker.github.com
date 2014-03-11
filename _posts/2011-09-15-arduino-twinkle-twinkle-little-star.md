---
layout: post
title: "Arduino twinkle, twinkle little star"
description: ""
category: Arduino
tags: []
---
{% include JB/setup %}



I recently bought an arduino board and yesterday I took it out for a spin.

They have some thorough tutorials on their website here

I’ve used this tutorial to play some sounds with a piezo speaker and then I’ve changed a little bit the program to play my own baby. Since I don’t know which cord is which note (I suck reading guitar notes) I’ve used this tutorial to find what’s what.

This is [what came out](https://www.youtube.com/watch?v=Jw_qqG_obR8).

And here is the complete source code:

{% highlight ruby %}

   #include "pitches.h"
  // notes in the melody:
  int melody[] = {

      N_B4, N_B4, N_FS5, N_FS5, N_GS5, N_GS5, N_FS5
    , N_NONE
    , N_E5, N_E5, N_DS5, N_DS5, N_CS5, N_CS5, N_B4
    , N_NONE
    , N_FS5, N_FS5, N_E5, N_E5, N_DS5, N_DS5, N_CS5
    , N_NONE
    , N_FS5, N_FS5, N_E5, N_E5, N_DS5, N_DS5, N_DS5, N_DS5, N_CS5
    , N_NONE
    , N_B4, N_B4, N_FS5, N_FS5, N_GS5, N_GS5, N_FS5
    , N_NONE
    , N_E5, N_E5, N_DS5, N_DS5, N_CS5, N_CS5, N_B4
    , N_NONE
    };

  int noteDurations[] = {
     2,2,2,2,2,2,2
    ,2
    ,2,2,2,2,2,2,2
    ,2
    ,2,2,2,2,2,2,2
    ,2
    ,2,2,2,2,4,4,4,4,2
    ,2
    ,2,2,2,2,2,2,2
    ,2
    ,2,2,2,2,2,2,2
    ,2
    };

  void setup() {}

  void playSong(){
    // iterate over the notes of the melody:
    for (int thisNote = 0; thisNote < sizeof(melody)/sizeof(int); thisNote++) {

      // to calculate the note duration, take one second divided by the note type.
      //e.g. half note = 1000 / 2
      int noteDuration = 1000/noteDurations[thisNote];
      tone(8, melody[thisNote],noteDuration);

      delay(noteDuration);
      // stop the tone playing:
      noTone(8);
    }
  }

  void loop() {
    playSong();
    delay(1000);
  }

{% endhighlight %}
