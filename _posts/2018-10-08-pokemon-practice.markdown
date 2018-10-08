---
layout: post
title:  "Pokemon Practice"
date:   2018-10-08 15:22:07 -0400
categories: pokemon brain ai recognition
---

[project]: https://i.postimg.cc/vM1W1Mm3/pokemon_screenshot.png "WHOSE THAT POKEMON?!"

# New-ish Project

So... Started on a project involving Pokemon! The current idea is to create a 'battle brain' that can learn how to battle in the gen-1 (Probably Yellow, specifically)pokemon battle system.
It's a bit of a big task, and I don't have amazing amounts of experience yet, I'm hoping the research will help prep me for the Nanodegree.

# Getting Off The Ground

The first thing I've built is a little data pipeline and image recognizer.

```python
    #whose_that_pokemon.py

    json_file = open('outputs/pokemon_data.json', 'r')
    json_data = json.loads(json_file.read())
    pokemon_by_id = {x['id']: x['name'] for x in json_data}

    pokemon_to_predict = int(sys.argv[-1])
    poke = str(pokemon_to_predict)
    if pokemon_to_predict < 10:
        poke = "00" + poke
    elif pokemon_to_predict >= 10 and pokemon_to_predict < 100:
        poke = "0" + poke

    figure = plt.figure()
    a = figure.add_subplot(1,2,1)
    img1 = mpimg.imread('sprites/Spr_1b_' + poke  + '.png')
    flt1 = img1.flatten()
    plt.imshow(img1)
    a.set_title('Front')
    pokemon_model = joblib.load('outputs/pokemon.mdl')
    prediction_1 = pokemon_model.predict([flt1])[0]

    a = figure.add_subplot(1,2,2)
    img2 = mpimg.imread('sprites/' + str(pokemon_to_predict) + '.png')
    flt2 = img1.flatten()
    plt.imshow(img2)
    a.set_title('Back')
    pokemon_model = joblib.load('outputs/pokemon.mdl')
    prediction_2 = pokemon_model.predict([flt2])[0]

    print('From the front: It\'s ' + pokemon_by_id[prediction_1])
    print('From the back: It\'s ' + pokemon_by_id[prediction_2])
    plt.show()

```

The code is very very basic and uses scikit to do the classification.

This problem is a bit of a weird one as far as I understand ML/AI applications. The scope of data is very limited as there are only two images for every pokemon I have to worry about so it kind of feels like cheating to train the model with my entire dataset, but I feel like for the purpose of what I'm going for, it's probably okay.

I hope to at some point revist this with tensorflow or more advanced frameworks to attempt and apply a CNN to this problem specifically, but even if I don't I think it's liveable in it's current state.

![alt text][project]

As you can see from the code and screenshot above, so far it's only through manual invocation of the script and even then it goes back and loads image data from a file and runs it through our model.

Such is the life of a test script.

# Next Steps

Obviously with where the project is currently at it won't do anything other than kind of tell me what a pokemon is. My next goal is to find someway to get images of the pokemon battle screen to python in order for it to recognize and start piecing together some sort of model to train.

I think I need to think more about how I want to build out the full battle brain. Image recognition in this case feels very easy. But the general idea around 'I'm a machine how do I learn how to battle' feels very complicated and very situational.

Maybe fleshing out the model of avaialable data will shed some light on what I can do and what techniques can be applied.

[The project can be found here](https://github.com/FeedDahBirds/PokeBattleBrain)