---
layout: post
title: "ScholarInsight: a Chrome browser extension for visualizing Google Scholar profiles" 
date: 2020-12-21
description: a course project for data visualization
tag: data-visualization
giscus_comments: true
related_posts: false
published: true
---

ScholarInsight was my course project for [INST760](https://sites.umiacs.umd.edu/elm/teaching/inst-760-data-visualization/){:target="_blank" rel="noopener"} in 2020 Fall. I deployed it to Google Web Store for a while but took it down after a Chrome update broke my code.
- [video demo](https://youtu.be/7-C5sBFzqNc){:target="_blank" rel="noopener"}
- [GitHub repo](https://github.com/TracyYXChen/ScholarInsight){:target="_blank" rel="noopener"}


## Limitations of Google Scholar
Despite its popularity, Google Scholar provides limited user interactions on scholars’ profile pages: papers can only be sorted by year or citations. While it’s easy for people to identify the most cited or recent papers, it’s hard to spot papers published a few years ago but gradually gained more attraction. Besides, Google Scholar treats all citations from papers of different authorships equally.


## ScholarInsight
At first, I thought about creating interactive data visualizations based on offline datasets. However, citations change every day, so I decided to build an online tool to visualize the latest citation data. Therefore, I created a Chrome web browser extension called ScholarInsight. As a Chrome extension, it can easily be applied to any Google Scholar profile.


Here is what ScholarInsight looks like:


{% include figure.html path="assets/img/scholarInsight/fig1.png" class="img-fluid rounded z-depth-1" %}


ScholarInsight creates a side panel for any Google Scholar profile page. The primary view is a scatter plot, of which the x-axis is the published year and the y-axis is the number of citations. Each colored dot represents a paper: orange denotes first-author papers, blue denotes last-author papers, and grey denotes other papers. ScholarInsight also calculates the author’s median citation for each year and connects them via a dashed line.


ScholarInsight provides several ways for users to interact with:


- **Filter**: users can filter papers by checking/unchecking paper type boxes; currently ScholarInsight supports first-author, last-author, and other types.
- **Scale-transform**: users can choose linear or log-scale for citations
- **Zoom/pan**: users can either click the zoom buttons or use the mouse wheel to zoom; users can also pan the visualization by dragging it.
- **Tooltips**: users can hover on the dot, and click it to show detailed information about that paper.


## Insights uncovered by ScholarInsight


- ### a best paper


Let’s zoom in and take a look at the recent first-author publications of this researcher:
{% include figure.html path="assets/img/scholarInsight/fig2.png" class="img-fluid rounded z-depth-1" %}
a paper published in 2018 that is way above the median citation line (the dot becomes larger because we hovered on it), and if we click it, it’s “Data Illustrator.” The paper won the best paper award of the CHI (Computer-Human Interaction) conference. It is also a popular data visualization authoring tool: Data Illustrator has its own Twitter account and has attracted 1200+ followers.


Sometimes the linear scale doesn’t work well; for example, let’s look at this researcher’s profile:
{% include figure.html path="assets/img/scholarInsight/fig3.png" class="img-fluid rounded z-depth-1" %}
At first glimpse, something went wrong. Why is there only one dot? Oh wait, that’s because one paper has more than 17k citations; thus, all papers are pushed to the bottom. In this case, we can switch to the log-scale.
{% include figure.html path="assets/img/scholarInsight/fig4.png" class="img-fluid rounded z-depth-1" %}


- ### career transitions
If we only look at the first-author and last-author papers, we can see a rough boundary: this researcher started to have last-author papers after 2009. What happened? Well, yes, he became a professor around that time. For productive authors, more than one paper published in the same year may have the same number of citations, so ScholarInsight also jittered the data to avoid visual clutter.


{% include figure.html path="assets/img/scholarInsight/fig5.png" class="img-fluid rounded z-depth-1" %}
Above is the profile of Andrew Ng, a renowned machine-learning researcher. Why has he had many first-author papers in recent years when he’s already a tenured professor? It turned out that he spent more time introducing artificial intelligence to a broader audience, so he started to publish less technical papers, like this one: Artificial intelligence is the new electricity.


## User studies
I conducted informal user study sessions of three female computer science Ph.D. students. In their research fields, authorship order is contribution-based. They applied ScholarInsight to one of the researchers they know and shared their findings. For example, they quickly identified the most-cited first/last-author papers. One participant disappointedly found that although the researcher she chose has hundreds of citations, none are first or last-author papers.


In the future, ScholarInsight could help novice researchers or students who apply for graduate schools.