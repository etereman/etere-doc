[Return to main page](../README.md)

# Manual Mode description
This page describes manual mode. Manual mode allow you to change current effect by single click.

## Default Interaction
For example there are 4 effects on SD card.
- `S1.txt`
- `S3.txt`
- `S4_rain.txt`
- `S12.txt`  

When controller loaded you are in `blackout`.  
If you press button 1 (Next button) multiple times
You will change effects in order  
`blackout` \> `S1.txt` \> `S3.txt` \> `S4_rain.txt` \> `S12.txt` \> `blackout` \> `S1.txt` \> ...

If you press button 2 (Prev button) multiple times
You will change effects in order
`blackout` \> `S12.txt` \> `S4_rain.txt` \> `S3.txt` \> `S1.txt` \> `blackout` \> `S12.txt` \> ...

If you perform `reset action` (Keep button pressed for 1.5 sec and releasing) you will automatically switched to blackout state. You can perform reset using any button.

So `S4_rain.txt` \> `blackout` when `reset action` done

## Radio Work
- Radio Receive Mode:  
  Receives one number - `effect id` and instantly set this effect.
  >**Note:** if effect file with required `effect id` not on sd card, controller will keep current state.  
  
  `255` reserved for reset action.
  `0` reserved for ignore.  
  >**Note:** So if controller in this mode, you can use external radio module (like 20 button console or different) to perform effect changing.
- Radio Send Mode:  
  Sends current `effect id`, for `blackout state` sends `255`.
  Send performed each `radio.sendEach=ms;` where `ms` is 30 by default.

- Radio Group Mode:  
  Receives effect number same way as it done in `Radio Receive Mode`, but on user performed action sends `effect id` of current effect multiple times each 7ms for duration `radio.sendEach=ms;`.
  >**Note:** So if controller in this mode, you can use external radio module (like 20 button console or different) to perform effect changing.

## Availability
This mode is default, and always available, you can find it at first position in que. Suit will glow in orange/amber.
>**Note:** if there is no effects detected on SD card, suit will automatically use `Test Mode` instead of Manual.

## Technical details
Mode prefix is `[ M ]` inside log file.  
If `play.autostart=1;` then on load controller will automatically play first effect in list, for current example `S1.txt`.

