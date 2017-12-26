# Enterprice Solution Exercise

The exercise is based on an ASP.NET CORE MVC project pluralsight course. Main Topics:

- Attribute Based Routing
  - areas
  - in/outgoing links
- UT
- logging middleware
- filters
  - pipeline with filters   
  - type of filters
- azure application insights
- automating the deployment of the application

**Useful Links**
- https://github.com/rushfive/UnitTestingExample
- https://rushfive.github.io/Start-Unit-Testing-with-xUnit-Moq/

## Personal notes and reflections

Is possible match the code I wrote and the notes by searching for instance "//LD STEP001" 

### Optimizing the Discoverability of Pages Using the Routing Engine

**CONVENTION BASED ROUTES**

the trainer explain the common MVC routing engine that comes as standard, in this case we define the congiguration of the accepted route

it's IMPORTANT to say that for the common configuration, the nos specific routes need to be declared at the top of the list.

       routes.MapRoute(name: "default",template: "{controller}/{action}",
       [optimizing-the-discoverability-of-pages-using-the-routing-engine-slides.pdf](https://trello-attachments.s3.amazonaws.com/579776d92c5f0fd947aff4a8/59c0c2d736d28c7e6364794f/2adc1d55caeb9147bb6fe3cbc0eebe78/optimizing-the-discoverability-of-pages-using-the-routing-engine-slides.pdf) defaults: new {controller="Home", action="Index"});


**ATTRIBUTE ROUTES**

nothing need to be configured at application level

**AREAS**
he shows the common routing setting(the code below, as specific, need to be declared at the top of the list):
      ```
      app.UseMvc(routes =>{routes.MapRoute(name: "areas",template: "
      {area:exists}/{controller=Home}/{action=Index}");
      }
	  ```
 we need to define the ATTRIBUTE in controller:

      Area["areaName"]

he shows the settings to make in "_Layout" to link that specific area.

**OUTGOING LINKS**

 very interesting how to generate links to jump between action and action in the same controller and by making calls from views

- attribute based generation
- action name
- actionresult

+ "Url.Action" useful to create dynamic redirections

      >//LD STEP001 


### Diagnosing Runtime Application Issues**

+ way to diagnostic at runtime the application when in production.

**LOGGING MIDDLEWARE**

      >//LD STEP002

**DIAGNOSTICS MIDDLEWARE**
useful to reporting informations and handling exceptions .
- when we are in "development environment" we can use the 
          ```
          useDeveloperExceptionPage
		  ```
- use a generic error page to handle and show errors from 400 to 600
         ```
         useStatusCodepage
		 ```
- when we run in stage or production, the framework will show a nice exception page, so no implementations details, the developer decide the page to show.
          ```
          useExceptionHandler
		  ```
- when the website is still loading we can use a "welcomPage" where the user will be redirect until the website will be ready.


**FILTERS**
DEFINITION: allow us to add logic on MVC requestes

- Allow us to add logic in MVCs request
  Before or after

- Used often for cross
  cutting concerns
  Avoiding code duplication

- Common usages
  Authorization
  Requiring HTTPS

**REQUEST PIPELINE WITH FILTERS**

When a request is received, it goes in sequence to:

1 - middleware

2 - routing middleware

3 - action

then the action filter is applied

4 - action filter

**TYPES OF FILTERS**
the filters will be called in sequence, the pipeline is that one below in order of prioriry, they all inherit from "IFilterMetadata"

**1 - Authorization**(they only define methods that will be executed before the action method itself)
those methods receive an "AuthorizationFilterContext", in order to be able to dig on property related with the user and be able to write custom logic for authorization.

      >//LD STEP003

**2 - Resource** (like cache, called before Method Binding)

**3 - Action** (those filters can run before of after the action itself execution, they can change the responce value of the action)
are usually used for general purpose, usually for logging.

The methods "OnActionExecuting" and "OnActionExecuted" gets in input the "ActionExecutedContext", the context gives access to:
- Controller
- Result
- ActionArguments

Example of "ActionFilterAttribute"

      >//LD STEP004

**4 - Exception** (tipically applied at global level, here we specify how to handle exceptions)

Example of "ExceptionFilterAttribute", we have to register the exception on action or controller level.

      >//LD STEP005 

**5 - Result** (they can run before or after an action result, they will only run if the action method run successfully)

We can also register a GLOBAL FILTER in "Startup.cs", to the filter will be applied not just at action or controller level but in all the application.

      >//LD STEP006 
            services.AddMvc
                (
                    config => { config.Filters.AddService(typeof(TimerAction)); }
                )
                .AddViewLocalization(
                    LanguageViewLocationExpanderFormat.Suffix,
                    opts => { opts.ResourcesPath = "Resources"; })
                .AddDataAnnotationsLocalization();
       
