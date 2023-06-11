# AI-Recipe-Maker
Using OpenAI's DALLE and Chat-GPT to create recipes given a list of recipes. Enjoy! For this project, I will be providing a complete walkthrough of the code and how to use it, and eventually I'll get it up on YouTube as well!!!

## Table of Contents
- [AI-Recipe-Maker](#ai-recipe-maker)
  - [Table of Contents](#table-of-contents)
  - [TASK:](#task)
  - [WORKFLOW:](#workflow)
- [Tutorial:](#tutorial)
  - [Set up API key](#set-up-api-key)
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

## Make Python List Compatible with Prompt Engine
Since we have a list, for example [ Tube Screamer, Fender, Gibson ], we can call this g for gear, and then use JOIN to connect this into a string. We will use a delimiter to join/separate the items, say a ‘, ’, then go ahead and JOIN:
**NOTE: WE CANNOT PASS A PYTHON LIST IN AS A PROMPT**

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


