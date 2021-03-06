# Boomhauer

_Jeffrey Dexter Boomhauer III, most commonly referred to as Boomhauer, is a fictional character in the animated series
King of the Hill.  He is known for his nearly incomprehensible speech._

![Boomhauer](https://upload.wikimedia.org/wikipedia/en/b/be/Jeff_Boomhauer.png)
[![Boomhauer](http://img.youtube.com/vi/bIaUfBjHjpI/0.jpg)](https://www.youtube.com/watch?v=bIaUfBjHjpI "Boomhauer calls 911")

Boomhauer is a library to simplify writing [AWS Lambda](https://aws.amazon.com/lambda/) functions in
[Clojure](http://clojure.org/) to handle [Alexa](https://developer.amazon.com/public/solutions/alexa) voice requests.

## Latest version

[![Clojars Project](http://clojars.org/com.climate/boomhauer/latest-version.svg )](http://clojars.org/com.climate/boomhauer)
[![Dependencies Status](https://jarkeeper.com/TheClimateCorporation/boomhauer/status.svg)](https://jarkeeper.com/TheClimateCorporation/boomhauer)
[![Build Status](https://travis-ci.org/TheClimateCorporation/boomhauer.svg?branch=master)](https://travis-ci.org/TheClimateCorporation/boomhauer)

## Usage

Boomhauer makes registering `intents` and their handlers pretty easy.
Create a request handler that looks similar to the below code example:

```clojure
(ns your.speechlet-request-handler
  (:gen-class
    :name your.SpeechletRequestHandler
    :extends com.amazon.speech.speechlet.lambda.SpeechletRequestStreamHandler
    :init init
    :constructors {[] [com.amazon.speech.speechlet.Speechlet java.util.Set]})

  (:use [your.custom-intent])

  (:import [com.climate.boomhauer BoomhauerSpeechlet]))

(defn -init []
  [[(BoomhauerSpeechlet.
      {:launch-message "Intro message when skill launches"
       :card-title "Your Skill"}), #{}] nil])
```

Then, create an intent for your skill (that looks something like the
code below):

```clojure
(ns your.intent
  (:require [com.climate.boomhauer.intent-handler :refer [defintent]])
  (:import [com.amazon.speech.speechlet SpeechletResponse]
           [com.amazon.speech.ui PlainTextOutputSpeech]))

(defn- mk-plain-speech [text]
  (doto (PlainTextOutputSpeech.) (.setText text)))

(defn hello-world [session session-map]
  (let [speech (mk-plain-speech "Hello, world!")]
    (SpeechletResponse/newTellResponse speech)))

(defintent :HelloWorldIntent hello-world)
```

## Acknowledgments

Shoutouts to [Mario Aquino](https://github.com/marioaquino), [Jeff Melching](https://github.com/jmelching),
[Tim Palmer](https://github.com/palmertimj), [Robert Grailer](https://github.com/RobertGrailer), and
[Eric Turcotte](https://github.com/ericturcotte) for being major contributors to this project.

## License

Copyright (C) 2016 The Climate Corporation. Distributed under the Apache
License, Version 2.0.  You may not use this library except in compliance with
the License. You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the License for the
specific language governing permissions and limitations under the License.
