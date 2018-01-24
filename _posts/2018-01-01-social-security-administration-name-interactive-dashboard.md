---
layout: post
title: "SSA Interactive Tableau Dashboard and Animated Trend gif"
date: 2018-01-01
---
<p>This is a revisiting to one of my earlier projects using names from the Social Security Administration database but now you can interact by manipulating certain filters such as the year slider or searching for specific names, states, and gender.</p>
<p>Link to my interactive Tableau workbook <a href="https://public.tableau.com/views/Book2_19285/Dashboard?:embed=y&amp;:display_count=yes&amp;publish=yes&amp;:toolbar=no">here</a>.</p>
<p><span style="text-decoration: underline;">How to use and understand dashboard:</span></p>
<p>The top graph below denotes the total count of top name by state on the y-axis with the list of states and territories on the x-axis. The colors correlate to a specific name on the graph and on the geographical map and are not reused for the duration between the years 1910 to 2015. The color denotations correlate to the active gender filter and the composition, e.g., male, female, or both. As you may notice, the animated trends below reuse the same color palette since I animated the map solely by gender exclusivity, i.e., female or male, and not both at once. If you were to toggle both genders, from the top right-side panel, then that would give you the top names by count while ignoring gender and necessitating an expanded color palette to distinguish all the included names.</p>
<p><img src="https://raw.githubusercontent.com/michaelip2/michaelip2.github.io/master/images/1910%20female%20us.png" /></p>
<p><span style="text-decoration: underline;">How to read:</span></p>
<p>Example: The color green represents Helen and during 1910 the female baby name was most popular amongst states such as Washington, South Dakota, Indiana, and Michigan with a recorded count of 156, 60, 249, and 368 respectively.</p>
<p>Furthermore, I made a .gif to show the all correlative popularity changes of a name among several states and it was interesting to see name staples envelope large regions of the country and its inverse as the name staple becomes increasingly uncommon as years progress.</p>
<p><strong>Female name trends from 1910 to 2015</strong>:</p>
<p><img src="https://raw.githubusercontent.com/michaelip2/michaelip2.github.io/master/images/1910%20-%202015%20female%20Name.gif" /></p>
<p><strong>Male name trends from 1910 to 2015</strong>:</p>
<p><img src="https://raw.githubusercontent.com/michaelip2/michaelip2.github.io/master/images/1910%20-%202015%20male%20Name.gif" /></p>
<p><u>Questions concieved after project</u>:</p>
<p>1) What internal or external factors determine the rise and fall of a name's popularity?</p>
<p>An example being are the names Linda and Mary during 1947 for which Linda became the most popular name with an 86% overall country coverage while the name Mary persists with a 14% overall country coverage as represented by the seven orange remaining states.</p>
<p><img src="https://raw.githubusercontent.com/michaelip2/michaelip2.github.io/master/images/1947%20female%20us.png" /></p>
<p>Was there a prominent cultural figure that influenced new and budding parents to consider Linda and Mary among the list of prospective names for their newborn?</p>
<p>Anecdotally I recalled reading this <a href="https://www.huffingtonpost.com/2015/04/08/game-of-thrones-baby-names_n_7018504.html">HuffPost article</a> from 2015 where the HBO's Game of Thrones' meteoric rise to fame coincidentally resulted in 241 babies being named Khaleesi who is one of the pivotal female characters from the medieval fantasy drama. It is amazing and equally odd to witness how our daily hobbies and activities can influence the naming decision for a child.</p>
<p>Another ancillary question to ask is could this behavior be attributed to how a name occasionally becomes popular in certain swaths or regions in a particular year?</p>
<p>2) Can you discern someone's age or guess correctly within a range of years based on the person's birth state?</p>
<p>Complete hypothetical: We can see that Mary was a popular name from 1910 to 1946 so then are you able to successfully assess the age of an elderly woman like one of your next-door neighbors or a friend's grandmother?</p>
