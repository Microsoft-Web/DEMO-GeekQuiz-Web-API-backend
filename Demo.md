<a name="title" />
# Overview of Web API backend from GeekQuiz #

---
<a name="Overview" />
## Overview ##

This demo builds the Web API backend shown in the GeekQuiz application. It will rely on the QuestionsService and AnswersService for data access, but will be otherwise built completely from scratch.

In this demo you will:

1. Create a new TriviaController.
1. Build the Get method which returns async Task<TriviaQuestion> using QuestionsService.
1. Build the Post method which accepts a TriviaAnswer using AnswersService.
1. Set breakpoints on the Get and Post methods, run the application, and step the methods.

<a id="goals" />
### Goals ###
In this demo, you will see how to:

1. (TODO: Insert goal 1 here)
1. (TODO: Insert goal 2 here)
1. (TODO: Insert goal 3 here)

<a name="technologies" />
### Key Technologies ###

- {TODO: Include technology name here} [here][1]
- {TODO: Include technology name here}
- [{TODO: Include technology name here}][2]

[1]: http://insert_link_to_technology_1_here/
[2]: http://insert_link_to_technology_2_here/

<a name="Setup" />
### Setup and Configuration ###

In order to execute this demo you need to set up your environment.

1. Visual Studio 2013 running

<a name="Demo" />
## Demo ##

This demo is composed of the following segments:

1. [Create the TriviaController](#segment1).
1. [Run the solution](#segment2).

<a name="Segment1" />
### Create the TriviaController ###

1. Right click on the **Controllers** folder and go to **Add/Controller...** in order to create a new **TriviaController**.

	![Creating a new Controller](images/creating-a-new-controller.png?raw=true "Creating a new Controller")

	_Creating a new Controller_

1. In the **Add Scaffold** dialog select the **Web API 2 Controller - Empty** option from the list and click **Ok**

	![Selecting the Web API 2 Controller - Empty option](images/selecting-the-web-api-controller-scaffold.png?raw=true "Selecting the Web API 2 Controller - Empty option")

	_Selecting the Web API 2 Controller - Empty option_

1. In the **Add Controller** dialog, set the Controller name to **TriviaController**.

	![Setting the name to the TriviaController](images/setting-the-name-to-the-triviacontroller.png?raw=true "Setting the name to the TriviaController")

	_Setting the name to the TriviaController_

1. Implement the controller using the following code.

	<!-- mark:3-18 -->
	````C#
    public class TriviaController : ApiController
    {
        private TriviaContext db;
        private QuestionsService questionsService;
        private AnswersService answersService;

        public TriviaController()
        {
            this.db = new TriviaContext();
            this.questionsService = new QuestionsService(db);
            this.answersService = new AnswersService(db);
        }

        protected override void Dispose(bool disposing)
        {
            this.db.Dispose();
            base.Dispose(disposing);
        }
    }
````

1. Add the following using statements.

	<!-- mark:1-2 -->
	````C#
	using GeekQuiz.Models;
	using GeekQuiz.Services;
````

1. Add the following code to create a **Get** action in the **TriviaController**.

	<!-- mark:1-14 -->
	````C#
	public async Task<TriviaQuestion> Get()
	{
		var userId = User.Identity.Name;

		TriviaQuestion nextQuestion =
			await this.questionsService.NextQuestionAsync(userId);

		if (nextQuestion == null)
		{
			throw new HttpResponseException(Request.CreateResponse(HttpStatusCode.NotFound));
		}

		return nextQuestion;
	}
````

1. Resolve the missing _using_ statemets for **Task**.

1. Add the **Post** method below the **Get** method in the **TriviaController**.

	<!-- mark:1-15 -->
	````C#
	public async Task<HttpResponseMessage> Post(TriviaAnswer answer)
    {
        if (ModelState.IsValid)
        {
            answer.UserId = User.Identity.Name;

            var isCorrect = await this.answersService.StoreAsync(answer);

            return Request.CreateResponse(HttpStatusCode.Created, isCorrect);
        }
        else
        {
            return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
        }
    }
````

1. Build the solution.

<a name="Segment2" />
### Run the solution ###

1. Set breakpoints on the first line of the Get and Post methods.

	![Setting the debug breakpoints to the methods](images/setting-the-debug-breakpoints.png?raw=true "Setting the debug breakpoints to the methods")

	_Setting the debug breakpoints to the methods_

1. Debug the application with **F5**.

1. TODO: Step the methods.

---

<a name="summary" />
## Summary ##

By completing this demo you should have:

1. Created a new TriviaController.
1. Built the Get method which returns async Task<TriviaQuestion> using QuestionsService.
1. Built the Post method which accepts a TriviaAnswer using AnswersService.
1. Set breakpoints on the Get and Post methods, ran the application, and stepped the methods.

---