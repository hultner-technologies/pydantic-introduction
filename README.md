<p align="center">
  <a href="https://hultner.se/"><img src="https://hultner.se/img/logo/logo_black-01.svg" alt="Hultnér Technologies" align="center" width="200"></a>
</p>
<p align="center">
	<a href="https://hultner.se/" rel="nofollow" class="rich-diff-level-one">Hultnér Technologies AB</a> | <a href="https://twitter.com/ahultner" rel="nofollow" class="rich-diff-level-one">@ahultner</a> | <a href="http://alexander.hultner.se" rel="nofollow" class="rich-diff-level-one">Blog</a> | <a href="https://slides.com/hultner/" rel="nofollow" class="rich-diff-level-one">Slides</a> | <a href="https://www.facebook.com/groups/nordiskpython/" rel="nofollow" class="rich-diff-level-one">Nordisk Python Community</a> | <a href="https://github.com/hultner-technologies/pydantic-introduction/" rel="nofollow" class="rich-diff-level-one">GitHub</a>
	<hr>
</p>

# ⠠⠵ Introduction to pydantic
**Introduction to the Python Pydantic library.**  
Take your data classes to the next level! Showcasing run time type checking, data serialization and deserialization, custom validators and of course data class integration. 
Make it easy to go from standard data classes to pydantic models.

## ⠠⠵ Quick reference

```python
# Minimal usage example
from pydantic import BaseModel, validator, root_validator
from enum import Enum

class Topping(str, Enum):
    mozzarella = 'mozzarella'
    tomato_sauce = 'tomato sauce'
    prosciutto = 'prosciutto'
    basil = 'basil'
    rucola = 'rucola'


class Pizza(BaseModel):
    style: str
    toppings: Tuple[Topping, ...]
    

class BakedPizza(Pizza):
    # For simplicity in the example we use int for temperature
    oven_temperature: int
        
    # A validator looking at a single property
    @validator('style')
    def check_style(cls, style):
        house_styles = ("Napoli", "Roman", "Italian")
        if style not in house_styles:
            raise ValueError(f"We only cook the following styles: {house_styles}, given: {style}")
        return style
    
    # Root validators check the entire model
    @root_validator
    def check_temp(cls, values):
        style, temp = values.get("style"), values.get("oven_temperature")
        
        if style != "Napoli":
            # We don't have any special rules yet for the other styles
            return values

        if 350 <= temp <= 400: 
            # Target temperature 350 - 400°C, ideally around 375°C
            return values

        raise ValueError(f"Napoli pizzas require a oven_temperature in the range of 350 - 400°C, given: {temp}°C")


```

## Talks
### ⠠⠵ Python Pizza, 2020 April 25 (Remote)
**Give your data classes super powers with pydantic**, _25 April 2020, 14:12 UTC_  
The speaker schedule for the next Python Pizza conference is up, and I'm one of the speakers. The conference will be held remotely, [tickets](https://ti.to/acpyss/remote-python-pizza-2020-1) are €30 and all the proceeds will go to [Doctors Without Borders](
https://www.msf.org/)!  


If you or your company can't afford the ticket there's also a [financial aid program](https://docs.google.com/forms/d/e/1FAIpQLSeEzqiE9bTCiM2dOQ9Numku2xJHPJKbRj9cqMGqxSD3KVlxOA/viewform).


<!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/LzNBfPVtrPk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->
[![Speaker at Python Pizza 2020, Alexander Hultnér talks about pydantic](https://i.ytimg.com/vi/LzNBfPVtrPk/maxresdefault.jpg)](https://www.youtube.com/watch?v=LzNBfPVtrPk)

- [Slides](http://slides.com/hultner/python-pizza-2020/#/)
- [Conference site](https://remote.python.pizza)
- [Jupyter Lab Notebook](demo/pydantic.ipynb)
- [Announcement video](https://www.youtube.com/watch?v=LzNBfPVtrPk)

## ⠠⠵ FAQ, pydantic
**Is the pydantic type-checker strict?**  
No, pydantic currently favours parsing and will coerce the type if possible. A [strict-mode](https://github.com/samuelcolvin/pydantic/issues/1098) is being worked on.

**Can pydantics runtime type-checker be used on functions?**  
[Yes](https://pydantic-docs.helpmanual.io/usage/validation_decorator/), through the @validate_arguments decorator. But the feature is at the time of writing still in beta (2020-04-25).

**Do settings managment support `.env`?**  
[Yes](https://pydantic-docs.helpmanual.io/usage/settings/) it does!

**I have a question not covered here, where can I ask it?**  
I'm [@ahultner on twitter](https://twitter.com/ahultner), otherwise you can also email me (address in slides).

## ⠠⠵ Links
- [Pydantic docs](https://pydantic-docs.helpmanual.io)
- [Pydantic Github](https://github.com/samuelcolvin/pydantic/)
- [FastAPI](https://fastapi.tiangolo.com)
- [Dataclasses](https://docs.python.org/3/library/dataclasses.html)
- [NamedTuple](https://docs.python.org/3/library/typing.html#typing.NamedTuple)
