---
layout: post
title:      "422 Error: the perils of forgetting about authenticity"
date:       2019-03-04 22:11:59 +0000
permalink:  422_error_the_perils_of_forgetting_about_authenticity
---

I just finished working on my Rails with JS Portfolio project, and one of my biggest takeaways is that the project was a test in seeing how Rails and JS could work together (combining server-side functionality, with client-side operations).  What information is accessible where, and how to pass the data back and forth between the two seemed to be the ongoing challenge of the project.  

While the prior project, JS Tic-Tac-Toe with a Rails API had some elements of both JS and Rails, there wasn't much interaction between the two, and as the name suggests it primarily utilized Rails as an API, and didn't have crossover with views/various models, etc.  All-in-all, the JS Tic-Tac-Toe work was a very different experience than this project which truly fused the application of both languages in a variety of manners.

While I still don't find JavaScript as elegant and intuitive as Ruby (or more specifically Rails), I think the OO implementations of ES6 have served to increase my comfort with operating within JavaScript.  My code now seems both better organized and more intuitively aligned to the backend Rails framework.  

Besides the use of the JavaScript Model Objects, I found that increasing my familiarity and use of HTML data attributes to be very helpful to accomplishing coordination between JS and Rails.

However, there was one thing I did encounter that caused me a bit of frustration.  With the use of JS to dynamically cycle between various instances of the models, the url would reflect a different instance than the data that was presented (e.g. a url of categories/5/recommendations/13 though the data presented was of categories/5/recommendations/23).  

This became problematic because of the ability to submit form data on the page.  The Rails side would create an authenticity token assuming that the form related to the displayed url, and when I used AJAX to correct the route/url to post the information, it would reject it with a 422 error and fail to create the new object instance.  While there are likely better solutions (and feel free to let me know about them!), I was able to bypass the authenticity validation for the specific controller action that this related to (e.g.   `skip_before_action :verify_authenticity_token, only: [:create]` ).

I don't propose that this should be utilized as a solution in real-world applications (without implementing some additional security), but I do hope that this experience will help serve as a reminder to others, while you might not think about the "magic" that occurs behind the scenes with Rails (and might not have to that often), you are more likely to encounter issues when you combine programming languages or alter the expected usage. 


