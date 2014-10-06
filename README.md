## Welcome to Strikingly Interview Test - iOS/Android engineer

# INTRODUCTION

This test is based on the famous Hangman Game. Your task is to write an iOS/Android app to let users play Hangman, guessing words from our server through a REST API.

## Requirements
* Write an iOS/Android app according to the following specifications. Users of your app should be able to play the hangman game, and they should be able to view and submit their scores.
* Email us your finished project! Be sure to include a Readme.txt explaining the cool things you did.
* For this test, please write an iOS/Android app using Objective-C/Java
* You can use ANY libraries relevant for this task.

## Our Expectations
Through this programming test, you should be able to demostrate:

* Good understanding of core iOS/Android concepts and technologies, the Objective-C/Java programming languange, how to construct a UI and how to organize code with MVC pattern
* Code quality and maintainability
* Good programming practices
* Creativity!

## Program Flow and Sepecification
The overall workflow is in 5 stages, namely "Initiate Game", "Give Me A Word", "Make A Guess", "Get Test Results" & "Submit Test Results"

1. **Initiate Game**
  - You send a request to the system to initiate a game
  - Provide a way for the user to start a game in your app
  - The URL of the system could be found in your invitation email
  - Get a secret key from the system and include this key in the following communications. This key identifies the game session!
  - Request & Response
    * Request:
      <pre><code>
      {
        "action" : "initiateGame",
        "userId" : "test@test.com"
      }
      </code></pre>

      Explanation: 
      1. "action" key should have the value for "initiateGame"
      2. "userId" key should be your email address, shown on your invitation email. please contact joyce@strikingly.com if you find your email address is invalid in this programming test.

    * Response:
      <pre><code>
      {
        "message":"Welcome To Strikingly Interview Test!",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F",
        "status":200,
        "data":
        {
          "numberOfWordsToGuess":80,
          "numberOfGuessAllowedForEachWord":10
        }
      }
      </code></pre>

      Explanation: 
      1. "message" always tells you a human readable message. The message in this response is a welcome message.
      2. "userId" shows your email address
      3. "secret" gives you a secret for the following communication. This secret key identifies each game you play.
      4. "status" tells you the status of your request. It follows the standard HTTP Status Code. 200 - OK, 400 - Bad Request, 401 - Unauthenticated
      5. "data" contains some useful data for your reference. In the "initiateGame" response, 
        "numberOfWordsToGuess" - tells you how many words you will have to guess to finish the game.
        "numberOfGuessAllowedForEachWord" - tells you how many INCORRECT guess you may have for each word.

2. **Give Me A Word**
  - After getting the secret key, you can ask the system to give you a word
  - Provide a way for the user to request a new word
  - Remember to include BOTH your "userId" and "secret", and put the correct "action" as "nextWord"
  - In the response you will have "word" in the JSON. The "\*" indicates the characters that you have to guess in a word. The number "\*" in the word key tells you the number of charaters in a word.
  - What kinds of Words will appear in the game? please read [Words](https://github.com/joycehan/strikingly-interview-test-instructions-iOS#words) section 
  - Request & Response
    * Request:
      <pre><code>
      {
        "userId":"test@test.com",
        "action":"nextWord",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F"
      }
      </code></pre>

      Explanation:
      1. "userId" should be your email address
      2. "action" should be "nextWord"
      3. "secret" should be the secret in the reponse from "initateGame"

    * Response:
      <pre><code> 
      {   
        "word":"*****",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F",
        "status":200,
        "data":
          {
            "numberOfWordsTried":1,
            "numberOfGuessAllowedForThisWord":10
          }
      }
      </code></pre>

      Explanation:
      1. "word" contains the "\*". A "\*" sign indicate a missing character in a word
      2. "data" 
        * "numberOfWordsTried" - tells you the number of words that you have tried.
        * "numberOfGuessAllowedForThisWord" - tells you the number of guess you can have on this word. When the number is 0 and you still can't get the correct word, then this word will be counted as incorrect and you have to get a new word.
      3. "userId", "secret", "status" - explained in Response section of "Initiate Game"

3. **Make A Guess**
  - You can make a guess on those characters shown as "*" in a word
  - Your app should have an UI to allow the user to choose the next letter to guess
  - You can only guess ONE character per request, i.e. "guess":"P". **NOTE**: The system will treat two characters or more as WRONG guess.
  - Let's say the word - "*****" is indeed "HAPPY". If the your character does exist in the word, then the response will return you the character in the corresponding positions of the word, i.e. "word": "**PP*"
  - But if your guess is incorrect, then it will return you the same word. Say you make your next guess as "guess":"K" and "K" does NOT exist in "HAPPY", you will get the response as "**PP*"
  - If you get the "word" without any "*", then it means you get the word correct. for instance, "word":"HAPPY". And you can then get a new word by sending "Give A Word" request
  - Note that you are not required to generate guesses automatically, your app receives input from user and then displays the result of the guess
  - Request & Response
    * Request:
      <pre><code>
      {
        "action":"guessWord",
        "guess":"P",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F"
      }
      </code></pre>

      Explanation:
      1. "action" should be "guessWord"
      2. "guess" is the character that you think should exist in the word. IMPORTATN: only accept CAPITAL Letter
      3. "userId" & "secret" - same previouse request. you MUST include these two fields

    * Response:
      <pre><code>
      {
        "word":"**PP*",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F",
        "status":200,
        "data":
        {
          "numberOfWordsTried":1,
          "numberOfGuessAllowedForThisWord":10
        }
      }
      </code></pre>

      Explanation:
      1. "word" is updated to "**PP*" because "P" exist in "HAPPY"
      2. "data" 
        * "numberOfWordsTried" - tells you the number of words that you have tried.
        * "numberOfGuessAllowedForThisWord" - tells you the number of guess you can have on this word. When the number is 0 and you still can't get the correct word, then this word will be counted as incorrect and you have to get a new word.
      3. "userId", "secret", "status" - explained in Response section of "Initiate Game"

4. **Get Test Results**
  - You can get your results only when you have finished guess all the 80 words
  - Show user's test results in your app
  - You get your results by sending a request with "action":"getTestResults"
  - Request & Response:
    * Request:
      <pre><code>
      {
        "action":"getTestResults",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F"
      }
      </code></pre>

      Explanation:
      1. "action" should be "getTestResults"
      3. "userId" & "secret" - same previouse request. you MUST include these two fields
    
    * Response:
      <pre><code>
      {
        "message":"Please check your results! Submit the results if you are happy with it!",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F",
        "status":200,
        "data":
        {
          "numberOfWordsTried":80,
          "numberOfCorrectWords":50,
          "numberOfWrongGuesses":300,
          "totalScore": 700
        }
      }
      </code></pre>

      Explanation:
      1. "message" tells you a readable message which 
      2. "userId", "secret", "status" - explained in Response section of "Initiate Game"
      3. "data" - your results! 
        * "numberOfWordsTried" - the total number of words you tried in this game.
        * "numberOfCorrectWords" - the total number of words you guess correctly 
        * "numberOfWrongGuesses" - the total number of Wrong guess you have made. That is the "guessWord" you made but incorrect.
        * "totalScore" - the total score is calculated. The higher the totalScore, the better results! 
          - totalScore = numberOfCorrectWord * 20 - numberOfWrongGuess. 

5. **Submit Test Results**
  - After getting your results, if users are happy with their results, then they can submit the results
  - To submit the results, you put "action":"submitTestResults"
  - Request & Response
    * Request:
      <pre><code> 
      {
        "action":"submitTestResults",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F"
      }
      </code></pre>

      Explanation:
      1. "action" should be "submitTestResults"
      3. "userId" & "secret" - same previouse request. you MUST include these two fields

    * Response:
      <pre><code> 
      {
        "message":"Thank You! Please paste this JSON and send to joyce[at]strikingly.com",
        "userId":"test@test.com",
        "secret":"2V2GUB00590H33DIEMH24V9V03PU8F",
        "status":200,
        "data":
        {
          "userId":"test@test.com",
          "secret":"2V2GUB00590H33DIEMH24V9V03PU8F",
          "numberOfWordsTried":80,
          "numberOfCorrectWords":50,
          "numberOfWrongGuesses":30,
          "totalScore":700,
          "dateTime":"Fri Aug 02 2013 14:40:39 GMT+0000 (UTC)"
        }
      }
      </code></pre>

      Explanation:
      1. "message" tells you a readable message which 
      2. "userId", "secret", "status" - explained in Response section of "Initiate Game"
      3. "data" - a whole set of data that will be store in our database as your results.

## Deliverables
1. Xcode project
2. If you used any external libraries, make it clear in your submission's readme

## Words
1. Types of Words
  - Plural
  - Tenses
  - Adjectives
2. Difficulty of Words
  - Among the 80 words to guess, there will be in different lengths
    * 1st to 20th word : length <= 5 characters
    * 21st to 40th word : length <= 8 characters
    * 41st to 60th word : length <= 12 characters
    * 61st to 80th word : length > 12 characters

## Http Request Sepecification
1. Request URL: <Please found it in your email address>

2. HTTP Request
  - Method : post
  - Request Body
    <pre><code>
      {
        "key1":"value1",
        "key2":"value2"
      }
    </code></pre>

Sample: 

`curl -X POST -d '{"userId":YOUR_EMAIL, "action":"initiateGame"}' http://strikingly-interview-test.herokuapp.com/guess/process --header "Content-Type:application/json"`
    

## Tips:
Use a Chrome extension to simulate send request and get response in order to familiar with yourself with the flow

## Q&A

### Can I skip a word?

Yes! send another "Give Me A Word" request, i.e. "action":"nextWord"

### Can you submit mulitple results?

Yes and No! You can submit your test results as many times as you want. BUT we ONLY store your LATEST submission.

### Can I always inititate a new game?

Yes! you can always initiate a new game and play the game again and again. BUT always remember to submit your results if you are happy with your performance on a certain game.

### I receive 5xx

We have a lot of applicants playing the game. Unfortunately, our server is not made to handle a lot of load at the same time, so it could timeout. As you are writing a client program, you should handle this case. This is just something you have to deal with when you write production software.

### I receive 401 error

This is because you have a wrong secret key.

### Your hangman server is buggy!

We get this comments sometimes. In most cases, it's because the applicants didn't handle certain cases. Close to a thosand of applicants have played the game with no issue. So please read this document carefully before you jump into this conclusion. No software is bug-free though, so if you have carefully thought about it and still can't proceed, email us.



## Further Questions?
If you have any questions, please write to joyce@strikingly.com. 
In case you want to make queries on any unexpected system error, please send us your HTTP request body and response (if any).

HAVE FUN!


