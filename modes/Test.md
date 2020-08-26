[Return to main page](../README.md)

# Test Mode description
This page describes test mode. Test mode allow you to check led state, and created mainly for checking strips and finding led's with problems.

## Default Interaction

When controller loaded you are in `blackout`.
- If you set this mode as default in configuration file `play.default=Test;`, you start in Test Mode.
- If you keep defaults you should press button for 5 seconds and release on white dots with black spaces.

It's actually most updated version to version mode. Order and count of sub-modes may be different.

On any button click you change sub-mode to next.  
You start with `state_off`, or `blackout`.

- State Off
    - Led's are turned off.
- State Data highlight
    - Each strip connected to controller blink in it's color, used to find which line has problems.
- State Full White
    - Used to find power hungry parts of suits, use system brightness (70% by default).
- State Basic Test
    - Led's one by one change color to Red->Green->Blue->...
- State Clouds
    - As we noticed, damaged led's can be detected fast on low brightness white, so we implemented this state to detect damaged led's.
- State Color Scroll
    - Rainbow effect on suit.
- State Bright -> Color
    - Used for engineering purposes.

## Radio Work
- Radio Receive Mode:  
  Suit send to '`Realtime`' log current data received. You can find all data received by setting `log.rt=1;` in config file.
  >**Note:** We don't use Realtime logging by default because of high chance of SD file corruption on power off.
- Radio Send Mode:  
  This mode don't send any data.
  
  Send performed each `radio.sendEach=ms;` where `ms` is 30 by default.

- Radio Group Mode:  
  Same as on Receive Mode.

## Availability
This mode is always available, and if no effects found this mode will be automatically selected.

You can find it at fourth position in que (or lesser if other mods unavailable). Release on white dots with black spaces.

## Technical details
Mode prefix is `[TST]` inside log file.  
If `play.autostart=1;` and mode is set as default, then on load controller will automatically set State Data highlight.