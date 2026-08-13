# Newton's Cradle

**AppADay 098** | Category: Interactive | Shipped 2026-08-13

**Live:** https://augustineiacopelli.github.io/appaday-098-newtons-cradle/

**Portfolio:** https://augustineiacopelli.github.io/appaday/

---

Five brass balls hang on short strings. Pull one aside and let it go, and the ball at the far end leaps out while the four in the middle never stir. Lift two and two answer. Lift three and three answer. It is the oldest demonstration on any executive's desk, and it is doing something genuinely surprising every time.

This is not an animation loop. Every ball is an independent pendulum, integrated at 480 steps per second, and the collisions between them are resolved as real elastic impulses. The momentum you put into one end travels through the line and comes out the other, because that is what the equations do, not because a keyframe says so.

## Using it

Drag any ball aside with a mouse or a finger and release it. A flick carries its speed into the swing. Push a ball into the stack while still holding it and the whole line shoves along instead of passing through.

If you would rather not aim, the release buttons drop one, two, or three balls from the left, or lift both ends at once so the outer pair trade places around a motionless center.

With the cradle focused, the left and right arrow keys lift the ball at that end and the space bar brings everything to rest.

## Controls

**Balls** runs from three to nine. The frame, the strings, and the ball size all resize around whatever you pick.

**Gravity** runs from 0.30 g to 2.20 g. Turn it down and the whole toy moves like it is underwater. Turn it up and the transfers get sharp and quick.

**Energy loss** moves two things together: how much energy each collision keeps, and how much the air steals between swings. At the low end the cradle runs for a long time. At the high end it settles in a few seconds.

**Finish** switches the balls between brass, chrome, and copper.

**Sound** produces a short metallic click on every contact, pitched and scaled to the actual impact speed, so a hard strike sounds different from a glancing one. It can be muted.

**Bring to rest** stops everything and clears the counters.

## The readouts

**Transfers** counts genuine contacts, not frames. A new contact only registers when two balls that were apart come together hard enough to matter.

**Energy kept** is the current total energy of the system as a percentage of its peak since the last release. It is computed live from the kinetic and potential energy of every ball, which is why it holds near 100 percent with energy loss set to none and falls away quickly when it is set high.

**Widest swing** reports the largest angle any ball is currently reaching, in degrees.

## How the physics works

Each ball carries an angle and an angular velocity, and is advanced by the pendulum equation with gravity scaled by the slider. Because a ball hangs from its own pivot and the pivots are spaced one ball diameter apart, two neighbors are overlapping exactly when the sine of the left angle exceeds the sine of the right one. That test drives the contact solver.

Contacts are resolved with ten Gauss-Seidel passes per step. Each pass walks the line, separates any overlapping pair, and exchanges their velocities using the elastic-collision result for equal masses, scaled by the restitution the energy-loss slider implies. Sweeping the chain repeatedly is what lets an impulse at one end arrive at the other end within a single step, which is the whole trick: it produces the correct multi-ball behavior without any special-casing for how many balls were lifted.

A ball being dragged is treated as infinitely massive. Its neighbors bounce off it and get pushed by it, but it does not move except where the pointer puts it.

Ball size is solved from the widest thing that can appear on screen, which is a fully lifted end ball, rather than from the available width. That keeps the string-to-ball proportion identical on a 375px phone and a wide monitor instead of letting a narrow screen squash the strings.

## Built with

One file. Vanilla HTML, CSS, and JavaScript. Canvas 2D for the toy, Web Audio for the clicks, no frameworks, no build step, no dependencies beyond Google Fonts. Fraunces for the display type, IBM Plex Mono for every label and readout. Ball count, gravity, energy loss, finish, and the sound setting persist in localStorage.

## Testing

Forty-seven behavioral tests under jsdom cover momentum transfer for one-, two-, and three-ball drops, energy conservation within three percent at zero loss, nine seconds of nine-ball simulation with no tunneling and no non-finite values, every control, the full drag path, and storage persistence. Visual passes under Playwright at 375x812, 375x667, 768x1024, and 1280x800 confirm no page errors, no horizontal scroll, and no ball rendering outside the canvas at any ball count.

---

*Part of [AppADay](https://augustineiacopelli.github.io/appaday/): one complete web app, every day.*
