---
layout: post
title:  "Collateral Learning"
date:   2016-02-18 20:00:00
category: learning
tags: learning
---

Lately I've been having more free. I try to spend as much of the extra time as possible learning. One of the first things that I've learned is that I get bored soon. I love starting new courses, but once I'm past the introduction it get's hard for me to continue. I want to keep track of what I've been learning before I drop part of it.

### Python

I've had this programming language on mind for a long time. It's known for being a fast option to test and implement your ideas and algorithms, but it also has some machine learning and computer vision libraries that make it an interesting language for me. Also, I think mastering more than one language can help me to learn new ones faster.

I have to admit that I fell in love with this implementation of the quick sort algorithm found [here][quickSort]:

{% highlight python %}
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) / 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)
{% endhighlight %}

There are a lot of resources available on the internet so I won't post any links. The official documentation is [available here][pythonDoc].

### CS231n: Convolutional Neural Networks for Visual Recognition

I've been following [this course][cs231n] since it started some weeks ago. Right now we are past midterm and I'm enjoying it **a lot**. The content fits perfectly with what we are doing at [Sadako][sadako] and the teachers are great.

Something that surprised me is that students at Stanford are very well prepared. I've been working on convNets for almost one year now and some parts of the course are still hard to follow to me, but the students seem to follow every class and always have good questions to ask.

### Jekyll

One of the teachers of the CS231n course has [a blog][aKarpathy] made with jekyll where [he praises about jekyll][aKarpathyJekyll] over Wordpress.

[Jekyll][jekyll] is the blogging platform this blog is made of. I don't have a lot of experience with blogging but once I tried to create a static website with Wordpress and I felt really disappointed about the tool. Reading Andrej complaining about the same problems that I faced made me feel better and encouraged me to give Jekyll an opportunity.

Furthermore, the idea of starting some sort of personal diary has been in my head for a while now and this blog can help that in a way.

### Markdown

Sometimes, when learning something new, is necessary to get introduced to more than one tool simultaneously. I had to learn a little bit about [Markdown][markdown] in order to write this blog. Luckily, it's really easy.

### Git

I've always used [subversion][svn] as my version control software. I unsuccessfully tried to learn git during the final project of my degree. There was a guy at IRI who was known for loving [Git][git] and I asked him to introduce me to this tool. It seems really fast and light, but it's interface seems like a joke to me compared to subversion.

However, I'm starting to see github everywhere lately (even this blog is hosted there), so I think it's time to get used to it.

### Programming patterns

I found [this book][gamePP] by chance on Twitter thanks to [@ciberado][ciberado] and I enjoyed it since the beginning.

Javi Moreno said it was *very well written and designed* and I couldn't agree more. It's not so usual, and a pleasure, to find this level of clarity in a technical book.

### Miscellany

The topics above are all the technical stuff that I'm trying to learn for now, but I've been watching documentaries about other stuff that I also want to mention:

- [Bitcoins](https://bitcoin.org/es/)
- [The salt of the Earth](http://www.imdb.com/title/tt3674140/combined), Sebastiao Salgado is a social photographer with a great empathy.
- [Electric cars](http://www.imdb.com/title/tt0489037/combined), the documentary is optional but it helped me open my eyes about the real problem with electric cars: industry's self interest.
- [Elon Musk](https://www.youtube.com/watch?v=mh45igK4Esw). I like him not only because he is really smart, but because he seems to have so much faith in humanity. He is willing to give up his patents just for humanity to build something useful and good.
- [Food Inc.](http://www.imdb.com/title/tt1286537/combined) is **absolutely mandatory** and reaffirmed me even more about the sickness of this society.

[quickSort]: http://cs231n.github.io/python-numpy-tutorial/
[pythonDoc]: https://www.python.org/
[cs231n]: http://cs231n.stanford.edu/
[sadako]: http://www.sadako.es/
[aKarpathy]: http://karpathy.github.io/
[aKarpathyJekyll]: http://karpathy.github.io/2014/07/01/switching-to-jekyll/
[jekyll]: https://jekyllrb.com/
[markdown]: https://es.wikipedia.org/wiki/Markdown
[svn]: https://en.wikipedia.org/wiki/Apache_Subversion
[git]: https://git-scm.com/
[gamePP]: http://gameprogrammingpatterns.com/
[ciberado]: https://twitter.com/ciberado/status/698150211512889344
