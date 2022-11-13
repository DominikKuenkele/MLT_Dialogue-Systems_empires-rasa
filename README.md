# empires-rasa
> Part of the Master in Language Technology at the University of Gothenburg
>
> **Course:** Dialogue Systems (LT2216)
>
> **Assignment:** Final project

This project contains the training data and pipelines for a rasa server to classify intents and extract entities from utterances for the game [Empires](https://github.com/DominikKuenkele/empires).
It acts more as a starting point, to handle the basic commands than a complete training data set.


## Setup
For running the server, [Rasa Open Source](https://rasa.com/docs/rasa/installation) needs to be installed.

First, the rasa models need to be trained, by running the following command in the project root:
```bash
rasa train nlu
```

The rasa server can then be started with an enabled API:
```bash
rasa run --enable-api --cors '*'
```

It is now available at `http://localhost:5005`

## Examples
To parse an utterance, one can send POST requests to `http://localhost:5005/model/parse`.
(Full documentation of the API available under [Rasa HTTP API](https://rasa.com/docs/rasa/pages/http-api))

### Attack a unit
**POST body**
```json
{
  "text": "Attack the unit on E4."
}
```

**response**
```json
{
  "text": "Attack the unit on E4.",
  "intent": {
    "name": "attack",
    "confidence": 0.999997615814209
  },
  "entities": [
    {
      "entity": "field",
      "start": 19,
      "end": 21,
      "confidence_entity": 0.9947807192802429,
      "role": "target",
      "confidence_role": 0.9939903020858765,
      "value": "E4",
      "extractor": "DIETClassifier"
    }
  ],
  "text_tokens": [
    [
      0,
      6
    ],
    [
      7,
      10
    ],
    [
      11,
      15
    ],
    [
      16,
      18
    ],
    [
      19,
      21
    ]
  ],
  "intent_ranking": [
    {
      "name": "attack",
      "confidence": 0.999997615814209
    },
    {
      "name": "request_turn",
      "confidence": 8.069031309787533e-7
    },
    {
      "name": "exit_question",
      "confidence": 7.216600579340593e-7
    },
    {
      "name": "produce",
      "confidence": 2.9099672360644036e-7
    },
    {
      "name": "request_unit_move_range",
      "confidence": 1.8774903765006457e-7
    },
    {
      "name": "approve",
      "confidence": 1.3083118233225832e-7
    },
    {
      "name": "skip_round",
      "confidence": 9.403521517015179e-8
    },
    {
      "name": "request_moves",
      "confidence": 8.976755339062947e-8
    },
    {
      "name": "move",
      "confidence": 2.498743079115684e-8
    }
  ]
}
```

### Skip the turn
**POST body**
```json
{
  "text": "I want to do nothing."
}
```

**response**
```json
{
  "text": "I want to do nothing.",
  "intent": {
    "name": "skip_round",
    "confidence": 0.9912384152412415
  },
  "entities": [],
  "text_tokens": [
    [
      0,
      1
    ],
    [
      2,
      6
    ],
    [
      7,
      9
    ],
    [
      10,
      12
    ],
    [
      13,
      20
    ]
  ],
  "intent_ranking": [
    {
      "name": "skip_round",
      "confidence": 0.9912384152412415
    },
    {
      "name": "approve",
      "confidence": 0.0030413952190428972
    },
    {
      "name": "exit_question",
      "confidence": 0.0026366126257926226
    },
    {
      "name": "request_turn",
      "confidence": 0.0016047819517552853
    },
    {
      "name": "request_unit_move_range",
      "confidence": 0.0009069963125512004
    },
    {
      "name": "move",
      "confidence": 0.0002982705773320049
    },
    {
      "name": "produce",
      "confidence": 0.00011910013563465327
    },
    {
      "name": "attack",
      "confidence": 0.0000909798764041625
    },
    {
      "name": "request_moves",
      "confidence": 0.00006337260856525972
    }
  ]
}
```
