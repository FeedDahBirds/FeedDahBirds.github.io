---
layout: post
title:  "Making Mistakes"
date:   2018-10-14 15:22:07 -0400
categories: pokemon ai recognition
---

[test_screen]: https://image.ibb.co/mNvtTp/test-image.png "Bulbasaur"
[other_screen]:https://image.ibb.co/j619jU/caterpie-char-test.png "This one will hurt"
[non_fixed]: https://image.ibb.co/jVBjjU/analysis-non-gray.png "Yep, that's bad"
[fixed]: https://image.ibb.co/crmOdp/analysis-gray-scaled.png "That's better"
[char_gas]: https://image.ibb.co/mQ3NM9/char-gas-breakdown.png "They don't look THAT similar"

[success]:https://image.ibb.co/dV0Vr9/accurate-pokemon.png "WOOOOOOO"
[failure]:https://image.ibb.co/d5W3B9/uh-oh-pokemon.png ":("

# What Happend

Continuing my adventures of building the pokemon battle brain, my initial models to recognize the pokemon were looking great. So I decided to take a screenshot from an emulator running Pokemon Blue and use that as a test image to start building out the recog module of the brain. 

![alt text][test_screen]

After fiddiling around with templating the battle mode screenshot properly, I was finally able to test out my models and start recognizing pokemon directly from the game. Andddddd the results were.... Not great.

I was immediatley greeted with a KeyException error becuase the model predicted my pokemon to be Pokemon 3142.... Which is far far beyond the upperbounds of the original 151 pokemon we should be looking at. I created a basic program to show the images side-by-side and shows the flattened array of data as histogram easily visualize differences.

Results below:

![alt text][non_fixed]

Obviously, a total and complete fail on my part.

As you can see, the sprite I'm testing against is colored and the screenshot is effectivley grey-scaled which is probably the most obvious fail, also the white background is different for both images; in the screenshot the background values drift more towards .9723 with an opacity value of one. In our training sprite, the image background is 1 with an opacity of 0.

So I played around a bit and decided to use the rgb2grey on both to see if that evens anything out.

![alt text][fixed]

That's a lot better.

After retraining the model and adapting the data in my script I then get...

![alt text][success]

which matches our screenshot perfectly!

I took a few more test images, so let's see how they work out...

Let's try this fair-fight between a Caterpie and my Charmander

![alt text][other_screen]

And with that we get.

![alt text][failure]

Well, that's not right. ** sigh **

More research!

![alt text][char_gas]

The general shape of the histograms do appear very similar, but not similar enough that I think it would warrant this kind of error.

# Making More Changes

I think I'm probably making a pretty big mistake using the base SVM provided by scikit learn and trying to regression to the correct value. I could probably attempt a one-hot encoded output and have better results than trying to classify by regression.

Another thing I was thinking about trying is a CNN. CNNs seem to be the best bet for image classification as they do a better job at being spatially aware of the acutal values as opposed to what I'm doing where I fully flatten out the image.

I think I'll make a one-hot encoded attempt to see if that eliminates my current issue, will also still jump into some tensorflow CNN action as the nanodegree course is only two days away and learning about it on my own sooner rather than later may give me the advantage I need to fully understand it.

TTFN.


