# HalloweenFlames
Fun Maker STEM project for kids and their parents

I offer children and adults a folded-paper fortune from a bowl. They take one. The paper has no writing on it. They show it to me, and I tell them "Oh, it must be broken, shall I fix it?" When they say yes I turn and hold the paper to the bottom of a long PVC pipe. I press a button to start an animated flame that goes up and to the sky. While they look upwards I swap the paper for a real fortune paper. The fortune papers have fun sayings like "Oh no! Someone is going to
tell you they like you!"

[![Halloween Flaming Fortunes Fun](https://img.youtube.com/vi/D3yvTOuFSvA/0.jpg)](https://www.youtube.com/watch?v=D3yvTOuFSvA)

## Ingredients needed
- 2 bowls, 1 for the blank papers, 1 for the real fortune papers
- 1 20 foot or longer PVC sprinkler system pipe
- 1 4 foot rebar post to hold the PVC pipe upwards
- 120 or more [WS2812 addressible LEDs](), already mounted to a flexible PC board
- 1 ESP32-S3 development board, I used [Adafruit Feather](https://www.adafruit.com/product/5477)
- 1 Level Shifter, shifts 3.3 v to 5 v to help LEDs, I used [HiLetgo](https://www.amazon.com/gp/product/B07F7W91LC/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1)
- 1 5v Power Supply, I used [HiLetGo](https://www.amazon.com/HiLetgo-Supply-Module-Prototype-Breadboard/dp/B00HJ6AE72/ref=asc_df_B00HJ6AE72)
- 1 Switch Button

## Board Wiring
Wiring enables a button to be sensed on GPIO 12 and a string of WS2812 LEDs on GPIO 14.

![Board wiring 1](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093538.jpg)

![Board wiring 2](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093544.jpg)

![Board wiring 3](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093549.jpg)
![Board wiring 4](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093557.jpg)
