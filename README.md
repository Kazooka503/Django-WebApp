
![LiveProject Logo](http://www.austinkrzciok.com/img/lp_logo.jpg)

## Introduction:

The Tech Academy Live Project was both an exercise in the Agile/Scrum method of Project Management and a great experience in diving into the inner workings of the Python based web-framework Django. I spent two weeks working with a team of student collegues in developing various web applications to track and catalog a variety of hobbies and interests. My web application was designed as a possible tool for up and coming cannabis industry. It allows you store different cannabis strains to a database with specific data about that strain.

Working with Django on a major project for the first time was an excellent learning opportunity to understand MVT on a deeper level and it's direct relation to a SQL database. Roadblocks came and were overcome. Redesign of the model structure of the database became a necessity to fix bugs. Coming up with solutions for problems was at times frustrating but fullfilling once overcome. 

Here you will find details of the stories I completed with supporting code snippets.


# Project Stories:

## Many-To-Many Model Relationship

A user needed to be able to select multiple options on a web form field in relation to the primary model object they needed to store in the database. This meant the web application I was developing required two seperate tables to be created.  A secondary model become the basis to accomplish this, by using a CharField for the data type inside the second model, I was able to link it to the primary model via a ManyToMany Field.

```from django.db import models


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

    objects = models.Manager()
```

In order to have the correct form choices, the secondary table needed to be seeded with data. I hard coded a data migration file to presupply the necessary data for the form.

```

from __future__ import unicode_literals
from django.db import models, migrations


def load_terpenes(apps, schema_editor):
    Terpene = apps.get_model("ReliefLeafApp", "Terpene")
    terp_limonene = Terpene(id=0, name='Limonene')
    terp_limonene.save()
    terp_myrcene = Terpene(id=1, name='Myrcene')
    terp_myrcene.save()
    terp_pinene = Terpene(id=2, name='Pinene')
    terp_pinene.save()
    terp_terpinolene = Terpene(id=3, name='Terpinolene')
    terp_terpinolene.save()
    terp_caryophyllene = Terpene(id=4, name='Caryophyllene')
    terp_caryophyllene.save()


class Migration(migrations.Migration):

    dependencies = [
        ('ReliefLeafApp', '0025_auto_20200820_2306'),
    ]

    operations = [
        migrations.RunPython(load_terpenes)
    ]


```

## Editing Cataloged Items

In addition to adding new items to the database, a requirement for the project was the ability to edit the information of existing stored objects. I achieved this by creating a view that populated the existing html form page with instance data of a user selected object. 

```
def strain_update_view(request, pk):
    strain = Strain.objects.get(id=pk)
    form = StrainForm(instance=strain)

    if request.method == "POST":
        form = StrainForm(request.POST, instance=strain)
        if form.is_valid():
            form.save()
            return redirect('../../index/')

    context = {'form': form}
    return render(request, 'ReliefLeafApp/ReliefLeafApp_strain_create.html', context)

```

## Bootstrap Styling 

Integrating Bootstrap to work with a Django model forms meant integrating styling into the form class itself. 

### forms.py 

```

class StrainForm(ModelForm):
    class Meta:
        model = Strain
        fields = ('name', 'type', 'terpene', 'percentage', 'description')
        widgets = {
            'name': forms.TextInput(attrs={'class': 'form-control'}),
            'type': forms.RadioSelect(choices=TYPE_CHOICES),
            'terpene': forms.CheckboxSelectMultiple(choices=TERPENE_CHOICES),
            'percentage': forms.NumberInput(attrs={'class': 'form-control'}),
            'description': forms.Textarea(attrs={'class': 'form-control'}),
        }

```

And on the front-end side wrapping the form in a div with the Bootstrap class of "form-group"

### form html 

```

<div class="jumbotron">
        <div class="form-group col-md-12">
        <form action='.' method='POST'>{% csrf_token %}
                <h1>Strain To Save</h1><br />
                {{ form.as_p }}
                <button type="submit" class="btn btn-dark" value='Save' id='strainInput'>Submit</button>
        </form>
        </div>
</div>


```



