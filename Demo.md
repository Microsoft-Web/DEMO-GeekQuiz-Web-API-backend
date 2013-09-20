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

1. Open Visual Studio 2013.
1. Open the **GeekQuiz.sln** solution located under **source\begin**.
1. If you don't have one, create a user account for the application. To do that, press **F5**, click **Register** and provide the information required. After that, close the browser window.

	> **Note:** Remember the information you provided as you will be using it during the demo.

1. In Visual Studio, close all open files.

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

1. Add the Authorize attribute to the TriviaController.

	<!-- mark:3 -->
	````C#
	namespace GeekQuiz.Controllers
	{
		 [Authorize]
		 public class TriviaController : ApiController
		 {
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

1. Resolve the missing _using_ statements for **Task**.

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

1. Set breakpoints on the first line of the **Get** and **Post** methods.

	![Setting the debug breakpoints to the methods](images/setting-the-debug-breakpoints.png?raw=true "Setting the debug breakpoints to the methods")

	_Setting the debug breakpoints to the methods_

1. Debug the application with **F5**.

	> **Note:** If the Log in page is displayed, provide the credentials you created during the setup steps.
	
	> ![Logging in the site](images/logging-in-the-app.png?raw=true "Logging in the site")

1. In Visual Studio, you will see the debugger at the first line of the **Get** method. Continue by stepping over the code (**F10**).

	![Stopping at the first line of the Get method](images/stopping-at-the-first-line-of-get.png?raw=true "Stopping at the first line of the Get method")

	_Stopping at the first line of the Get method_

1. Once you have reached the end of the method, press **F5** and go back to the browser.

	![Retrieving  the question](images/retriving-the-questions.png?raw=true "Retrieving the question")

	_Retrieving the question_

1. Click any of the buttons.

1. In Visual Studio, you will see the debugger at the first line of the **Post** method. Continue by stepping over the code (**F10**).

	![Stopping at the first line of the Post method](images/stopping-at-the-first-line-of-post.png?raw=true "Stopping at the first line of the Post method")

	_Stopping at the first line of the Post method_

---

<a name="summary" />
## Summary ##

By completing this demo you should have:

1. Created a new TriviaController.
1. Built the Get method which returns async Task<TriviaQuestion> using QuestionsService.
1. Built the Post method which accepts a TriviaAnswer using AnswersService.
1. Set breakpoints on the Get and Post methods, ran the application, and stepped the methods.

---