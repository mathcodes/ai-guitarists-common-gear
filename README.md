# AI-Recipe-Maker
Using OpenAI's DALLE and Chat-GPT to create recipes given a list of recipes. Enjoy! For this project, I will be providing a complete walkthrough of the code and how to use it, and eventually I'll get it up on YouTube as well!!!

## Table of Contents
- [AI-Recipe-Maker](#ai-recipe-maker)
  - [Table of Contents](#table-of-contents)
  - [TASK:](#task)
  - [WORKFLOW:](#workflow)
- [Tutorial:](#tutorial)
  - [Set up API key](#set-up-api-key)
  - [Generate a recipe](#generate-a-recipe)
  - [Make Python List Compatible with Prompt Engine](#make-python-list-compatible-with-prompt-engine)
  - [First Line of Prompt:](#first-line-of-prompt)
  - [Extend the prompt](#extend-the-prompt)
  - [Run create\_guitarist\_prompt](#run-create_guitarist_prompt)
  - [Create a function to call the API](#create-a-function-to-call-the-api)

## TASK:
Create a prompt that generates a recipe from a list of ingredients.

## WORKFLOW:
 - User gives list of guitar gear
 - Prompt will ask the completion engine (da-vinci) to return a 3 guitarists who use that gear
 - We then generate an image url for each guitarist

# Tutorial:

## Set up API key
```python
import os import openai
openai.api_key = os.getenv('OPENAI_APIKEY')
```
## Generate a recipe
Define a function `create_dish_prompt`, which will accept a Python list of ingredients, and return the prompt.

```python
def create_guitarist_prompt (list_of_gear):
    return prompt
```

**NOTE: WE CANNOT PASS A PYTHON LIST IN AS A PROMPT**

## Make Python List Compatible with Prompt Engine
Since we have a list, for example [ Tube Screamer, Fender, Gibson ], we can call this g for gear, and then use JOIN to connect this into a string. We will use a delimiter to join/separate the items, say a ‘, ’, then go ahead and JOIN:

```python
g = ['ibanez', 'fender', 'gibson']

', '.join(g)  # 'ibaez, fender, gibson'
```

Now we just have that list of ingredients that was a Python list show up more naturally in our prompt.


## First Line of Prompt:
So let's start off with the first line of the prompt, which I'm just going to ask it to create a detail recipe based only on the following ingredients. So let's have our little f string here. And we'll say prompt is equal to f string literal. And I will say create a detail recipe we notice asking the model for detail helped out a lot and then we'll say based on only the following ingredients. And what's kind of interesting is you can also tell the model to do stuff like oh assume the user has basic items like oil, salt and pepper so you could do that as well.
```python
def create_guitarist_prompt (list_of_gear):
  prompt = f'Find 3 famous guitarists that use the following brands or gear: {", ".join(list_of_gear)}'
  return prompt
```
And then here I'll extend the prompt. Go ahead and concatenate a new string here.

## Extend the prompt
```python
def create_guitarist_prompt (list_of_gear):
  prompt = f'Find 3 famous guitarists that use the following brands or gear: {", ".join(list_of_gear)}\n'\
          +f"Additionally, create cards for each guitarist with their name, image, and a link to their website or a site with their biography."
  return prompt
```

## Run create_guitarist_prompt
And we're going to run this and let's let's make sure this actually works.

```python
guitarists = create_guitarist_prompt(['ibanez', 'fender', 'gibson'])

guitarists
```
```
and actually then print this out:

```python
print(create_guitarist_prompt(['ibanez', 'fender', 'gibson']))
```

## Create a function to call the API

```python
response = openai.Completion.create(
  engine="text-davinci-003",
  prompt=prompt,
  max_tokens=512,
  temperature=0.7,
)

result_text = response['choices'][0]['text']

import re

result_text.split('Guitarist Names:')
```

```python
def extract_names(guitarists):
  return re.finalall('^.*Guitarist Names: .*$',guitarists,re.MULTILINE)[0].strip().split('Guitarist Names: ')[1].split('\n')

extract_names(result_text)
```









05:50
so I'm going to


05:51
say recipe is equal to create this prompt. And now if I asked for the recipe, I have that nice string, so I'm gonna say my prompt is equal to that recipe. Max tokens if you want to be a little longer, we'll say 512 And then temperature. This is where you have to experiment, but you probably want to go a little higher. You definitely don't just want zero otherwise, we'll get the same exact recipe every single time. So we'll go ahead and run this. And now let's see what the response output is. Remember, the response object is this large JSON object so we'll need to say off this response choices. First item because and it's just equal to one and then grab the text off of that. And then I'm going to run this and let's actually print this out. I forgot what I just said. It's actually kind of a weird list of ingredients ham, turkey, eggs, and maybe a high protein meal. But let's see what it says. I'm going to print this out so it's a little easier to read. Okay, am Turkey an egg breakfast sandwich actually sounds pretty good. Two slices of bread ham turkey. Oh, and look at even though salt and pepper that tastes. So even though we didn't explicitly tell the model by the way, you can assume they have these kind of base ingredients. It's probably learn enough from all the recipes read online, that most recipes online actually assume you have some basic level of ingredients like oil, salt, pepper, etc. You'll notice it also assumes you have things like a skillet etc. And then serve and enjoy and you'll notice the recipe title here is probably the most crucial part because we're going to do is later grab that recipe title essentially extracted and then send it to Dali. So how do we do this? We can do this properly, simply through a regular expression. So to do that, there's definitely different ways you could do this. You could also just split on a colon and grab the second item there. But if you want to be really clear, you can do this with a regular expression. So I'm gonna import import our E. Again, lots of different ways you can do this. So let me set this as our result so we'll say result text is equal to this response text. So again, lots of different ways you could do this, you could try to do the following manner. Result text splits on recipe title, colon space, and if you do that, then you should notice right away, maybe you get some new lines and then the first item from until the new line is going to be ham turkey egg breakfast sandwich. So you can try messing around with that. I would actually suggest using regular expressions, because you never know for such a long piece of text, how exactly the formatting is going to be. So we are going to actually just strip what's in front of the recipe title via regular expressions. So let's create a function called extract underscore title and we will pass in the recipe text. And then we are going to call regular expression. Find all and then I'm going to pass in the regular expression that basically grabs all the text in front of that recipe title colon. So if you don't know how regular expressions work, especially just key characters that allow you to maybe ignore all the text up to but then look for basically find the title. And then I'm going to say, dots, asterisk dollar sign. Again, this is just a regular expression code, you don't need to worry about it too much. And then I'm going to call recipe here. And this is on a multi line call. So let's go in and see what happens when I return that and let's experiment with this. I will have to extract the item and then call it as a string. So if I say extract title, and let's say results text, I should get back a regular expression object. Run that and you can see okay, perfect. I got recipe title there. And then I'm going to do a little work here. To grab just the title name. So all we need to do is essentially is grab the first item in this list and then split on recipe title. And from there, it's pretty easy. So we're gonna say grab everything in that list. So the first item in that list and then I'm going to grab actually, let's first strip it. So we will say, strip in case you want to see the result of that. You get back that string, just in case and we will split on recipe title colon that you could do back something that looks like this and we will grab the second item in that list. If you want you can also say space here. And then that should also work in to get rid of the space in front of him. So we're going to then grab minus oops minus one. So essentially, they'll always bribe us that last string, in front of recipe title, lots of different ways you could have done this, but that's essentially the main parts we need to worry about. When it comes to the completion API. It's taking a list of ingredients, get back that recipe and then extract the title. Why do we need the title? Because that's what we're sending to Dolly, which we're going to learn about next. Coming up. See you there.












