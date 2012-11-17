Fast scalable graphics with SVG mixins
==========

##The problem

Images need to look good on displays of different sizes and densities... with minimal effort.

__Current techniques for high-res/zoomed in displays__ 

- Cater to the *highest* common denominator by serving a pixel-doubled image. The downside of this technique is that most users get more than they need.  

- Serve a *2x image* for iOS devices. This technique requires multiple images for the same presentation, which is more work for designers. And math is hard.


__Techniques for faster/more efficient image serving__

- Use *Sprites* to combine your images into a single image, reducing the number of header calls. Even with help from Compass, this makes changing images a little tricky.

- Encode images as *base64* and include them inline to reduce the number of header calls. Naturally, doing this manually is a chore and would bloat your code with unreadable junk. 

- *Use an icon font* for commonly used icons. Since these are monochromatic, this doesn't work for all of your images. Unless you use a custom font -- also a trick to maintain -- you're still probably serving more information than you need. The markup for this technique is also a bit weird. And finally, you navy have to deal with browser support and image fallbacks.

##The solution: SVG Mixins! 

1. Start with a scalable vector image (think Illustrator, not Photoshop). The 90's are over. 72px is our generation's 78 RPM.

2. Save it as SVG -- with a PNG fallback you have to support IE8 and below. (More on that later.)

3. Create a mixin with an SVG file as as background image. With clever use of *inline-image{}* rather than *image{}*, SASS automatically converts the image to Base64 and places it inline. This means images load with the markup. 


###What does this mean?
![Unscaled](http://cl.ly/image/113o1c2c2y28/Screen%20Shot%202012-11-17%20at%201.57.34%20PM.png)

![Scaled](http://cl.ly/image/1J1t420k1i0j/Screen%20Shot%202012-11-17%20at%201.58.02%20PM.png)


- Scalable graphics that look good at every size on every machine in every browser
- Faster! No additional header calls!
- Easy to maintain! No more sprite files!


##The code
__The Mixin__:

``@mixin spyglass {
 .svg & {
    background-image:  inline-image("img/spyglass-grey.svg");
 }
 background-repeat:  no-repeat;
}``

__The CSS__

``#box {@include spyglass;}``

From here you can use background-size to scale the image. 

This is what is generated:

![Code](http://cl.ly/image/2b3v3x0c2q3j/Screen%20Shot%202012-11-17%20at%202.01.51%20PM.png)

###What about old browsers?
SVG is supported by all modern browsers, including Chrome, Safari, Firefox, and even -- gasp! -- IE9. 

What about IE? Well, including a PNG fallback is easy. 

Simply use the Modernizr script to indicate which image to use. Since SASS can look back UP the DOM tree, our mixin will serve the correct image based on browser support. Best of all, the other image is not included in the final CSS, which is a problem with other fallback techniques.

``@mixin spyglass {
 .svg & {
    background-image:  inline-image("img/spyglass-grey.svg");
 }
 .no-svg & {
    background-image: inline-image("img/spyglass-grey.png");
 }
 background-repeat:  no-repeat;
}
``

The only additional step when building images, is an additional "Save as..." The fallback PNG is still encoded, so there are still speed advantages.




