
![LiveProject Logo](http://www.austinkrzciok.com/img/lp_logo.jpg)

## Introduction:

The Tech Academy Live Project was both an exercise in the Agile/Scrum method of Project Management and a great experience in diving into the inner workings of the Python based web-framework Django. I spent two weeks working with a team of student collegues in developing various web applications to track and catalog a variety of hobbies and interests. Working with Django on a major project for the first time was an excellent learning opportunity to understand MVT on a deeper level and it's direct relation to a SQL database. Roadblocks came and were overcome. Redesign of the model structure of the database became a necessity to fix bugs. Coming up with solutions for problems was at times frustrating but fullfilling once overcome. 

Here you will find details of the stories I completed with supporting code snippets.


# Back-End Stories:

## Many-To-Many Model Relationship

A user needed to be able to select multiple options on a web form field in relation to the primary model object they needed to store in the database. This meant the web application I was developing required two seperate tables to be created.  A secondary model become the basis to accomplish this, by using a CharField for the data type inside the second model, I was able to link it to the primary model via a ManyToMany Field.

'''from django.db import models


class Terpene(models.Model):
    name = models.CharField(max_length=20)

    def __str__(self):
        return self.name


class Strain(models.Model):

    def terp_names(self):
        return', '.join([a.name for a in self.terpene.all()])

    name = models.CharField(max_length=20, blank=False)
    type = models.TextField(max_length=20, blank=True, null=True)
    terpene = models.ManyToManyField('Terpene')
    percentage = models.DecimalField(default=0.00, max_digits=1000, decimal_places=1)
    description = models.TextField(blank=False, null=True)

    objects = models.Manager()'''








<!--For the last two weeks of my time at the tech academy, I worked with my peers in a team developing a full scale MVC/MVVM Web Application in C#. Working on a legacy codebase was a great learning oppertunity for fixing bugs, cleaning up code, and adding requested features. There were some big changes that could have been a large time sink, but we used what we had to deliver what was needed on time. I saw how a good developer works with what they have to make a quality product. I worked on several back end stories that I am very proud of. Because much of the site had already been built, there were also a good deal of front end stories and UX improvements that needed to be completed, all of varying degrees of difficulty. Everyone on the team had a chance to work on front end and back end stories. Over the two week sprint I also had the opportunity to work on some other project management and team programming skills that I'm confident I will use again and again on future projects

<!--Below are descriptions of the stories I worked on, along with code snippets and navigation links. I also have some full code files in this repo for the larger functionalities I implemented.


