# Seam Carving: Content Aware Image Resizing
Seam-carving is a content-aware image resizing technique where the image is reduced in size by one pixel
of height (or width) at a time. A vertical seam in an image is a path of pixels connected from the top to the
bottom with one pixel in each row; a horizontal seam is a path of pixels connected from the left to the right
with one pixel in each column.

Although the underlying algorithm is simple and elegant, it was not discovered until 2007. Now, it is now a
core feature in Adobe Photoshop and other computer graphics applications. You will be implementing the ACM
Transactions on Graphics 2007 paper titled Seam Carving for Content-Aware Image Resizing. Don’t forget to
check out this YouTube video that shows the results obtained by Shai Avidan and Ariel Shamir. Refer to this
Wikipedia page to get a quick overview.

In this project, I will write an algorithm that resizes a W -by-H image using the seam-carving tech-
nique. Finding and removing a seam involves three parts and a tiny bit of notation. In image processing, pixel
(x, y) refers to the pixel in column x and row y, with pixel (0, 0) at the upper-left corner and pixel (W −1, H −1)
at the lower-right corner. We also assume that the color of each pixel is represented in RGB space, using three
integers between 0 and 255.

#Step 01: Energy Calculation
       The magic is in finding the lowest-energy seam. To do so, we first assign each pixel of the image an energy.
Then, we apply dynamic programming to find the lowest-energy path through the image, an algorithm we’ll
discuss in detail in the next section. First, let’s cover how energy values are assigned to the pixels of the image.
The paper discusses a few different energy functions and the effect they have on resizing. We’ll keep it simple
with an energy function that simply captures how sharply the color in the image changes around each pixel.

       To compute the energy of a single pixel, we look at the pixels to the left and right of that pixel. We find
the squared component-wise distance between them, that is compute the squared difference between the red
components, the squared difference between the green components and the squared difference between blue
components, then add them up. We do the same for the pixels above and below the center pixel. Finally, we
add up the horizontal and vertical distances.


       The only caveat is if a pixel is up against, say, the left edge, there is no pixel to the left. In that case, we
just compare the pixel itself to the pixel to the right. A similar adjustment is made for pixels on the top, right
and bottom edges. This energy function is large when the surrounding pixels are very different in color, and
small when the surrounding pixels are similar.

#Step 02: Seam Identification
With the energy computed for each pixel, we can now look for the lowest-energy seam that goes from the
top of the image down to the bottom. The same analysis applies for horizontal seams going from the left edge
to the right edge, which would allow us to reduce the height of the original image. However, we’ll focus on
vertical seams.

Let’s start by defining the lowest-energy seam:
• A seam is sequence of pixels, exactly one per row. The requirement is that between two consecutive rows,
the x coordinate can only vary by at most one. This keeps the seam connected.
• The lowest-energy seam is the one whose total energy across all the pixels in the seam is minimized.

#Step 03: Seam Removal
The final step is remove from the image all of the pixels along the vertical or horizontal seam. This process
is repeated until the desired number of minimum seams are removed. The energy map needs to be recomputed
every time a seam is removed. Steps 01 - 03 are originally describing reducing the image’s width. The same
code can easily be used for reducing the image’s height by simply taking the transpose of the input image.

