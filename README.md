# Halloween Flames
I do a fun Halloween prank in front of my house. In 2023 I did a fun maker STEM project for kids and their parents.

## How It Works

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
- [FastLED library](https://fastled.io/)

## Board Wiring
Wiring enables a button to be sensed on GPIO 12 and a string of WS2812 LEDs on GPIO 14.

![Board wiring 1](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093538.jpg)

![Board wiring 2](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093544.jpg)

![Board wiring 3](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093549.jpg)
![Board wiring 4](https://github.com/frankcohen/HalloweenFlames/blob/main/IMG20231030093557.jpg)

## Arduino Sketch

```
#include <FastLED.h>

#define LED_PIN     14
#define COLOR_ORDER GRB
#define CHIPSET     WS2812
#define NUM_LEDS    130

#define BRIGHTNESS  200
#define FRAMES_PER_SECOND 60

bool gReverseDirection = false;

CRGB leds[NUM_LEDS];

int buttonState = 0;

#define buttonPin 12

// Array of temperature readings at each simulation cell
  static uint8_t heat[NUM_LEDS];


// Fire2012 with programmable Color Palette
//
// This code is the same fire simulation as the original "Fire2012",
// but each heat cell's temperature is translated to color through a FastLED
// programmable color palette, instead of through the "HeatColor(...)" function.
//
// Four different static color palettes are provided here, plus one dynamic one.
// 
// The three static ones are: 
//   1. the FastLED built-in HeatColors_p -- this is the default, and it looks
//      pretty much exactly like the original Fire2012.
//
//  To use any of the other palettes below, just "uncomment" the corresponding code.
//
//   2. a gradient from black to red to yellow to white, which is
//      visually similar to the HeatColors_p, and helps to illustrate
//      what the 'heat colors' palette is actually doing,
//   3. a similar gradient, but in blue colors rather than red ones,
//      i.e. from black to blue to aqua to white, which results in
//      an "icy blue" fire effect,
//   4. a simplified three-step gradient, from black to red to white, just to show
//      that these gradients need not have four components; two or
//      three are possible, too, even if they don't look quite as nice for fire.
//
// The dynamic palette shows how you can change the basic 'hue' of the
// color palette every time through the loop, producing "rainbow fire".

CRGBPalette16 gPal;

int lastState = HIGH; // the previous state from the input pin
int currentState;     // the current reading from the input pin

void setup() 
{
  Serial.begin(115200);
  delay(2000);
  Serial.println( "HalloweenFire" );
  
  pinMode(buttonPin, INPUT_PULLUP);

  delay(3000); // sanity delay
  FastLED.addLeds<CHIPSET, LED_PIN, COLOR_ORDER>(leds, NUM_LEDS).setCorrection( TypicalLEDStrip );
  FastLED.setBrightness( BRIGHTNESS );

  // This first palette is the basic 'black body radiation' colors,
  // which run from black to red to bright yellow to white.
  gPal = HeatColors_p;
  
  // These are other ways to set up the color palette for the 'fire'.
  // First, a gradient from black to red to yellow to white -- similar to HeatColors_p
  //   gPal = CRGBPalette16( CRGB::Black, CRGB::Red, CRGB::Yellow, CRGB::White);
  
  // Second, this palette is like the heat colors, but blue/aqua instead of red/yellow
  //   gPal = CRGBPalette16( CRGB::Black, CRGB::Blue, CRGB::Aqua,  CRGB::White);
  
  // Third, here's a simpler, three-step gradient, from black to red to white
  //   gPal = CRGBPalette16( CRGB::Black, CRGB::Red, CRGB::White);

}

void loop()
{
  buttonState = digitalRead(buttonPin);

  // check if the pushbutton is pressed. If it is, the buttonState is HIGH:
  if (buttonState == HIGH) {
    Serial.println(  "HIGH" );
    fadeToBlackBy( leds, NUM_LEDS, 10);  
      FastLED.show(); // display this frame

      FastLED.delay(1000 / FRAMES_PER_SECOND);

    for( int i = 0; i < NUM_LEDS; i++) {
      heat[i] = 0;
    }


  } 
  else
  {
    Serial.println(  "LOW" );

  
  // Add entropy to random number generator; we use a lot of it.
  random16_add_entropy( random());

  // Fourth, the most sophisticated: this one sets up a new palette every
  // time through the loop, based on a hue that changes every time.
  // The palette is a gradient from black, to a dark color based on the hue,
  // to a light color based on the hue, to white.
  //
  //   static uint8_t hue = 0;
  //   hue++;
  //   CRGB darkcolor  = CHSV(hue,255,192); // pure hue, three-quarters brightness
  //   CRGB lightcolor = CHSV(hue,128,255); // half 'whitened', full brightness
  //   gPal = CRGBPalette16( CRGB::Black, darkcolor, lightcolor, CRGB::White);


  Fire2012WithPalette(); // run simulation frame, using palette colors
  
  FastLED.show(); // display this frame
  FastLED.delay(1000 / FRAMES_PER_SECOND);

  }
  
}

// Fire2012 by Mark Kriegsman, July 2012
// as part of "Five Elements" shown here: http://youtu.be/knWiGsmgycY
//// 
// This basic one-dimensional 'fire' simulation works roughly as follows:
// There's a underlying array of 'heat' cells, that model the temperature
// at each point along the line.  Every cycle through the simulation, 
// four steps are performed:
//  1) All cells cool down a little bit, losing heat to the air
//  2) The heat from each cell drifts 'up' and diffuses a little
//  3) Sometimes randomly new 'sparks' of heat are added at the bottom
//  4) The heat from each cell is rendered as a color into the leds array
//     The heat-to-color mapping uses a black-body radiation approximation.
//
// Temperature is in arbitrary units from 0 (cold black) to 255 (white hot).
//
// This simulation scales it self a bit depending on NUM_LEDS; it should look
// "OK" on anywhere from 20 to 100 LEDs without too much tweaking. 
//
// I recommend running this simulation at anywhere from 30-100 frames per second,
// meaning an interframe delay of about 10-35 milliseconds.
//
// Looks best on a high-density LED setup (60+ pixels/meter).
//
//
// There are two main parameters you can play with to control the look and
// feel of your fire: COOLING (used in step 1 above), and SPARKING (used
// in step 3 above).
//
// COOLING: How much does the air cool as it rises?
// Less cooling = taller flames.  More cooling = shorter flames.
// Default 55, suggested range 20-100 
#define COOLING  55

// SPARKING: What chance (out of 255) is there that a new spark will be lit?
// Higher chance = more roaring fire.  Lower chance = more flickery fire.
// Default 120, suggested range 50-200.
#define SPARKING 120


void Fire2012WithPalette()
{

  // Step 1.  Cool down every cell a little
    for( int i = 0; i < NUM_LEDS; i++) {
      heat[i] = qsub8( heat[i],  random8(0, ((COOLING * 10) / NUM_LEDS) + 2));
    }
  
    // Step 2.  Heat from each cell drifts 'up' and diffuses a little
    for( int k= NUM_LEDS - 1; k >= 2; k--) {
      heat[k] = (heat[k - 1] + heat[k - 2] + heat[k - 2] ) / 3;
    }
    
    // Step 3.  Randomly ignite new 'sparks' of heat near the bottom
    if( random8() < SPARKING ) {
      int y = random8(7);
      heat[y] = qadd8( heat[y], random8(160,255) );
    }

    // Step 4.  Map from heat cells to LED colors
    for( int j = 0; j < NUM_LEDS; j++) {
      // Scale the heat value from 0-255 down to 0-240
      // for best results with color palettes.
      uint8_t colorindex = scale8( heat[j], 240);
      CRGB color = ColorFromPalette( gPal, colorindex);
      int pixelnumber;
      if( gReverseDirection ) {
        pixelnumber = (NUM_LEDS-1) - j;
      } else {
        pixelnumber = j;
      }
      leds[pixelnumber] = color;
    }
}
```

## Need Help?

Open an [issue](https://github.com/frankcohen/HalloweenFlames/issues) here, or contact me at fcohen at votsh.com.

Hope you enjoy a happy Halloween!
